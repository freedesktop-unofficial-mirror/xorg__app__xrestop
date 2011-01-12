
# Initialize Autoconf
AC_PREREQ([2.60])
AC_INIT([xrestop], [0.0.4],
        [https://bugs.freedesktop.org/enter_bug.cgi?product=xorg], [xrestop])
AC_CONFIG_HEADERS([config.h])

AM_INIT_AUTOMAKE()
AM_MAINTAINER_MODE

# Require X.Org macros 1.8 or later for MAN_SUBSTS set by XORG_MANPAGE_SECTIONS
m4_ifndef([XORG_MACROS_VERSION],
          [m4_fatal([must install xorg-macros 1.8 or later before running autoconf/autogen])])
XORG_MACROS_VERSION(1.8)
XORG_CWARNFLAGS

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

found_xres_lib=no
AC_CHECK_LIB(XRes, XResQueryClients, XRES_LIBS=-lXRes found_xres_lib=yes,,
	-lXext $ALL_X_LIBS)

if test "x$found_xres_lib" = "xno"; then
    # Try again, overriding defaults for certain platforms
    case `uname -sr` in
      	"SunOS 5"*)
	    # Solaris ships libXres in /usr/openwin/sfw/lib
	    save_LDFLAGS=$LDFLAGS
	    SOL_XRES_LDFLAGS="-L/usr/openwin/sfw/lib -R/usr/openwin/sfw/lib"
	    LDFLAGS="$LDFLAGS $SOL_XRES_LDFLAGS"
	    AC_CHECK_LIB(XRes, XResQueryClientResources, 
		found_xres_lib=yes XRES_LIBS="$SOL_XRES_LDFLAGS -lXRes",
		,-lXext $ALL_X_LIBS)
	    LDFLAGS="$save_LDFLAGS"
	;;
    esac
fi


if test "x$found_xres_lib" = "xno"; then
   AC_MSG_ERROR([Cannot find XRes extension library])
   exit 1
fi

found_xres=no

AC_CHECK_HEADER(X11/extensions/XRes.h,found_xres=yes,,[#include <X11/Xlib.h>])

if test "x$found_xres" = "xno"; then
   AC_MSG_ERROR([Cannot find XRes extension headers])
   exit 1
fi

XLIBS_LIBS="$XRES_LIBS -lXext $ALL_X_LIBS"
   	
fi

# check for ncurses, fall back to curses
AC_CHECK_LIB([ncurses], [initscr],,AC_CHECK_LIB([curses], [initscr]))

XLIBS_CFLAGS="$CWARNFLAGS $XLIBS_CFLAGS"
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

XORG_CHANGELOG

AC_OUTPUT([Makefile doc/Makefile xrestop.spec])
