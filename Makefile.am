bin_PROGRAMS = sxiv

# Build only the selected autoreload backend
AUTORELOAD_SOURCES = autoreload_$(AUTORELOAD_BACKEND).c

sxiv_SOURCES = \
	commands.c \
	image.c \
	main.c \
	options.c \
	thumbs.c \
	util.c \
	window.c

nodist_sxiv_SOURCES = \
	$(AUTORELOAD_SOURCES)

sxiv_CPPFLAGS = \
	-std=c99 \
	-Wall \
	-pedantic \
	-D_XOPEN_SOURCE=700 \
	-DAUTORELOAD_BACKEND=\"$(AUTORELOAD_BACKEND)\"

# Create version.h from git describe
version.h: Makefile
	@echo "GEN $@"
	v="$$(cd $(srcdir); git describe --tags 2>/dev/null)"; \
	echo "#define VERSION \"$${v:-26}\"" >$@ || echo "#define VERSION \"26\"" >$@

BUILT_SOURCES = version.h

# Clean version.h on distclean
DISTCLEANFILES = version.h

man_MANS = sxiv.1

# Install scripts to share/sxiv/exec
sxivexecdir = $(datadir)/sxiv/exec
dist_sxivexec_SCRIPTS = \
	exec/image-info \
	exec/key-handler

# Install desktop file
dist_pkgdata_DATA = sxiv.desktop

SUBDIRS = icon
