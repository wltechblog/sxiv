# sxiv

Simple X Image Viewer - A continuation of development

This project is a continuation of the sxiv (Simple X Image Viewer) project, originally created by Bert Muennich. sxiv is a lightweight, efficient image viewer for the X Window System, designed for simplicity and speed.

## Overview

The sole purpose of sxiv is to be a perfect image viewer. It is light, fast, and includes only the common useful features.

It is free software so that you can use it and modify it for your needs. Bug reports and contributions are welcome.

## Features

- Basic image operations: zooming, panning, rotating, flipping
- Thumbnail mode: grid of selectable previews of all images
- Multi-frame image support (GIF animations)
- Customizable key and mouse button mappings (in `config-settings.h`)
- Display image information in status bar
- EXIF metadata support (optional, via libexif)
- GIF animation support (optional, via giflib)
- Minimal resource usage
- Fast loading and navigation

## Dependencies

Required:
- Imlib2
- X11
- Xft
- fontconfig
- freetype2

Optional:
- giflib - for GIF support
- libexif - for EXIF metadata handling

## Building

This project supports two build systems:

### GNU Autotools (Recommended)

The autotools build system automatically detects library paths and dependencies:

```bash
# From git repository
./bootstrap
./configure
make

# From release tarball
./configure
make

# Install (requires root)
sudo make install
```

**Note:** This project requires GNU Make. On BSD systems where the default `make` command is not GNU Make, the configure script will detect this and provide clear instructions to use `gmake` instead. You will see a message like:

```
==========================================
IMPORTANT: GNU Make is required for building.
Please use the following command to build:

  gmake

Or use:
  gmake && sudo gmake install
==========================================
```

Configuration options:

```bash
./configure --prefix=/usr               # Install to /usr instead of /usr/local
./configure --without-giflib           # Disable GIF support
./configure --without-libexif           # Disable EXIF support
./configure --help                     # See all options
```

### Manual Makefile (Legacy)

The original Makefile is included as `Makefile.bak` for reference. Edit it to set build options:

```bash
make
sudo make install
```

## Usage

Basic usage:
```bash
# View a single image
sxiv image.jpg

# View multiple images
sxiv image1.jpg image2.png image3.jpg

# View all images in a directory
sxiv directory/

# Start in fullscreen mode
sxiv -f image.jpg

# Start in thumbnail mode
sxiv -t directory/
```

Common options:
- `-f`  : Start in fullscreen mode
- `-t`  : Start in thumbnail mode
- `-s`  : Disable slideshow
- `-r`  : Recursively open images in directories
- `-g`  : Set window geometry (WxH)
- `-Z`  : Start with zoom level 100%
- `-h`  : Display help message

For complete usage information, see the man page:
```bash
man sxiv
```

## Configuration

Behavior and keybindings can be customized in `config-settings.h`. Before building, you can create `config.h` from the default settings:

```bash
cp config-settings.h config.h
```

Then edit `config.h` to modify:
- Window dimensions
- Zoom levels
- Keyboard and mouse mappings
- Color schemes
- Default slideshow delay

## Keybindings (Default)

Image mode:
- `q`        : Quit
- `Return`    : Switch to thumbnail mode
- `Space`/`p`: Next/previous image
- `[`/`]`    : Go 10 images back/forward
- `+`/`-`    : Zoom in/out
- `h`/`j`/`k`/`l`: Pan left/down/up/right
- `r`        : Reload image
- `a`        : Toggle anti-aliasing
- `A`        : Toggle alpha layer checkerboard
- `s`        : Toggle slideshow
- `m`        : Mark image
- `Delete`    : Delete current image (move to trash)

Thumbnail mode:
- `Return`    : Open selected image in image mode
- `h`/`j`/`k`/`l`: Navigate thumbnails

Mouse:
- Left click        : Navigate images
- Scroll wheel      : Navigate images
- Scroll + Ctrl     : Zoom
- Scroll + Shift    : Pan
- Right click       : Switch to thumbnail mode (image mode) / Open image (thumb mode)

## License

This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.

## Contributing

Bug reports and contributions are welcome. Please check the existing documentation and code before submitting changes.
