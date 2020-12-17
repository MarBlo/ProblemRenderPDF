# ProblemRenderPDF


# Problem

When creating a RMarkdown document with the following, standard `yaml` everything works out well and a pdf is produced.

```r
---
title: "Failed Test"
output:
  pdf_document: default
  html_document: default
---

This is going to work
```

If the text contains just one markdown markup like `#` for making a title:

```r
---
title: "Failed Test"
output:
  pdf_document: default
  html_document: default
---

# This is not going to work
```

the following error message is given without further compilation.

```{r}
knitr::include_graphics(here('pics', 'Fehlermeldung.png'))
```


The error message may vary a bit but is often of the type `!undefined control sequence`.

The log file looks like

```python
This is pdfTeX, Version 3.14159265-2.6-1.40.21 (TeX Live 2020) (preloaded format=pdflatex 2020.12.10)  15 DEC 2020 18:42
entering extended mode
 restricted \write18 enabled.
 %&-line parsing enabled.
**NB_one.tex
(./NB_one.tex
LaTeX2e <2020-10-01> patch level 2
L3 programming layer <2020-12-07> xparse <2020-03-03> (/Users/Martin/Library/Ti
nyTeX/texmf-dist/tex/latex/base/minimal.cls
Document Class: minimal 2001/05/25 Standard LaTeX minimal class
)
(/Users/Martin/Library/TinyTeX/texmf-dist/tex/latex/l3backend/l3backend-pdftex.
def
File: l3backend-pdftex.def 2020-09-24 L3 backend support: PDF output (pdfTeX)
\l__kernel_color_stack_int=\count177
\l__pdf_internal_box=\box47
)
No file NB_one.aux.
\openout1 = `NB_one.aux'.

LaTeX Font Info:    Checking defaults for OML/cmm/m/it on input line 2.
LaTeX Font Info:    ... okay on input line 2.
LaTeX Font Info:    Checking defaults for OMS/cmsy/m/n on input line 2.
LaTeX Font Info:    ... okay on input line 2.
LaTeX Font Info:    Checking defaults for OT1/cmr/m/n on input line 2.
LaTeX Font Info:    ... okay on input line 2.
LaTeX Font Info:    Checking defaults for T1/cmr/m/n on input line 2.
LaTeX Font Info:    ... okay on input line 2.
LaTeX Font Info:    Checking defaults for TS1/cmr/m/n on input line 2.
LaTeX Font Info:    ... okay on input line 2.
LaTeX Font Info:    Checking defaults for OMX/cmex/m/n on input line 2.
LaTeX Font Info:    ... okay on input line 2.
LaTeX Font Info:    Checking defaults for U/cmr/m/n on input line 2.
LaTeX Font Info:    ... okay on input line 2.
! Undefined control sequence.
l.4 \hypertarget
                {this-is-not-going-to-work}{% 
Here is how much of TeX's memory you used:
 192 strings out of 481331
 5031 string characters out of 5917010
 264796 words of memory out of 5000000
 17145 multiletter control sequences out of 15000+600000
 403430 words of font info for 27 fonts, out of 8000000 for 9000
 14 hyphenation exceptions out of 8191
 38i,0n,46p,127b,38s stack positions out of 5000i,500n,10000p,200000b,80000s

!  ==> Fatal error occurred, no output PDF file produced!
```

I have done all proposed changes I could find:

+ reinstall `tinytex` with all the miracles of `tinytex::install_tinytex()`  and closing and opening _RStudio_
+ deleted _MacTex_ with _Latexit_ and _TexShop_
+ re-installed it again
  + updated **ALL** _latex_-packages
+ I have made a _tex.file_ with adding `keep_tex=true` out of the _Rmd.file_ and tried to compile this in _TexShop_.
+ I have made the same with with an _md.file_ and tried to compile via pandoc with `pandoc text.md -s -o text.pdf` without any succes. It led to the same error message as from _RStudio_.
+ I upgraded _pandoc_ from something like _2.5_ to now _2.11.2_ without any success.
+ After recognizing that the upgrade of _pandoc_ initially did not lead the system to find the right version, I found out that there was a _Haskell pandoc_ version, which I deleted. The new version could then be found via terminal `pandoc --version` and in _RStudio_ with `rmarkdown::pandoc_version()`.
+ I followed all the recommendations Yihui gave 
+ I posted a question in _RCommunity_ without any reply
+ All relevant _R-packages_ were updated

At latest here I was thinking about the _pandoc-template_ as a possible source of the problem.
And I found this [talk.macpoweruser](https://talk.macpowerusers.com/t/workflow-automatically-convert-markdown-to-pdf-pandoc-latex-hazel/15506/11)

and  this [StackExchange](https://tex.stackexchange.com/questions/257418/error-tightlist-converting-md-file-into-pdf-using-pandoc)

I downloaded [this](https://raw.githubusercontent.com/jgm/pandoc-templates/master/default.latex) new _template_ , stored in the directory where _.Rmd_ was with the name _mytemp.tex_ and changed my _yaml_ to:

```r
---
output: 
  pdf_document:
    template: mytemp.tex
  html_document:
    df_print: paged
---
```


<b> That worked!</b>
