SUBDIRS = doc

AM_CFLAGS = @XLIBS_CFLAGS@

bin_PROGRAMS = xrestop

xrestop_SOURCES = xrestop.c

xrestop_LDADD = @XLIBS_LIBS@

extra_DIST = xrestop.spec xrestop.spec.in

EXTRA_DIST += ChangeLog
MAINTAINERCLEANFILES = ChangeLog

.PHONY: ChangeLog

ChangeLog:
	(GIT_DIR=$(top_srcdir)/.git git-log > .changelog.tmp && mv .changelog.tmp ChangeLog; rm -f .changelog.tmp) || (touch ChangeLog; echo 'git directory not found: installing possibly empty changelog.' >&2)

dist-hook: ChangeLog
