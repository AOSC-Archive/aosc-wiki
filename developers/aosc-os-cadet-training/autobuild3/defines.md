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

| Variable | Value Format | Usage | Essential? |
|--------------|----------------------|----------------|--------------|
| `PKGNAME=` | String, No Space | Defines the name of the package (i.e. `plasma-desktop`). | Yes |
| `PKGVER=` | String, No Space | Defines the version of the package (i.e. `5.16.4`). | Yes ¹ |
| `PKGREL=` | Integer | Defines the revision level of the package (i.e. the `1` in `5.16.4-1`). | Yes ¹ |
| `PKGDES=` | String | Defines the package's description (i.e. `Plasma interface for desktop computers`). | Yes |
| `PKGSEC=` | String, No Space | Defines the section in which the package belongs, using sections from the [pre-defined list of sections](https://github.com/AOSC-Dev/autobuild3/blob/master/sets/section) is recommended. | Yes |
| `PKGDEP=` | String, Space Separated | Defines the list of dependency(ies) of the package, defaults to `>=` the newest version available from the repository. | No |
| `BUILDDEP=` | String, Space Separated | Defines the list of build-time dependency(ies) of the package, *not required when end-users install the package*. | No |
| `PKGRECOM=` | String, Space Separated | Defines the list of "soft dependencies" of the package, these dependencies will be automatically installed when available, but can be removed later by end-users' discretion. | No |
| `PKGSUG=` | String, Space Separated | Defines the list of "suggested extras" of the package, package listed in this variable will *not* be installed by default, but end-users will be notified by their package manager(s). | No |
| `PKGBREAK=` | String, Space Separated | Defines the list of package(s) that the current package version "breaks" (ABI, API, user interface, etc.). | No |
| `PKGCONFL=` | String, Space Separated | Defines the list of package(s) that the current package version conflicts with (i.e. cannot be installed concurrently). | No |
| `PKGPROV=` | String, Space Separated | Defines the list of package(s) that the package provides or serves the alias to (i.e. `nano` can `PKGPROV` the package `default-editor`). | No |
| `PKGREP=` | String, Space Separated | Defines the list of package(s) that the package replaces or obseletes (i.e. `gnome-shell` is a new set of technologies that replaces `gnome-desktop2`). | No |

# Build Parametres

# Enhanced Usage