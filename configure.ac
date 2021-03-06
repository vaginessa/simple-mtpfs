AC_PREREQ([2.60])
AC_INIT([simple-mtpfs], [0.2], [phatina@gmail.com])

AC_CONFIG_MACRO_DIR([m4])

AM_INIT_AUTOMAKE([foreign dist-bzip2])
AC_CONFIG_HEADERS(config.h)
AM_SILENT_RULES([yes])

AC_LANG([C++])
AC_PROG_CXX
AC_PROG_INSTALL

m4_include([m4/cxx11.m4])
AX_CXX_COMPILE_STDCXX_11([noext])
AC_CHECK_LIB([mtp], [LIBMTP_Init], [], [AC_MSG_ERROR([libmtp not found])])
AC_CHECK_LIB([mtp], [LIBMTP_Check_Capability],
    [AC_DEFINE([HAVE_LIBMTP_CHECK_CAPABILITY], [1], [Check device capabilities])],
    [])
AC_CHECK_HEADERS([libmtp.h])

dnl Enable fdatasync, but not on OSX
test `uname -s` == "Darwin" || AC_CHECK_FUNCS([fdatasync])

PKG_CHECK_MODULES([FUSE], [fuse >= 2.7.3])
AC_SUBST([FUSE_CFLAGS])
AC_SUBST([FUSE_LIBS])

PKG_CHECK_MODULES(
    [LIBUSB1],
    [libusb-1.0 >= 1.0.0],
    [AC_DEFINE([HAVE_LIBUSB1], [], [Have libusb 1.0])]
)
AC_SUBST([LIBUSB1_CFLAGS])
AC_SUBST([LIBUSB1_LIBS])

dnl Configurable temporary directory
AC_ARG_WITH(
    [tmpdir],
    [AS_HELP_STRING([--with-tmpdir@<:=DIR@:>@],
        [Directory for temporary files, defaults to /tmp])],
    [], []
)
AS_IF(
    [test "x$withval" != "x"],
    [tmpdir="$withval"],
    [tmpdir="/tmp"]
)
AC_DEFINE_UNQUOTED(
    [TMPDIR],
    ["${tmpdir}"],
    [Directory for temporary files]
)

AC_OUTPUT([
    makefile
    src/makefile
    man/makefile
])

AC_MSG_NOTICE([

    simple-mtpfs $VERSION
    ================

    prefix: ${prefix}
    tmpdir: ${tmpdir}

    Now type 'make' to build $PACKAGE
])
