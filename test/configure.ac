dnl Process this file with autoconf to produce a configure script.
AC_INIT
AC_CONFIG_SRCDIR([loopwave.c])

dnl Detect the canonical build and host environments
AC_CONFIG_AUX_DIR([../build-scripts])
AC_CANONICAL_HOST

dnl Check for tools
AC_PROG_CC

dnl Check for compiler environment
AC_C_CONST

dnl Figure out which math library to use
case "$host" in
    *-*-cygwin* | *-*-mingw*)
        EXE=".exe"
        MATHLIB=""
        SYS_GL_LIBS="-lopengl32"
        ;;
    *-*-beos* | *-*-haiku*)
        EXE=""
        MATHLIB=""
        SYS_GL_LIBS="-lGL"
        ;;
    *-*-darwin*)
        EXE=""
        MATHLIB=""
        SYS_GL_LIBS="-Wl,-framework,OpenGL"
        ;;
    *-*-aix*)
        EXE=""
        if test x$ac_cv_c_compiler_gnu = xyes; then
            CFLAGS="-mthreads"
        fi
        SYS_GL_LIBS=""
        ;;
    *-*-mint*)
        EXE=""
        MATHLIB=""
        AC_PATH_PROG(OSMESA_CONFIG, osmesa-config, no)
        if test "x$OSMESA_CONFIG" = "xyes"; then
            OSMESA_CFLAGS=`$OSMESA_CONFIG --cflags`
            OSMESA_LIBS=`$OSMESA_CONFIG --libs`
            CFLAGS="$CFLAGS $OSMESA_CFLAGS"
            SYS_GL_LIBS="$OSMESA_LIBS"
        else
            SYS_GL_LIBS="-lOSMesa"
        fi
        ;;
    *-*-riscos*)
        EXE=",e1f"
        MATHLIB=""
        SYS_GL_LIBS=""
        ;;
    *-*-os2*)
        EXE=".exe"
        MATHLIB=""
        SYS_GL_LIBS=""
        ;;
    *)
        EXE=""
        MATHLIB="-lm"
        SYS_GL_LIBS="-lGL"
        ;;
esac
AC_SUBST(EXE)
AC_SUBST(MATHLIB)

dnl Check for SDL
SDL_VERSION=1.2.10
AM_PATH_SDL($SDL_VERSION,
            :,
	    AC_MSG_ERROR([*** SDL version $SDL_VERSION not found!])
)
CFLAGS="$CFLAGS $SDL_CFLAGS"
LIBS="$LIBS $SDL_LIBS"

dnl Check for X11 path, needed for OpenGL on some systems
AC_PATH_X
if test x$have_x = xyes; then
    if test x$ac_x_includes = xno || test x$ac_x_includes = x; then
        :
    else
        CFLAGS="$CFLAGS -I$ac_x_includes"
    fi
    if test x$ac_x_libraries = xno || test x$ac_x_libraries = x; then
        :
    else
        XPATH="-L$ac_x_libraries"
    fi
fi

dnl Check for OpenGL
AC_MSG_CHECKING(for OpenGL support)
have_opengl=no
AC_TRY_COMPILE([
 #include "SDL_opengl.h"
],[
],[
have_opengl=yes
])
AC_MSG_RESULT($have_opengl)
if test x$have_opengl = xyes; then
    CFLAGS="$CFLAGS -DHAVE_OPENGL"
    GLLIB="$XPATH $SYS_GL_LIBS"
else
    GLLIB=""
fi
AC_SUBST(GLLIB)

dnl Finally create all the generated files
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
