<!-- TITLE: autobuild/defines -->
<!-- SUBTITLE: Autobuild3's Core Manifest -->

# To Reiterate...

*`autobuild/defines` defines most aspects of a package's build-time and install-time.* This page serves as a comprehensive description of all standardised variables used by Autobuild3 defined in this particular file. Given the large amount of variables that can be potentially defined in this file, this page is divided into the following sections:

- File Format
- Package Metadata
- Build Parametres
- Enhanced Usage

# File Format

Like most Autobuild3 manifests, `autobuild/defines` is by nature a Bash script executed/sourced by Autobuild3. `autobuild/defines` looks like a series of Bash variable declarations:

```
PKGNAME=nautilus
PKGSEC=gnome
PKGDEP="libexif gnome-desktop exempi desktop-file-utils dconf tracker libzeitgeist \
        libnotify packagekit gnome-autoar gvfs gexiv2"
BUILDDEP="gnome-common gtk-doc gobject-introspection intltool meson ninja appstream-glib"
PKGDES="GNOME file manager"

ABTYPE=meson
MESON_AFTER="-Denable-gtk-doc=true \
             -Denable-selinux=false \
             -Denable-desktop=true \
             -Denable-exif=true \
             -Denable-packagekit=true \
             -Denable-xmp=true \
             -Denable-desktop=true \
             -Denable-nst-extension=true"

PKGBREAK="nautilus-sendto<=3.8.5"
PKGREP="nautilus-sendto<=3.8.5"
```

The following sections will discuss what these declarations mean, and how they should be used.

# Package Metadata

In most cases, Autobuild3 can handle the building of a package with the following variables defined:

- `$PKGNAME`

# Build Parametres

# Enhanced Usage