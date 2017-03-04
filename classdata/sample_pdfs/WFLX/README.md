## More complete details on Tesseract processing, extraction

This directory deals with scanning and extracting text information from the sample_contract.pdf file. 

#### Preprocessing

Tesseract operates on image files, so you'll need to convert pdfs to images first. The simplest way is probably to use imagemagick. For installation see [here](http://www.imagemagick.org/script/binary-releases.php) [on mac, try `brew install imagemagick`, assuming you have [homebrew](http://brew.sh/).]
If you get errors about 'no images defined' you may need to also [install ghostscript](http://ghostscript.com/doc/current/Install.htm) [also, `brew install ghostscript`].

For examples/WFLX/sample_contract, convert the first page with: `convert -density 300 ./sample_contract.pdf[1] ./sample_contract_p1.png` ; the result is an image file named [sample_contract_p1.png](sample_contract_p1.png). 

#### Get structured info from that file

Pay attention to tesseract versions. 3.04 is current; 3.01 is needed for bounding box stuff. 

1) Run tesseract with this [config file](configfile), which tells it to output files in hOCR flavor of html. `tesseract sample_contract_p1.png p1_hocr ./configfile` which will output the result to [p1_hocr.hocr](p1_hocr.hocr).

The outputted hocr file reads like this:

	<div class='ocr_page' id='page_1' title='image "sample_contract_p1.png"; bbox 0 0 2550 3300; ppageno 0'><div class='ocr_carea' id='block_1_1' title="bbox 42 72 2386 298"><p class='ocr_par' dir='ltr' id='par_1_1' title="bbox 42 72 2386 298">
     <span class='ocr_line' id='line_1_1' title="bbox 42 72 2386 115; baseline -0.001 -16; x_size 33; x_descenders 7; x_ascenders 6"><span class='ocrx_word' id='word_1_1' title='bbox 42 73 340 115; x_wconf 18' lang='eng' dir='ltr'><strong>ContraCTtrAgreement</strong></span> 
    
This looks a little intimidating, but it's really a regular variant of xml. And we can *see* the bounding boxes in it. 

Jacob hacked up a tool to make sense of it, which you can see [here](https://github.com/jsfenfen/python-hocr). The takeaway here, though is that we can run this command on the raw hocr file like this:

`$ python convert_hocr.py examples/WFLX/p1_hocr.hocr examples/WFLX/p1_hocr.csv`

To output [a csv file](p1_hocr.csv) of words and positions that looks like this:

		pageid,page_dim,text,object_type,height,width,x0,x1,y0,y1
	1,2550x3300,Between:,word,26,132,352,484,3201,3227
	1,2550x3300,Print,word,26,69,1755,1824,3201,3227     



2) *Alternative method that's worth mentioning because it can be used for regular pdfs too.* 

Beginning with Tesseract version 3.03  can turn that image back into a searchable pdf with tesseract using: `tesseract sample_contract_p1.png sample_contract_p1 pdf`. You can that you can spit out bounding boxes from that pdf (or any text-based pdf) with `pdftotext -bbox sample_contract_p1.pdf`. Confusingly, that option ships with the poppler tools version of pdftotext, not the foolabs one. See for more details this [digression on pdftotext](pdftotext_details.md).

The output of this is by default [an .html file](sample_contract_p1.html) that looks like this:

	  <page width="2622.860000" height="3394.290000">
    <word xMin="1805.142000" yMin="83.357050" xMax="1876.113600" yMax="100.391058">Print</word>
    <word xMin="1886.399000" yMin="83.275793" xMax="1956.342440" yMax="100.309801">Date</word>


Processing this data--it's xml--isn't that hard; will have code for it later on. 