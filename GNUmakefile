# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=	/usr/local
BINDIR ?=	$(PREFIX)/bin
MANDIR ?=	$(PREFIX)/share/man
MAN1DIR ?=	$(MANDIR)/man1

GZIPCMD?=	gzip
INSTALL_DATA?=	install -m 644
INSTALL_SCRIPT?=	install -m 755
RST2HTML?=$(call first_in_path,rst2html.py rst2html)

artifacts =	README.html pastish.1.gz pastish

wc:
	git submodule init
	git submodule update

all: $(artifacts)

clean:
	$(RM) $(artifacts)

check: all
	SHELL=$(SHELL) $(SHELL) rnt/run-tests.sh tests $$PWD/pastish

pastish: pastish.in
	$(INSTALL_SCRIPT) $< $@

html: README.html

%.html: %.rest
	$(RST2HTML) $< $@

%.1.gz: %.1
	$(GZIPCMD) < $< > $@

install: pastish.1.gz
	@mkdir -p $(DESTDIR)$(BINDIR)
	@mkdir -p $(DESTDIR)$(MAN1DIR)
	@$(INSTALL_SCRIPT) pastish.in $(DESTDIR)$(BINDIR)/pastish
	@$(INSTALL_DATA) pastish.1.gz $(DESTDIR)$(MAN1DIR)/pastish.1.gz

define first_in_path
  $(firstword $(wildcard \
    $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
  ))
endef

.DEFAULT_GOAL := all

.PHONY: all
.PHONY: check
.PHONY: clean
.PHONY: html
.PHONY: install
.PHONY: wc
