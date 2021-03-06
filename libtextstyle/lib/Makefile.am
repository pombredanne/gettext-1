## Makefile for the lib subdirectory of GNU libtextstyle.
## Copyright (C) 2009-2019 Free Software Foundation, Inc.
##
## This program is free software: you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation; either version 3 of the License, or
## (at your option) any later version.
##
## This program is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with this program.  If not, see <https://www.gnu.org/licenses/>.

## Process this file with automake to produce Makefile.in.

AUTOMAKE_OPTIONS = 1.13 gnits subdir-objects no-dependencies
EXTRA_DIST =
BUILT_SOURCES =
MOSTLYCLEANFILES = core *.stackdump
MOSTLYCLEANDIRS =
CLEANFILES =
DISTCLEANFILES =
MAINTAINERCLEANFILES =
SUFFIXES =

lib_LTLIBRARIES = libtextstyle.la

noinst_LTLIBRARIES =

nobase_include_HEADERS = textstyle.h

nobase_nodist_include_HEADERS =

noinst_HEADERS = notinline.h

AM_CPPFLAGS = \
  -I. -I$(srcdir) \
  -I$(top_builddir) -I$(top_srcdir) \
  -DIN_LIBTEXTSTYLE

# Rules generated and collected by gnulib-tool.
include Makefile.gnulib

# Additional source files.
libtextstyle_la_SOURCES += \
  color.h color.c \
  misc.h misc.c \
  version.c

# The <stdbool.h> replacement that can be installed.
nobase_nodist_include_HEADERS += \
  textstyle/stdbool.h

# Produce a textstyle/stdbool.h that is not compiler dependent.
# GCC >= 2.95 has <stdbool.h>.
# AIX >= 5.3 has <stdbool.h>.
# Solaris 10 and some HP-UX 11 versions have <stdbool.h> but it does not
# necessarily work.
textstyle/stdbool.h : $(STDBOOL_H) stdbool.mini.h
	@MKDIR_P@ textstyle
	rm -f $@-t $@
	{ echo '/* DO NOT EDIT! GENERATED AUTOMATICALLY! */'; \
	  echo '#if !defined _GL_STDBOOL_H'; \
	  if test -f /usr/include/stdbool.h; then \
	    echo '#if (__GNUC__ > 2 || (__GNUC__ == 2 && __GNUC_MINOR__ >= 95)) || defined _AIX'; \
	  else \
	    echo '#if (__GNUC__ > 2 || (__GNUC__ == 2 && __GNUC_MINOR__ >= 95))'; \
	  fi; \
	  echo '#include <stdbool.h>'; \
	  echo '#else'; \
	  cat $(srcdir)/stdbool.mini.h; \
	  echo '#endif'; \
	  echo '#endif'; \
	} > $@-t
	mv $@-t $@
BUILT_SOURCES    += textstyle/stdbool.h
MOSTLYCLEANFILES += textstyle/stdbool.h-t
CLEANFILES       += textstyle/stdbool.h
EXTRA_DIST       += stdbool.mini.h

# textstyle/version.h is public.
nobase_nodist_include_HEADERS += textstyle/version.h

# textstyle/woe32dll.h is not public, but is included by other header files.
nobase_nodist_include_HEADERS += textstyle/woe32dll.h

# Directories that contain some CLEANFILES.
CLEANDIRS =
CLEANDIRS_NOT_IN_SRCDIR = textstyle
clean-local: clean-generic
	@for dir in '' $(CLEANDIRS); do \
	  if test -n "$$dir" && test -d $$dir; then \
	    echo "rmdir $$dir"; rmdir $$dir; \
	  fi; \
	done; \
	if test '$(srcdir)' != '.'; then \
	  for dir in '' $(CLEANDIRS_NOT_IN_SRCDIR); do \
	    if test -n "$$dir" && test -d $$dir; then \
	      echo "rmdir $$dir"; rmdir $$dir; \
	    fi; \
	  done; \
	fi; \
        :

# Directories that contain some DISTCLEANFILES.
DISTCLEANDIRS = sys
DISTCLEANDIRS_NOT_IN_SRCDIR =
distclean-local: distclean-generic
	@for dir in '' $(DISTCLEANDIRS); do \
	  if test -n "$$dir" && test -d $$dir; then \
	    echo "rmdir $$dir"; rmdir $$dir; \
	  fi; \
	done; \
	if test '$(srcdir)' != '.'; then \
	  for dir in '' $(DISTCLEANDIRS_NOT_IN_SRCDIR); do \
	    if test -n "$$dir" && test -d $$dir; then \
	      echo "rmdir $$dir"; rmdir $$dir; \
	    fi; \
	  done; \
	fi; \
        :

# List of header files that get installed and that declare 'extern' symbols.
HEADERS_WITH_EXTERNS = \
  textstyle.h \
  textstyle/version.in.h

# List of exported symbols.
# We extract it from the header files that get installed.
# This file has the same format as the one expected by the libtool option
# '-export-symbols', but we don't use this option, because it may prevent us
# from building some of the unit tests.
$(srcdir)/libtextstyle.sym : $(HEADERS_WITH_EXTERNS)
	for f in $(HEADERS_WITH_EXTERNS); do \
	  if test -f $$f; then \
	    cat $$f; \
	  else \
	    cat $(srcdir)/$$f; \
	  fi; \
	done \
	  | $(srcdir)/declared.sh | LC_ALL=C sort | LC_ALL=C uniq \
	  > $@-t
	mv $@-t $@
