dist_icon_DATA = \
	16x16.png \
	32x32.png \
	48x48.png \
	64x64.png \
	128x128.png

icondir = $(datadir)/icons/hicolor

install-data-local:
	@for f in $(dist_icon_DATA); do \
		size="$${f%.png}"; \
		dir="$(DESTDIR)$(icondir)/$$size/apps"; \
		$(MKDIR_P) "$$dir"; \
		$(INSTALL_DATA) $(srcdir)/$$f "$$dir/sxiv.png"; \
	done

uninstall-local:
	@for f in $(dist_icon_DATA); do \
		size="$${f%.png}"; \
		rm -f "$(DESTDIR)$(icondir)/$$size/apps/sxiv.png"; \
	done
