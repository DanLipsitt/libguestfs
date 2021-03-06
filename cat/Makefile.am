# libguestfs virt-cat, virt-filesystems and virt-ls.
# Copyright (C) 2010-2012 Red Hat Inc.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

include $(top_srcdir)/subdir-rules.mk

EXTRA_DIST = \
	test-virt-cat.sh \
	virt-cat.pod \
	test-virt-filesystems.sh \
	virt-filesystems.pod \
	test-virt-ls.sh \
	virt-ls.pod

CLEANFILES = stamp-virt-cat.pod stamp-virt-ls.pod stamp-virt-filesystems.pod

bin_PROGRAMS = virt-cat virt-filesystems virt-ls

SHARED_SOURCE_FILES = \
	../fish/domain.c \
	../fish/inspect.c \
	../fish/keys.c \
	../fish/options.h \
	../fish/options.c \
	../fish/uri.h \
	../fish/uri.c

virt_cat_SOURCES = \
	$(SHARED_SOURCE_FILES) \
	cat.c

virt_cat_CPPFLAGS = \
	-DGUESTFS_WARN_DEPRECATED=1 \
	-DLOCALEBASEDIR=\""$(datadir)/locale"\" \
	-I$(top_srcdir)/src -I$(top_builddir)/src \
	-I$(top_srcdir)/fish \
	-I$(srcdir)/../gnulib/lib -I../gnulib/lib

virt_cat_CFLAGS = \
	$(WARN_CFLAGS) $(WERROR_CFLAGS) \
	$(GPROF_CFLAGS) $(GCOV_CFLAGS) \
	$(LIBXML2_CFLAGS)

virt_cat_LDADD = \
	$(top_builddir)/src/libutils.la \
	$(top_builddir)/src/libguestfs.la \
	$(LIBXML2_LIBS) \
	$(LIBVIRT_LIBS) \
	../gnulib/lib/libgnu.la

virt_filesystems_SOURCES = \
	$(SHARED_SOURCE_FILES) \
	filesystems.c

virt_filesystems_CPPFLAGS = \
	-DGUESTFS_WARN_DEPRECATED=1 \
	-DLOCALEBASEDIR=\""$(datadir)/locale"\" \
	-I$(top_srcdir)/src -I$(top_builddir)/src \
	-I$(top_srcdir)/fish \
	-I$(srcdir)/../gnulib/lib -I../gnulib/lib

virt_filesystems_CFLAGS = \
	$(WARN_CFLAGS) $(WERROR_CFLAGS) \
	$(GPROF_CFLAGS) $(GCOV_CFLAGS) \
	$(LIBXML2_CFLAGS)

virt_filesystems_LDADD = \
	$(top_builddir)/src/libutils.la \
	$(top_builddir)/src/libguestfs.la \
	$(LIBXML2_LIBS) \
	$(LIBVIRT_LIBS) \
	../gnulib/lib/libgnu.la

virt_ls_SOURCES = \
	$(SHARED_SOURCE_FILES) \
	ls.c \
	visit.c \
	visit.h

virt_ls_CPPFLAGS = \
	-DGUESTFS_WARN_DEPRECATED=1 \
	-DLOCALEBASEDIR=\""$(datadir)/locale"\" \
	-I$(top_srcdir)/src -I$(top_builddir)/src \
	-I$(top_srcdir)/fish \
	-I$(srcdir)/../gnulib/lib -I../gnulib/lib

virt_ls_CFLAGS = \
	$(WARN_CFLAGS) $(WERROR_CFLAGS) \
	$(GPROF_CFLAGS) $(GCOV_CFLAGS) \
	$(LIBXML2_CFLAGS)

virt_ls_LDADD = \
	$(top_builddir)/src/libutils.la \
	$(top_builddir)/src/libguestfs.la \
	$(LIBXML2_LIBS) \
	$(LIBVIRT_LIBS) \
	../gnulib/lib/libgnu.la

# Manual pages and HTML files for the website.
man_MANS = virt-cat.1 virt-filesystems.1 virt-ls.1

noinst_DATA = \
	$(top_builddir)/html/virt-cat.1.html \
	$(top_builddir)/html/virt-filesystems.1.html \
	$(top_builddir)/html/virt-ls.1.html

virt-cat.1 $(top_builddir)/html/virt-cat.1.html: stamp-virt-cat.pod

stamp-virt-cat.pod: virt-cat.pod
	$(PODWRAPPER) \
	  --man virt-cat.1 \
	  --html $(top_builddir)/html/virt-cat.1.html \
	  --license GPLv2+ \
	  $<
	touch $@

virt-ls.1 $(top_builddir)/html/virt-ls.1.html: stamp-virt-ls.pod

stamp-virt-ls.pod: virt-ls.pod
	$(PODWRAPPER) \
	  --man virt-ls.1 \
	  --html $(top_builddir)/html/virt-ls.1.html \
	  --license GPLv2+ \
	  $<
	touch $@

virt-filesystems.1 $(top_builddir)/html/virt-filesystems.1.html: stamp-virt-filesystems.pod

stamp-virt-filesystems.pod: virt-filesystems.pod
	$(PODWRAPPER) \
	  --man virt-filesystems.1 \
	  --html $(top_builddir)/html/virt-filesystems.1.html \
	  --license GPLv2+ \
	  $<
	touch $@

# Tests.

TESTS_ENVIRONMENT = $(top_builddir)/run --test

if ENABLE_APPLIANCE
TESTS = \
	test-virt-cat.sh \
	test-virt-filesystems.sh \
	test-virt-ls.sh
endif ENABLE_APPLIANCE

check-valgrind:
	$(MAKE) VG="$(top_builddir)/run @VG@" check

check-valgrind-local-guests:
	for g in $(GUESTS); do \
	  $(top_builddir)/run --test @VG@ ./virt-filesystems -c "$(libvirt_ro_uri)" -d "$$g" --all --long -h --uuid || exit $$?; \
	done
