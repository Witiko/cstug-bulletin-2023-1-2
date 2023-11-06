.PHONY: all preview clear FORCE

DOCKER = docker run --rm -u $(shell id -u):$(shell id -g) --env TEXMFVAR=/var/tmp/texmf-var -v "$$PWD":/workdir -w /workdir
IMAGE = texlive/texlive:TL2022-historic-with-cache
LATEXMK = latexmk -xelatex -f
DOCUMENT = main.tex

all: $(basename $(DOCUMENT)).pdf

preview: $(DOCUMENT)
	$(DOCKER) $(IMAGE) $(LATEXMK) -pvc $<

clear: $(DOCUMENT)
	$(DOCKER) $(IMAGE) $(LATEXMK) -C $<

%.pdf: %.tex FORCE
	rm -f $(basename $@).bbl $(basename $@).pax
	$(DOCKER) $(IMAGE) $(LATEXMK) $<
	touch $(basename $@).info
	! grep -F '\csbul@webtrue' $(basename $@).info || $(DOCKER) $(IMAGE) pdfannotextractor $@

# all: main
# 
# iesu:
# 	musixtex iesu.pmx
# 	pdfcrop iesu.pdf iesu.pdf
# 
# main: pmxsonata
# 	vlna main.tex
# 	xelatex main.tex
# 	biber main.bcf
# 	xelatex main.tex
# 	xelatex main.tex
# 
# pmxsonata:
# 	musixtex sonata.pmx
# 	pdfcrop sonata.pdf sonata.pdf
# 
# msonata:
# 	musixtex msonata.tex
# 	pdfcrop msonata.pdf msonata.pdf
# 
# clean: cleanup
# 	rm *.pdf 2>/dev/null; true
# 
# cleanup:
# 	rm *.aux *.bbl *.blg *.log *.run.xml *.mx1 *.mx2 *.bcf *.dat sonata.tex *.ps *.dvi *.out 2>/dev/null; true
