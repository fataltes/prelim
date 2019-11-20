OUTPUT_FORMAT = pdf
ALL = pufferfish.$(OUTPUT_FORMAT)
TEXFILES = pufferfish
STYFILES = sprmindx
#our-comments
#CLSFILES = lipics-v2016
#TEXFIGURES = cdbg-rep
BIBFILES = bibliography1 

ifeq ($(OUTPUT_FORMAT),pdf)
	LATEXMKOPTS = -pdf -shell-escape -dvi-
	LATEX = pdflatex
	TEXFIGURE_EXT = .pdf_tex
else ifeq ($(OUTPUT_FORMAT),ps)
	TEXFIGURE_EXT = .eps_tex
	LATEXMKOPTS = -pdfps -dvi-
	LATEX = pslatex
endif


########### Shouldn't require configuration below here ##########

# run "make CONT=1" for continuous compilation
ifdef CONT
	LATEXMKOPTS += -pvc
	FORCED = .PHONY
endif

all: $(ALL)

$(ALL): $(TEXFILES:=.tex) $(STYFILES:=.sty) $(CLSFILES:=.cls) $(TEXFIGURES:=$(TEXFIGURE_EXT)) $(BIBFILES:=.bib)


# How to compile a .tex file to a .pdf file
%.ps %.pdf: %.tex $(FORCED)
	@if (command -v latexmk > /dev/null) ; then latexmk $(LATEXMKOPTS) $< ; \
	else $(LATEX) $<; bibtex $@; $(LATEX) $<; $(LATEX) $<; fi 

#
# Rules for converting various figure formats to pdf
#

%.pdf_tex: %.svg
	inkscape -A $(@:.pdf_tex=.pdf) --export-latex $<

%.pdf: %.svg
	inkscape -A $@ $<

%.eps_tex: %.svg
	inkscape -E $(@:.eps_tex=.eps) --export-latex $<

%.eps: %.svg
	inkscape -E $@ $<

%.pdf: %.dia
	dia --nosplash -e $(@:.pdf=.eps) $(@:.pdf=.dia) 
	epstopdf $(@:.pdf=.eps)

%.pdf: %.fig
	fig2dev -L pdftex $< $@

%.pdf_t: %.fig %.pdf
	fig2dev -L pdftex_t -p $(@:.pdf_t=.pdf) $< $@

%.pstex_t: %.fig
	fig2dev -L pstex_t -p $(@:.pstex_t=.pstex) $< $@
	fig2dev -L pstex $< $(@:.pstex_t=.pstex)

.PHONY:

clean:
	latexmk -C
