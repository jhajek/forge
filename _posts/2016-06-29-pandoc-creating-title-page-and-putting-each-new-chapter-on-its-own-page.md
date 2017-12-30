---
layout: post
status: publish
published: true
title: Pandoc error - Argument of \Paragraph has an extra }
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1812
wordpress_url: https://forge.sat.iit.edu/?p=1812
date: '2016-06-29 10:30:49 -0500'
date_gmt: '2016-06-29 15:30:49 -0500'
categories:
- Pandoc
tags:
- textbook
- pandoc
- pdf
comments: []
---
I am currently writing a textbook.  In doing so I choose to use [Markdown](https://daringfireball.net/projects/markdown/ "Markdown history").  [The book is located on Github](https://github.com/jhajek/Linux-text-book-part-1 "Link to Github for textbook") you can download this and build the book yourself. This book is being used for a class is hard enough, I am using  a flavor of Markdown called [Pandoc](https://www.pandoc.org "pandoc") for my choice to produce PDF, ePub, html, docx, and mobi (Kindle)

There are differences and trade offs trying to take a common markdown code base and publish it to different formats.  Currently I don't have a feature parity between both formats they both have extra items the other one doesn't. 

Recently I reinstalled Pandoc with [MikTeX for Windows](http://miktex.org/ "Miktex") and [BasicTeX for Mac](https://www.tug.org/mactex/morepackages.html "BasicTeX"), and a [deb package for texlive](http://tex.stackexchange.com/questions/28528/best-way-to-install-packages-for-texlive-in-ubuntu "deb package for texlive") for Ubuntu 16.04.  These packages are needed to turn Pandoc markdown into PDFs via a LateX library.

Installing the latest version of Pandoc 1.17.1 and running my build script to produce a PDF all of a sudden generated this error... 

![*Pandoc Error*](/assets/2016/06/pandoc-error.png "pandoc-error")
Oh no... this error is not very helpful as I have 13 chapters worth of material, what could be causing this error?  It could be anywhere!   I did what any good detective did, I removed each chapter 1 by 1 and rebuilt the PDF trying to narrow down where the error is by process of elimination.

I went all the way down - stripping all the chapters out but one.  That was the title page.  The file titlesec.tex is a LaTeX style formatting command to generate a title page and also make sure that each new chapter starts on its own page.  This worked for the entire last year using this command line option:

Here is the full code that throws the error:
```pandoc --toc -V geometry:margin=1in --number-sections --include-in-header ./title/titlesec.tex -s -o ./output/pdf/Understanding-the-Technology-and-Philosophy-of-Linux-Part-I-$STAMP.pdf  ./Chapter-01/chapter-01.md ./Chapter-02/chapter-02.md ./Chapter-03/chapter-03.md ./Chapter-04/chapter-04.md ./Chapter-05/chapter-05.md ./Chapter-06/chapter-06.md ./Chapter-07/chapter-07.md ./Chapter-08/chapter-08.md ./Chapter-09/chapter-09.md ./Chapter-10/chapter-10.md ./Chapter-11/chapter-11.md ./Chapter-12/chapter-12.md ./Chapter-13/chapter-13.md ./Chapter-14/chapter-14.md ./Chapter-15/chapter-15.md ./Appendix-A/Appendix-A.md ./Appendix-B/Appendix-B.md ./Appendix-C/Appendix-C.md```

Strange...  The quick fix for this error was to remove the offending line of code:

```--include-in-header ./title/titlesec.tex```

Replacing it with this line:

```-V documentclass=report```

Which then generates the expected PDF -- compare the difference below:
![*Pandoc Broke*](/assets/2016/06/pandoc-broke.png "pandoc-broke")

![*Pandoc Fixed*](/assets/2016/06/pandoc-fix.png "pandoc-fix")

![*Pandoc Success*](/assets/2016/06/pandoc-success.png "pandoc-success")
