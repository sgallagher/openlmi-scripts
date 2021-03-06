# Makefile for Sphinx documentation
#

# You can set these variables from the command line.
SPHINXOPTS        =  
SPHINXBUILD       = sphinx-build
PAPER             =  
BUILDDIR         ?= _build
PROJECTNAME       = OpenLMIScripts
COMMANDS_PATH    ?= ../commands
INCLUDE_COMMANDS ?= 0
COMMANDS         ?= logicalfile service software storage

# Internal variables.
PAPEROPT_a4     = -D latex_paper_size=a4
PAPEROPT_letter = -D latex_paper_size=letter
ALLSPHINXOPTS   = -n -d $(BUILDDIR)/doctrees $(PAPEROPT_$(PAPER)) $(SPHINXOPTS) .
# the i18n builder cannot share the environment and doctrees with the others
I18NSPHINXOPTS  = $(PAPEROPT_$(PAPER)) $(SPHINXOPTS) .

.PHONY: help clean html dirhtml singlehtml pickle json htmlhelp qthelp \
    devhelp epub latex latexpdf text man changes linkcheck doctest gettext \
    commands.rst

all: html epub latexpdf

help:
	@echo "Please use \`make <target>' where <target> is one of"
	@echo "  html       to make standalone HTML files"
	@echo "  dirhtml    to make HTML files named index.html in directories"
	@echo "  singlehtml to make a single large HTML file"
	@echo "  pickle     to make pickle files"
	@echo "  json       to make JSON files"
	@echo "  htmlhelp   to make HTML files and a HTML help project"
	@echo "  qthelp     to make HTML files and a qthelp project"
	@echo "  devhelp    to make HTML files and a Devhelp project"
	@echo "  epub       to make an epub"
	@echo "  latex      to make LaTeX files, you can set PAPER=a4 or PAPER=letter"
	@echo "  latexpdf   to make LaTeX files and run them through pdflatex"
	@echo "  text       to make text files"
	@echo "  man        to make manual pages"
	@echo "  texinfo    to make Texinfo files"
	@echo "  info       to make Texinfo files and run them through makeinfo"
	@echo "  gettext    to make PO message catalogs"
	@echo "  changes    to make an overview of all changed/added/deprecated items"
	@echo "  linkcheck  to check all external links for integrity"
	@echo "  doctest    to run all doctests embedded in the documentation (if enabled)"

