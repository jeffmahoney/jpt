ASCIIDOC = /usr/bin/asciidoc
ASCIIDOC_EXTRA =
MANPAGE_XSL = manpage-normal.xsl
XMLTO = /usr/bin/xmlto
XMLTO_EXTRA = -m manpage-bold-literal.xsl
GZIPCMD = /usr/bin/gzip
INSTALL = /usr/bin/install -c

MAN1_TXT = exportpatch.asciidoc fixpatch.asciidoc
MAN5_TXT = patchtools.cfg.asciidoc
prefix ?= /usr
mandir ?= $(prefix)/share/man
man1dir = $(mandir)/man1
man5dir = $(mandir)/man5

PATCHTOOLS_VERSION = $(shell python ../setup.py --version 2> /dev/null)

GZ_MAN1 = $(patsubst %.asciidoc,%.1.gz,$(MAN1_TXT))
GZ_MAN5 = $(patsubst %.asciidoc,%.5.gz,$(MAN5_TXT))

%.1.gz : %.1
	$(GZIPCMD) -n -c $< > $@

%.1 : %.xml
	$(RM) -f $@ && \
	$(XMLTO) -m $(MANPAGE_XSL) $(XMLTO_EXTRA) man $<
%.5.gz : %.5
	$(GZIPCMD) -n -c $< > $@

%.5 : %.xml
	$(RM) -f $@ && \
	$(XMLTO) -m $(MANPAGE_XSL) $(XMLTO_EXTRA) man $<

%.xml : %.asciidoc asciidoc.conf
	rm -f $@+ $@
	$(ASCIIDOC) -b docbook -d manpage -f asciidoc.conf \
		$(ASCIIDOC_EXTRA) -apatchtools_version=$(PATCHTOOLS_VERSION) \
		-o $@+ $<
	mv $@+ $@

man: $(GZ_MAN1) $(GZ_MAN5)

all: man

man-install: man
	$(INSTALL) -d -m 755 $(DESTDIR)$(man1dir)
	$(INSTALL) -m 644 $(GZ_MAN1) $(DESTDIR)$(man1dir)
	$(INSTALL) -d -m 755 $(DESTDIR)$(man5dir)
	$(INSTALL) -m 644 $(GZ_MAN5) $(DESTDIR)$(man5dir)

man-clean:
	rm -f *.[0-9].gz *~

install: man-install

clean: man-clean
