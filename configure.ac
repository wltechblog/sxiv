AC_PREREQ([2.69])
AC_INIT([sxiv], [m4_esyscmd_s([git describe --tags 2>/dev/null || echo 26])], [https://github.com/wltechblog/sxiv], [sxiv])
AC_CONFIG_SRCDIR([main.c])
AC_CONFIG_HEADERS([config-auto.h])
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([1.14 foreign subdir-objects -Wall -Werror])

# Checks for programs
AC_PROG_CC
AM_PROG_CC_C_O

# Check for GNU Make
AC_MSG_CHECKING([for GNU Make])
if make --version 2>/dev/null | grep -q "GNU Make"; then
	AC_MSG_RESULT([yes])
	MAKE="make"
else
	AC_MSG_RESULT([no])
	AC_CHECK_PROGS([GMAKE], [gmake], [no])
	if test "$GMAKE" = "no"; then
		AC_MSG_ERROR([GNU Make not found. Please install GNU Make (usually called 'gmake' on BSD systems).])
	else
		if $GMAKE --version 2>/dev/null | grep -q "GNU Make"; then
			MAKE=$GMAKE
			AC_MSG_RESULT([found: $GMAKE (GNU Make)])
		else
			AC_MSG_ERROR([GNU Make not found. Neither 'make' nor 'gmake' are GNU Make.])
		fi
	fi
fi
AC_SUBST([MAKE])

# Store GNU_MAKE for user instructions
GNU_MAKE=$MAKE
AC_SUBST([GNU_MAKE])

# Checks for libraries
PKG_CHECK_MODULES([IMLIB2], [imlib2], [],
	[AC_MSG_ERROR([Imlib2 not found])])
PKG_CHECK_MODULES([X11], [x11], [],
	[AC_MSG_ERROR([X11 not found])])
PKG_CHECK_MODULES([XFT], [xft], [],
	[AC_MSG_ERROR([Xft not found])])
PKG_CHECK_MODULES([FONTCONFIG], [fontconfig], [],
	[AC_MSG_ERROR([fontconfig not found])])
PKG_CHECK_MODULES([FREETYPE2], [freetype2], [],
	[AC_MSG_ERROR([freetype2 not found])])

# Check for optional giflib
AC_ARG_WITH([giflib],
	[AS_HELP_STRING([--without-giflib], [disable giflib support])],
	[], [with_giflib=check])
AS_IF([test "x$with_giflib" != "xno"], [
	AC_CHECK_LIB([gif], [DGifOpenFileName],
		[AC_DEFINE([HAVE_GIFLIB], [1], [Define to 1 if you have giflib])
		 GIFLIB_LIBS="-lgif"
		 AC_SUBST([GIFLIB_LIBS])
		 have_giflib=yes],
		[have_giflib=no])
], [
	have_giflib=no
])
AS_IF([test "x$with_giflib" = "xyes" && test "x$have_giflib" = "xno"], [
	AC_MSG_ERROR([giflib requested but not found])
])

# Check for optional libexif
AC_ARG_WITH([libexif],
	[AS_HELP_STRING([--without-libexif], [disable libexif support])],
	[], [with_libexif=check])
AS_IF([test "x$with_libexif" != "xno"], [
	PKG_CHECK_MODULES([LIBEXIF], [libexif],
		[AC_DEFINE([HAVE_LIBEXIF], [1], [Define to 1 if you have libexif])
		 have_libexif=yes],
		[have_libexif=no])
], [
	have_libexif=no
])
AS_IF([test "x$with_libexif" = "xyes" && test "x$have_libexif" = "xno"], [
	AC_MSG_ERROR([libexif requested but not found])
])

# Check for inotify support (for autoreload)
AC_CHECK_HEADERS([sys/inotify.h],
	[have_inotify=yes
	 AC_DEFINE([HAVE_INOTIFY], [1], [Define to 1 if you have inotify support])],
	[have_inotify=no])

# Select autoreload backend
AS_IF([test "x$have_inotify" = "xyes"], [
	AUTORELOAD_BACKEND="inotify"
], [
	AUTORELOAD_BACKEND="nop"
])
AC_SUBST([AUTORELOAD_BACKEND])

# Combine library flags
LIBS="$LIBS $IMLIB2_LIBS $X11_LIBS $XFT_LIBS $FONTCONFIG_LIBS $FREETYPE2_LIBS"
AS_IF([test "x$have_giflib" = "xyes"], [
	LIBS="$LIBS $GIFLIB_LIBS"
])
AS_IF([test "x$have_libexif" = "xyes"], [
	LIBS="$LIBS $LIBEXIF_LIBS"
])

# Combine CFLAGS
CFLAGS="$CFLAGS $IMLIB2_CFLAGS $X11_CFLAGS $XFT_CFLAGS $FONTCONFIG_CFLAGS $FREETYPE2_CFLAGS"
AS_IF([test "x$have_libexif" = "xyes"], [
	CFLAGS="$CFLAGS $LIBEXIF_CFLAGS"
])

# Checks for header files
AC_CHECK_HEADERS([sys/inotify.h])

# Checks for typedefs, structures, and compiler characteristics
AC_C_INLINE
AC_TYPE_SIZE_T

# Checks for library functions
AC_FUNC_MALLOC
AC_FUNC_REALLOC

AC_CONFIG_FILES([
	Makefile
	icon/Makefile
])
AC_CONFIG_LINKS([config.h:config-auto.h])
AC_OUTPUT

# Build instructions
echo ""
echo "Configuration complete."
if test "$MAKE" != "make"; then
	echo ""
	echo "=========================================="
	echo "IMPORTANT: GNU Make is required for building."
	echo "Please use the following command to build:"
	echo ""
	echo "  $MAKE"
	echo ""
	echo "Or use:"
	echo "  $MAKE && sudo $MAKE install"
	echo "=========================================="
	echo ""
else
	echo "Now run:"
	echo "  $MAKE"
fi
echo ""

echo ""
echo "Configuration:"
echo "  Autoreload backend: $AUTORELOAD_BACKEND"
echo "  GIF support: $have_giflib"
echo "  EXIF support: $have_libexif"
