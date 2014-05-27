Copy bookmarks to another PDF file
================================

Warning: Still under construction

Adobe Acrobat does not offer the possibility to copy bookmarks from one PDF file to another.
One way to achieve this is to use the free command line tool [PDFtk Server](http://www.pdflabs.com/tools/pdftk-server/).

The procedure is as follows:

   1. Export the metadata (including the bookmarks) from the 'old' file to a text file
   2. Imort the meta data into the the 'new' PDF

###Export

    "C:\Program Files (x86)\PDF Labs\PDFtk Server\bin\pdftk.exe" Old_PDF_file.pdf dump_data output report.txt

###Import

    "C:\Program Files (x86)\PDF Labs\PDFtk Server\bin\pdftk.exe" New_PDF_file.pdf update_info report.txt  output out.pdf 