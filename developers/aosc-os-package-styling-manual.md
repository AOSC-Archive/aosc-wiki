<!-- TITLE: AOSC OS Package Styling Manual -->
<!-- SUBTITLE: Comprehensive (and updated) Styling Manual for AOSC OS Packagers -->

# Introduction and Rationale

The [AOSC OS ABBS Tree](https://github.com/AOSC-Dev/aosc-os-abbs/) is now home to over 5000 (and counting) packages, which are maintained by over 20 (historical and current) packagers. With more than 5 years of history behind some packages, issues began to emerge in the following fashion:

- Packages lack dependencies.
- Insecure (unquoted) variables in build scripts.
- Hard-to-read build scripts, with little comments or indication of progression.
- Undescriptive package description(s) (`$PKGDES`).
- Non-standard package section(s) (`$PKGSEC`).
- ...

Without a comprehensive guildeline for packaging work in the future, these issues will not go away - rather, the list will grow longer and further degrade package qualities in our distribution. This Styling Manual seeks to provide sectioned and targetted sets of guidelines for fixing existing and new packages.

# Package Specs

In the current implementation of [ACBS](https://github.com/AOSC-Dev/acbs) (AOSC CI Build Service), three categories of variables are defined in a file named `spec` - these variables will be discussed below.

## Versioning Variables

Versioning variable define the package's version and revision levels. 

### VER=

The `VER=`, or `$VER` variable defines the main version of the resulting package. When packaging for AOSC OS, packagers should take note of the following requirements. These requirements are presented in the table below.

| Situations | Appropriate Actions | Examples |
|-------------------|-------------------------------------|-------------------|
| "Normal" versioning, with only "dot" separators | Retain version, as defined by the upstream | GNOME Clocks 3.32.1 -> `VER=3.32.1` |
| Versions with letter notation(s) | Lower-case all letter(s), and remove symbols surrounding the letter(s) | Bind 9.12.3-P4 -> `VER=9.12.3p4` |
| Versions with dash(es) ("-") | Replace the dash(es) with plus ("+") sign(s) | ImageMagick 6.9.10-23 -> `VER=6.9.10+23` |
| Versions with underscore(s) ("\_") | Replace the underscore(s) with dot(s) (".") |  Icarus Verilog 10_2 -> `VER=10.2` |
| Versions with release stage notation(s) ("alpha", "beta", "rc", etc.) | Lower-case all notations, "Beta" to "beta", etc. Replace "alpha" with "a", "beta" with "b", retain "rc". Remove all symbols surrounding the notation(s) | Golden Dict 1.5.0-RC2 -> `VER=1.5.0rc2` |
| Git or other date-based snapshots | Simply write the date (dot not include `git`, or `svn`, etc. notation(s), but ensure consistent source(s) can be downloaded, see `GITCO`, etc. | Shadowsocks 5ff694b2c2978b432918dea6ac104706b25cbf48 -> `VER=20181219` |

### REL=

The `REL=`, or `$REL` variable defines the revision level of the resulting package. This variable should only hold a single positive integer as its value.

## Source Variables

Source variables define the package's source(s), and in the case of a VCS (version control system) based source, define in addition a specific source snapshot.

### SRCTBL=

The `SRCTBL=`, or `$SRCTBL` variable is used when a package's source is released in the form of a single compressed archive. Requirements and recommendations are presented in the table below.

| Criteria | Required/Recommended | Appropriate Actions |
|-------------|----------------------------------------|---------------------------------|
| URI schemes | Recommended | Use Hypertext Transfer Protocol Secure (HTTPS, https://) where possible. Avoid non-secure connections (http://) and plain FTP (File Transfer Protocol, ftp://). |
| Source format | Recommended | Use XZ-compressed Tar-Archives (.tar.xz) where possible, other formats are considered appropriate. Avoid the inefficient BZip2-compressed Tar-Archives (.tar.bz2) where possible. |
| Version substitutions | Required | Source links must replace all versions with substitutions from the `$VER` variable (see above). `SRCTBL=` must not be defined with hard-coded version(s). |
| Versioned tarballs | Required | Source archives (tarballs) must be versioned in order to ensure consistency. |

### CHKSUM=

The `CHKSUM=`, or `$CHKSUM` variable is used in conjunction with `$SRCTBL`, to define the proper checksum for specific source archive(s). The format is as follows.

```
CHKSUM="$ALGORITHM::$CHECKSUM"
```

Where...

- `$ALGORITHM` is to be replaced with specific hashing algorithms, written in lower-case. For instance, `sha256`.
- `$CHECKSUM` is to be replaced with the corresponding hash checksum of the aforementioned algorithm.

For descriptions of different cryptographic hash algorithms, and for identification of insecure (and therefore unacceptable) algorithms, refer to this [Wikipedia chapter](https://en.wikipedia.org/wiki/Cryptographic_hash_function#Cryptographic_hash_algorithms) under "Cryptographic Hash Function".

### VCS Variables

VCS (Version Control System) based sources may use any one of the each combinations.

| VCS | Required Variables | Additional Notes |
|--------|-------------------------------|---------------------------|
| Bazaar (BZR) | `BZRSRC=`, or `$BZRSRC`, which defines the Bazaar repository; `BZRCO=`, or `$BZRCO`, which defines the specific Bazaar revision. | |
| Git | `GITSRC=`, or `$GITSRC`, which defines the Git repository; `GITCO=`, or `$GITCO`, which defines the specific Git "checkout(s)" (a commit, or a tag) | Use Git over Hypertext Transfer Protocol Secure (HTTPS, https://) where possible. |
| Mercurial (HG) | `HGSRC=`, or `$HGSRC`, which defines the Meruciral repository. | Avoid using Mecurial source(s) where possible, as support for checking out a specific revision is not yet implemented. |
| Subversion (SVN) | `SVNSRC=`, or `$SVNSRC`, which defines the SVN repository; `SVNCO=`,  or `$SVNCO`, which defines the specific SVN checkout(s) (revision). | |

### DUMMYSRC=

The `DUMMYSRC=`, or `$DUMMYSRC` variable is used when a package is empty (meta package), or uses custom-generated sources. This variable takes a bolean value.

### Other Variables

Other variables may be used, so long as they are not any of the variables listed above. These variables are often used to aid with manipulating `$SRCDIR`, here's the `spec` file of `extra-devel/netbeans`, for instance.

```
VER=8.2
REL=1
SUB=201609300101
SRCTBL="http://download.netbeans.org/netbeans/$VER/final/zip/netbeans-$VER-$SUB.zip"
SUBDIR=.
```

# Dependencies

In the context of AOSC OS packaging, dependencies are arranged in two categories: run-time depedencies, and build-time dependencies. These dependencies are defined by `PKGDEP=` (`$PKGDEP`), and `BUILDDEP=` (`$BUILDDEP`), respectively.

## Run-time Dependencies

Run-time dependencies should be written in such a way that, not only does the package function (programs run, libraries link, etc.), all linkages to the package should also be included. In the case of `extra-multimedia/ario`, for instance, not only should `$PKGDEP` contain the following dependencies:

`avahi, curl, dbus-glib, gnutls, hicolor-icon-theme, libglade, libmpdclient, libnotify, libsoup, libunique, taglib, xdg-utils`

Which, through explicit and implicit dependencies, allows for a system environment that contains sufficient runtime for the program `/usr/bin/ario` to function.

By the quality assurance standard, defined in code [E432](https://wiki.aosc.io/developers/list-of-package-issue-codes#class-4-dependencies), all direct dependencies on the ELF level should also be included in `$PKGDEP`, and thus the addition of `dbus` to `$PKGDEP` is necessary.

As of March 16th, 2019, 42.4% (1592/3705) of all packages provided in the Stable channel for `amd64` has the issue of insufficient ELF dependencies.

### Additional Notes

- When a package has a sole dependency on the GCC Runtime (`gcc-runtime`) or the GNU C Library (`glibc`), these dependencies should be included in `$PKGDEP`.

## Build-time Dependencies

Build-time dependencies should written in such a way that the package will compile, install, and package successfully in the BuildKit build environment. Given this, any packages included in the BuildKit environment will not need to be included in `$BUILDDEP`. For example...

- CMake (`cmake`) is required for building `extra-devel/extra-cmake-modules`, however, `cmake` is an integral part of BuildKit. Therefore, packagers are not required to include `cmake` in `$BUILDDEP`.
- Bazel (`bazel`) is required fo building `extra-scientific/tensorflow`. In this case, `bazel` must be included in `$BUILDDEP`, as `bazel` is not installed in BuildKit as standard.

# Scripting

While most packages could be built with one of the pre-defined [Autobuild Types](https://github.com/AOSC-Dev/autobuild3/tree/master/build) (`$ABTYPES`), and that patches could be applied automatically from the `autobuild/patches` directory (or via a pre-defined `series` file to specify patch order), some packages require manual preparation, patching, and build. This section is dedicated to `prepare`, `patch`, `build`, and `beyond` under the `autobuild/` directory.

A general rule of thumb is to write such scripts secure (quoted) variables, sufficient comments, error control, architectural considerations, progression report, ... Writing easy-to-read and reliable build scripts is not easy, and the table below aims to aid you with making good scripting decisions.

| Criteria | Required/Recommended | Explanations |
|-------------|----------------------------------------|----------------------|
| Autobuild3 Build Templates (`ABTYPE`) | Required | Packager should utilise [Autobuild Types](https://github.com/AOSC-Dev/autobuild3/tree/master/build) where possible, without using `autobuild/build` or `ABTYPE=self`. |
| Error control | Required | Build errors should be captured and handled appropriately. By default, errors are handled automatically by Autobuild3 and will result in aborted build, however, `autobuild/build` is not yet covered due to a bug in Autobuild3. |
| Progression report | Requried | Progress should be reported by appropriately employing `abinfo` and `abwarn` wrappers, this is required for packages utilising the `autobuild/build`, or `ABTYPE=self`. |
| References | Required | When adapting/copying build scripts from other distributions, packager must include a comment indicating the source(s) of the build script(s) |
| Secure variables | Required | Variables should be quoted, for example, all `"$SRCDIR"` and `"$PKGDIR"`. |
| Architectural considerations | Recommended | While it is convenient to write build scripts adapted to the `amd64` port, it is important to note that AOSC OS builds packages for more than five other architectures using the same scripts. |
| Comments | Recommended | Good scripts tend to be well commented. However, comments can be replaced with progression report clauses, see "Progression report". |

As many packagers tend to reference or copy build scripts from Arch Linux, please reference the [TODO: AOSC OS-Arch Rosetta Stone](#) for a comprehensive guide on translating PKGBUILD (Arch Linux) into Autobuild3 manifests (AOSC OS).

# File Placements

AOSC OS, like many other Linux Distributions, expect packaged files to be located in appropriate directories. Please reference the *non-comprehensive* table below for our standard of file placements.

| Types of Files | Appropriate Placements |
|-----------------------|---------------------------------------|
| Binary or script executables | `/usr/bin` |
| Binaries run by other programs | `/usr/libexec`, unless hard-coded by other packages/components (GNOME, \*ahem\*) |
| Data files (no ELF, or architecturally-dependent scripts) | `/usr/share` |
| Daemon user home | `/var/lib/$COMPONENTNAME`, where an appropriate`$COMPONENTNAME` is decided in practice, for instance, `/var/lib/lightdm` |
| Go components and shared data | `/usr/share/gocode` |
| Headers (includes) | `/usr/include` |
| Java components (commons, etc.) | `/usr/share/java` |
| Libraries (shared and static) | `/usr/lib` |
| Licences | `/usr/share/doc/$PKGNAME` |
| Manpages | `/usr/share/man` |
| Non-manpage documentations | `/usr/share/doc/$PKGNAME` |
| Private libraries | `/usr/lib/$COMPONENTNAME`, where an appropriate`$COMPONENTNAME` is decided in practice, for instance, `/usr/lib/R` |

## Electron and Chromium-based Packages

Electron, Chromium, and other Chromium-based packages should be packaged with the following structure.

| Components | Appropriate Placements |
|---------------------|----------------------------------------|
| Binary executables | `/usr/bin`, where the executable is a symbolic link to its target in `/usr/lib/$PKGNAME` |
| Desktop, AppStream, and other data files | `/usr/share` |
| Main program data | `/usr/lib/$PKGNAME` |

## Binary Packaging (Binpack)

Binary packages should not be installed to `/opt`, unless the package's licence prohibits such file movement. With adjustments and other modifications, these packages should be installed to the `/usr` prefix - if packager find it impossible, they should consider rejecting such packages.