api.rst: gendoc.sh $(wildcard ../lmi/scripts/*.py \
	    ../lmi/scripts/common/*.py \
	    ../lmi/scripts/_metacommand/*.py \
	    ../lmi/scripts/formatter/*.py)
	./gendoc.sh

commands.rst:
	echo "Command libraries documentation" > $@
	echo "===============================" >> $@
ifneq ($(INCLUDE_COMMANDS), 0)
	-[ -e commands ] && rm -rf commands
	mkdir commands
	echo "Usage and developer documentations of provider libraries" >> $@
	echo "(also called *commands*.)" >> $@
	printf "\n.. toctree::\n" >> $@
	printf "    :maxdepth: 2\n\n" >> $@
	for cmd in $(COMMANDS); do \
	    printf "    commands/$$cmd\n" >> $@; \
	    ln -s ../$(COMMANDS_PATH)/$${cmd}/doc commands/$${cmd}_docdir; \
	    title="$${cmd^} library"; \
	    length=`echo "$$title" | wc -c`; \
	    ( \
		printf "$${title}\n"; \
		printf -- "!%.0s" `seq 2 $$length`; \
		printf "\n\n"; \
		if [ -e commands/$${cmd}_docdir/cmdline.rst ]; then \
		    sed "s,\(\.\.\s\+include::\s*\)\(.*\),\1$${cmd}_docdir/\2," \
			commands/$${cmd}_docdir/cmdline.rst \
			>commands/$${cmd}_cmdline.txt; \
		    printf ".. include:: $${cmd}_cmdline.txt\n\n"; \
		fi; \
		printf    "$${cmd^} library reference\n"; \
		printf -- "==========================\n"; \
		if [ -e commands/$${cmd}_docdir/python.rst ]; then \
		    printf ".. toctree::\n    :maxdepth: 2\n\n"; \
		    printf "    $${cmd}_docdir/python\n\n"; \
		else \
		    printf ".. automodule:: lmi.scripts.$${cmd}\n"; \
		    printf "    :members:\n"; \
		fi; \
	    ) > commands/$${cmd}.rst; \
	done
else
	( \
	printf "You may visit command usage and developer documentations\n"; \
	printf "on pythonhosted site:\n\n"; \
	for cmd in $(COMMANDS); do \
	    printf "    * \`openlmi-scripts-$${cmd}"; \
	    printf " <http://pythonhosted.org/openlmi-scripts-$${cmd}>\`_\n"; \
	done \
	) >> $@
endif


clean:
	-rm -rf $(BUILDDIR)/* modules/ api.rst commands.rst commands/
ifneq ($(INCLUDE_COMMANDS), 0)
	for cmd in $(COMMANDS); do \
	    make -C $(COMMANDS_PATH)/$${cmd}/doc clean; \
	done
endif

%.png: %.dia
	dia -e $@ $<

%.svg: %.uml plantuml.cfg
	plantuml -config plantuml.cfg -Tsvg $<

figures:


html: figures api.rst commands.rst
	./build-with-sphinx.sh "$(COMMANDS)" -b html $(ALLSPHINXOPTS) $(BUILDDIR)/html
	@echo
	@echo "Build finished. The HTML pages are in $(BUILDDIR)/html."

dirhtml:
	$(SPHINXBUILD) -b dirhtml $(ALLSPHINXOPTS) $(BUILDDIR)/dirhtml
	@echo
	@echo "Build finished. The HTML pages are in $(BUILDDIR)/dirhtml."

singlehtml:
	$(SPHINXBUILD) -b singlehtml $(ALLSPHINXOPTS) $(BUILDDIR)/singlehtml
	@echo
	@echo "Build finished. The HTML page is in $(BUILDDIR)/singlehtml."

pickle:
	$(SPHINXBUILD) -b pickle $(ALLSPHINXOPTS) $(BUILDDIR)/pickle
	@echo
	@echo "Build finished; now you can process the pickle files."

json:
	$(SPHINXBUILD) -b json $(ALLSPHINXOPTS) $(BUILDDIR)/json
	@echo
	@echo "Build finished; now you can process the JSON files."

htmlhelp:
	$(SPHINXBUILD) -b htmlhelp $(ALLSPHINXOPTS) $(BUILDDIR)/htmlhelp
	@echo
	@echo "Build finished; now you can run HTML Help Workshop with the" \
	      ".hhp project file in $(BUILDDIR)/htmlhelp."

qthelp:
	$(SPHINXBUILD) -b qthelp $(ALLSPHINXOPTS) $(BUILDDIR)/qthelp
	@echo
	@echo "Build finished; now you can run "qcollectiongenerator" with the" \
	      ".qhcp project file in $(BUILDDIR)/qthelp, like this:"
	@echo "# qcollectiongenerator $(BUILDDIR)/qthelp/AnacondaStorageProvider.qhcp"
	@echo "To view the help file:"
	@echo "# assistant -collectionFile $(BUILDDIR)/qthelp/AnacondaStorageProvider.qhc"

devhelp:
	$(SPHINXBUILD) -b devhelp $(ALLSPHINXOPTS) $(BUILDDIR)/devhelp
	@echo
	@echo "Build finished."
	@echo "To view the help file:"
	@echo "# mkdir -p $$HOME/.local/share/devhelp/AnacondaStorageProvider"
	@echo "# ln -s $(BUILDDIR)/devhelp $$HOME/.local/share/devhelp/AnacondaStorageProvider"
	@echo "# devhelp"

epub: figures api.rst commands.rst
	$(SPHINXBUILD) -b epub $(ALLSPHINXOPTS) $(BUILDDIR)/epub
	@echo
	@echo "Build finished. The epub file is in $(BUILDDIR)/epub."

latex: figures api.rst commands.rst
	$(SPHINXBUILD) -b latex $(ALLSPHINXOPTS) $(BUILDDIR)/latex
	sed -i 's/^\\makeindex$$/\0[columns=1]/' $(BUILDDIR)/latex/$(PROJECTNAME).tex
	@echo
	@echo "Build finished; the LaTeX files are in $(BUILDDIR)/latex."
	@echo "Run \`make' in that directory to run these through (pdf)latex" \
	      "(use \`make latexpdf' here to do that automatically)."

latexpdf: latex
	$(MAKE) -C $(BUILDDIR)/latex all-pdf
	@echo "pdflatex finished; the PDF files are in $(BUILDDIR)/latex."

text:
	$(SPHINXBUILD) -b text $(ALLSPHINXOPTS) $(BUILDDIR)/text
	@echo
	@echo "Build finished. The text files are in $(BUILDDIR)/text."

man:
	$(SPHINXBUILD) -b man $(ALLSPHINXOPTS) $(BUILDDIR)/man
	@echo
	@echo "Build finished. The manual pages are in $(BUILDDIR)/man."

texinfo:
	$(SPHINXBUILD) -b texinfo $(ALLSPHINXOPTS) $(BUILDDIR)/texinfo
	@echo
	@echo "Build finished. The Texinfo files are in $(BUILDDIR)/texinfo."
	@echo "Run \`make' in that directory to run these through makeinfo" \
	      "(use \`make info' here to do that automatically)."

info:
	$(SPHINXBUILD) -b texinfo $(ALLSPHINXOPTS) $(BUILDDIR)/texinfo
	@echo "Running Texinfo files through makeinfo..."
	make -C $(BUILDDIR)/texinfo info
	@echo "makeinfo finished; the Info files are in $(BUILDDIR)/texinfo."

gettext:
	$(SPHINXBUILD) -b gettext $(I18NSPHINXOPTS) $(BUILDDIR)/locale
	@echo
	@echo "Build finished. The message catalogs are in $(BUILDDIR)/locale."

changes:
	$(SPHINXBUILD) -b changes $(ALLSPHINXOPTS) $(BUILDDIR)/changes
	@echo
	@echo "The overview file is in $(BUILDDIR)/changes."

linkcheck:
	$(SPHINXBUILD) -b linkcheck $(ALLSPHINXOPTS) $(BUILDDIR)/linkcheck
	@echo
	@echo "Link check complete; look for any errors in the above output " \
	      "or in $(BUILDDIR)/linkcheck/output.txt."

doctest:
	$(SPHINXBUILD) -b doctest $(ALLSPHINXOPTS) $(BUILDDIR)/doctest
	@echo "Testing of doctests in the sources finished, look at the " \
	      "results in $(BUILDDIR)/doctest/output.txt."
