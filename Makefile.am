SUBDIRS = doc

AM_CFLAGS = @XLIBS_CFLAGS@

bin_PROGRAMS = xrestop

xrestop_SOURCES = xrestop.c

xrestop_LDADD = @XLIBS_LIBS@

extra_DIST = xrestop.spec xrestop.spec.in

EXTRA_DIST = ChangeLog
MAINTAINERCLEANFILES = ChangeLog

.PHONY: ChangeLog

ChangeLog:
	$(CHANGELOG_CMD)

dist-hook: ChangeLog