# We distribute it because declared.sh relies on GNU sed.
# The GNU Coding Standards say in
# <https://www.gnu.org/prep/standards/html_node/Makefile-Basics.html>:
#   "GNU distributions usually contain some files which are not source files
#    ... . Since these files normally appear in the source directory, they
#    should always appear in the source directory, not in the build directory.
#    So Makefile rules to update them should put the updated files in the
#    source directory."
# Therefore we put this file in the source directory, not the build directory.
MOSTLYCLEANFILES     += libtextstyle.sym-t
MAINTAINERCLEANFILES += libtextstyle.sym
EXTRA_DIST           += libtextstyle.sym declared.sh

# TEMPORARY
if false

## Tell the mingw or Cygwin linker which symbols to export.
#if WOE32DLL
#libtextstyle_la_SOURCES += ../woe32dll/unistring-exports.c
#libtextstyle_la_LDFLAGS += -Wl,--export-all-symbols
#endif

endif

# Hide undesired symbols that are defined by libtextstyle_la_SOURCES or
# gl_LIBOBJS or the dependency libraries from the global namespace,
# by prefixing them with "libtextstyle_".
all check install: config.h
COMPILATION_UNITS = $(libtextstyle_la_SOURCES) $(gl_LIBOBJS)
if INCLUDED_LIBCROCO
COMPILATION_UNITS += $(libcroco_rpl_la_SOURCES)
endif
if INCLUDED_LIBXML
COMPILATION_UNITS += $(libxml_rpl_la_SOURCES)
endif
if INCLUDED_LIBGLIB
COMPILATION_UNITS += $(libglib_rpl_la_SOURCES)
endif
config.h: $(BUILT_SOURCES) $(srcdir)/libtextstyle.sym
	{ echo '/* DO NOT EDIT! GENERATED AUTOMATICALLY! */'; \
	  : "Avoid double inclusion, to avoid a warning about redefinitions."; \
	  echo '#ifndef LIBTEXTSTYLE_CONFIG_H'; \
	  echo '#define LIBTEXTSTYLE_CONFIG_H'; \
	  echo; \
	  echo '#include "../config.h"'; \
	  echo; \
	  echo '#endif /* LIBTEXTSTYLE_CONFIG_H */'; \
	} > config.h && \
	if test -n "$(NAMESPACING)" && test -n "$(HAVE_GLOBAL_SYMBOL_PIPE)"; then \
	  { \
	    { \
	      for f in $(COMPILATION_UNITS); do \
	        case $$f in \
	          *.res.lo ) ;; \
	          *.c | *.$(OBJEXT) | *.lo ) \
	            sf=`echo "$$f" | sed -e 's,\\.[^.]*$$,,'`.c; \
	            test -f $$sf || sf=$(srcdir)/$$sf; \
	            of=`echo "$$f" | sed -e 's,^.*/,,' -e 's,\\.[^.]*$$,,'`.$(OBJEXT); \
	            echo "$(COMPILE) -c $$sf" 1>&6; \
	            $(COMPILE) -c $$sf || { rm -f config.h; exit 1; }; \
	            sh ./exported.sh $$of 1>&5; \
	            rm -f $$of `echo "$$of" | sed -e 's,\\.$(OBJEXT)$$,.lo,'`; \
	            ;; \
	        esac; \
	      done; \
	    } 5>&1 \
	      | sed -e 's,.* ,,' | LC_ALL=C sort | LC_ALL=C uniq \
	      | LC_ALL=C join -v 1 - $(srcdir)/libtextstyle.sym \
	      | sed -e 's,^\(.*\)$$,#define \1 libtextstyle_\1,' > config.h-t; \
	  } 6>&1 && \
	  if test -f config.h; then \
	    cat config.h-t >> config.h; \
	    rm -f config.h-t; \
	  else \
	    rm -f config.h-t; \
	    exit 1; \
	  fi \
	fi
MOSTLYCLEANFILES += config.h config.h-t

# TEMPORARY
if false

## Version information according to Woe32 conventions.
#EXTRA_DIST += libtextstyle.rc
#if WOE32
#WOE32_LIBADD = libtextstyle.res.lo
#libtextstyle.res.lo : $(srcdir)/libtextstyle.rc
#	$(LIBTOOL) --tag=RC $(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=compile $(RC) `$(SHELL) $(top_srcdir)/build-aux/windres-options --escape $(VERSION)` -i $(srcdir)/libtextstyle.rc -o libtextstyle.res.lo --output-format=coff
#MOSTLYCLEANFILES += libtextstyle.res.lo
#else
#WOE32_LIBADD =
#endif
#libtextstyle_la_LIBADD       += $(WOE32_LIBADD)
#libtextstyle_la_DEPENDENCIES += $(WOE32_LIBADD)

endif

# Parametrization of the 'relocatable-lib-lgpl' module.
AM_CPPFLAGS += -DDEPENDS_ON_LIBICONV=1

# Libtool's library version information for libtextstyle.
# See the libtool documentation, section "Library interface versions".
LTV_CURRENT=0
LTV_REVISION=0
LTV_AGE=0

# How to build libtextstyle.la.
libtextstyle_la_LDFLAGS += \
  -version-info $(LTV_CURRENT):$(LTV_REVISION):$(LTV_AGE) \
  -rpath $(libdir)
# Use the C++ compiler only when needed.
if WOE32DLL
libtextstyle_la_LINK = \
  $(LIBTOOL) $(AM_V_lt) --tag=CXX $(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link \
  $(CXXLD) $(AM_CXXFLAGS) $(CXXFLAGS) $(libtextstyle_la_LDFLAGS) $(LDFLAGS) -o $@
else
libtextstyle_la_LINK = \
  $(LIBTOOL) $(AM_V_lt) --tag=CC $(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link \
  $(CCLD) $(AM_CFLAGS) $(CFLAGS) $(libtextstyle_la_LDFLAGS) $(LDFLAGS) -o $@
endif
