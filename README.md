# Advanced PDF manipulation

## Overview

- There's not a single piece of software to demo, so we're gonna whip through a bunch
- format: start simple, become more codey
- not really an advertisment for coding yourself (but who else is really gonna do it for you?)
- "Hands-on" but you can be more hands on after the class with the code, because sometimes stuff works better outside of the hands-on classes? 
- This was added b/c of desire for more pdf manipulation, but we should do a quick poll--what techniques are people using and what do they wanna get? 


## Outline 

- About PDF format
- OCR'ing
- layout analysis 
- more layout analysis
- more layout analysis

## PDF format

The spec is [here](http://www.adobe.com/content/dam/Adobe/en/devnet/acrobat/pdfs/pdf_reference_1-7.pdf), but we'll oversimplify by saying that the important thing is that it pdf is a display language, so one thing it needs to know is what to render and where. We're gonna be concerned about letters, images, rectangles and lines. 

## Three flavors of pdfs
For our purposes, there are three kinds of PDFs:

- All images. You can't select the text, because there isn't any in there.

- Text-based pdfs. Someone generate the file by exporting it to pdf, and the text can be recovered because it's in there.

- Embedded text pdfs. This is a file with both an image of the file, and text "underneath" it. Basically you see this from OCR'ed scans. 

## Note about paths
This document assumes that the data files are in classdata/sample_pdfs/ . Important reminder before we go any further!! 
#### The file paths in the lab are likely slightly different than what's below any you may have to adjust accordingly. That means instead of typing classdata/sample\_pdfs/ you might have to type classdata/this\_class\_name/sample\_pdfs.

It's also possible the name has a space in it, which is annoying and my fault. You can either *move* the file or make sure you get the spaces right. 

#### PSA: Spaces in file names are almost always a bad idea! Maybe you should move it with something like `mv sample\ pdfs sample_pdfs`

## Look at the metadata. It's not too helpful
Poke at the pdf. There's usually not that much info here. It's interesting to see that Stacy Perrus created this on Wed. Sep2 2015 using Excel 2013. Don't think about this too much--anyone who really wants to remove this from the doc can. 

	$ pdfinfo classdata/sample\ pdfs/2015-2016-dma-ranks.pdf
	Author:         Perrus, Stacy
	Creator:        Microsoft® Excel® 2013
	Producer:       Microsoft® Excel® 2013
	CreationDate:   Wed Sep  2 09:32:02 2015
	ModDate:        Wed Sep  2 09:33:26 2015
	Tagged:         yes
	UserProperties: no
	Suspects:       no
	Form:           none
	JavaScript:     no
	Pages:          6
	Encrypted:      yes (print:yes copy:no change:no addNotes:no algorithm:AES)
	Page size:      612 x 792 pts (letter)
	Page rot:       0
	File size:      58627 bytes
	Optimized:      no
	PDF version:    1.6

## The simple stuff 

Raw file is: (classsata/sample_pdfs/2015-2016-dma-ranks.pdf)[classdata/sample_pdfs/2015-2016-dma-ranks.pdf]

This is an advanced class, you probably already know this, right? 

`$ classdata/sample_pdfs/2015-2016-dma-ranks.pdf classdata/sample_pdfs/2015-2016-dma-ranks-noargs.txt`

No args. Looks like this: 

There are important differences between different versions of pdftotext, see more about it here. 





## OCR: short version

- Overview will give you back searchable pdfs if you upload your files with them. And you probably want to do this anyway, because it rocks. As a side benefit you can just skip ahead to the next section if you do. 

This was talked about a fair amount in the intermediate PDF class and I'm not gonna go into much detail. But as a reference, here's an example. We're not gonna do it. 

#### Preprocessing

This is the simplified version: see full details in the [classdata/sample_pdfs/WFLX](classdata/sample_pdfs/WFLX/README.md) dir.

Tesseract operates on image files, so you'll need to convert pdfs to images first. The simplest way is probably to use imagemagick. For installation see [here](http://www.imagemagick.org/script/binary-releases.php).

Image pre-processing is a dark art and you can spend a lot of time on it. There's actually a script intended for OCR written by the author of imagemagick here: http://www.fmwconcepts.com/imagemagick/textcleaner/index.php. 

#### Make the directory first if it doesn't already exist! : `$mkdir classdata/output` -- or wherever you output your data. 

For [classdata/sample_pdfs/sample\_contract.pdf](classdata/sample_pdfs/WFLX/sample_contract.pdf), convert the first page with: `convert -density 300 ./classdata/sample_pdfs/sample\_contract.pdf ./output/sample_contract_p1.png` ; the result is [here](classdata/output/WFLX/sample\_contract\_p1.png). 

#### Imagemagick has crappy error messages. Instead of saying a file doesn't exist, it says something like this:
	convert: no images defined `./output/sample_contract_p1.png' @ error/convert.c/ConvertImageCommand/3252.

Image pre-processing is a dark art and you can spend a lot of time on it. There's actually a script intended for OCR written by the author of imagemagick here: 
http://www.fmwconcepts.com/imagemagick/textcleaner/index.php

The next step is to turn this into a searchable pdf:

`tesseract  classdata/sample_pdfs/WFLX/sample_contract_p1.png classdata/output/sample_contract_p1_text_added PDF`
 

Read more about quality! [https://github.com/tesseract-ocr/tesseract/wiki/ImproveQuality](https://github.com/tesseract-ocr/tesseract/wiki/ImproveQuality)


# Not going there
 - handwriting 
 - pdfinfo util for pdf metadata (not installed I think)

# General approach

perl's unofficial motto: [there's more than one way to do it](https://en.wikipedia.org/wiki/There%27s_more_than_one_way_to_do_it) . Am gonna follow it here. I'm including examples for PDF2TXT, PDF2XL and Tabula, but since this is a hands-on class I'll leave them as exercises for the reader at home. 

## Tabula-extractor

https://github.com/tabulapdf/tabula-java

https://github.com/chezou/tabula-py

####

## helper tools
test for the existence of these commands on mac / linux
with 
`which pdftk` 



	$ pdftohtml 
	pdftohtml version 0.41.0
	Copyright 2005-2016 The Poppler Developers - http://poppler.freedesktop.org
	Copyright 1999-2003 Gueorgui Ovtcharov and Rainer Dorsch
	Copyright 1996-2011 Glyph & Cog, LLC
	
	Usage: pdftohtml [options] <PDF-file> [<html-file> <xml-file>]
	  -f <int>              : first page to convert
	  -l <int>              : last page to convert
	  -q                    : don't print any messages or errors
	  -h                    : print usage information
	  -?                    : print usage information
	  -help                 : print usage information
	  --help                : print usage information
	  -p                    : exchange .pdf links by .html
	  -c                    : generate complex document
	  -s                    : generate single document that includes all pages
	  -i                    : ignore images
	  -noframes             : generate no frames
	  -stdout               : use standard output
	  -zoom <fp>            : zoom the pdf document (default 1.5)
	  -xml                  : output for XML post-processing
	  -hidden               : output hidden text
	  -nomerge              : do not merge paragraphs
	  -enc <string>         : output text encoding name
	  -fmt <string>         : image file format for Splash output (png or jpg)
	  -v                    : print copyright and version info
	  -opw <string>         : owner password (for encrypted files)
	  -upw <string>         : user password (for encrypted files)
	  -nodrm                : override document DRM settings
	  -wbt <fp>             : word break threshold (default 10 percent)
	  -fontfullname         : outputs font full name

asdfa

`$ pdftohtml classdata/sample\ pdfs/senate_page_sample.pdf -c generated_files/html/senate_page_sample_complex.html`

  

	$ ls -l generated_files/html/senate_page_sample_complex*`
	(pdf17) Jacobs-MacBook-Pro-2:pdf17 jfenton$ ls -l generated_files/html/senate_page_sample_complex*
	-rw-r--r--  1 jfenton  staff  25070 Mar  2 12:57 generated_files/html/senate_page_sample_complex-1.html
	-rw-r--r--  1 jfenton  staff    449 Mar  2 12:57 generated_files/html/senate_page_sample_complex.html
	-rw-r--r--  1 jfenton  staff   3612 Mar  2 12:57 generated_files/html/senate_page_sample_complex001.png
	-rw-r--r--  1 jfenton  staff    212 Mar  2 12:57 generated_files/html/senate_page_sample_complex_ind.html

lkj;lk

	$ head -n 100 generated_files/html/senate_page_sample_complex-1.html | tail -n 3
	<p style="position:absolute;top:306px;left:584px;white-space:nowrap" class="ft01">METAIRIE&#160;TO&#160;COVINGTON&#160;AND&#160;RETURN</p>
	<p style="position:absolute;top:314px;left:919px;white-space:nowrap" class="ft01">25.00</p>
	<p style="position:absolute;top:314px;left:584px;white-space:nowrap" class="ft01">STAFF&#160;PER&#160;DIEM</p>

stopping point -- what we have here are words in space. Actually, this is pretty good, because it found the cells for us. well, probably, one page isn't enough to be sure.



# Other cool stuff I shouldda talked about but didn't

- [Google vision API](https://cloud.google.com/vision/?utm_source=google&utm_medium=cpc&utm_campaign=2015-q1-cloud-na-gcp-skws-freetrial-en&gclid=CNnhpLWNu9ICFVFahgodbpYF1Q) . Note that 'in-situ' extraction is a little different (lots of it is based on stroke-width transform, maybe?)

- "A set of tools for extracting tables from PDF files helping to do data mining on (OCR-processed) scanned documents." https://github.com/WZBSocialScienceCenter/pdftabextract

--> related: https://github.com/WZBSocialScienceCenter/pdf2xml-viewer