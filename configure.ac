#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.53)
AC_INIT([xrestop], 0.1, [mallum@handhelds.org])
AC_CONFIG_SRCDIR([xrestop.c])

AM_INIT_AUTOMAKE()
AM_MAINTAINER_MODE
AM_CONFIG_HEADER([config.h])

# Checks for programs.
AC_PROG_CC

XRES_LIBS=	

PKG_CHECK_MODULES(X11, x11, [have_libx11pc="yes"], [have_libx11pc="no"])

if test $have_libx11pc = yes; then
   PKG_CHECK_MODULES(XLIBS, x11 xres xext)
else
dnl **** Check for xlibs 'non pc' way ****

AC_PATH_XTRA

ALL_X_LIBS="$X_LIBS -lX11"

found_xres=no
AC_CHECK_LIB(XRes, XResQueryClients,
               [AC_CHECK_HEADER(X11/extensions/XRes.h,
                                XRES_LIBS=-lXRes found_xres=yes,,
				[#include <X11/Xlib.h>])],
               , -lXRes -lXext $ALL_X_LIBS)

if test "x$found_xres" = "xno"; then
   AC_MSG_ERROR([Cannot find XRes extension])
   exit 1
fi

XLIBS_LIBS="-lXRes -lXext $ALL_X_LIBS"
   	
fi

AC_CHECK_LIB([ncurses], [initscr])

if test "x$GCC" = "xyes"; then
        GCC_WARNINGS="-Wall -fno-strict-aliasing"
        XLIBS_CFLAGS="$GCC_WARNINGS $XLIBS_CFLAGS"
fi

AC_SUBST(XLIBS_CFLAGS)
AC_SUBST(XLIBS_LIBS)

# Checks for header files.

AC_HEADER_STDC
AC_CHECK_HEADERS([stdlib.h string.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_PID_T

# Checks for library functions.
 
AC_CHECK_FUNCS([memset strdup])

AC_OUTPUT([Makefile])
