---
title: AOSC OS 软件包样式指南
description: 过好生活，打美包儿包儿
published: true
date: 2020-08-04T03:13:17.895Z
tags: dev-sys
editor: markdown
---

# 概述

[AOSC OS ABBS Tree](https://github.com/AOSC-Dev/aosc-os-abbs/) 现在有超过 5000 个包，这些包由 20 多个工具人维护。有些软件包已经有着五年的历史，问题也随之出现：

- 软件包依赖项缺失。
- 构建脚本中的变量没加引号。
- 构建脚本缺少注释，晦涩难懂。
- 软件包描述（`$PKGDES`）不清晰。
- 软件包分类（`$PKGSEC`）不正确。
- ... 

如果没有一个全面的指导方针在未来的打包工作，问题只会继续增加，这也是本文诞生的目的。

# 打包者、软件包名称与说明

本小节我们会讲到描述一个 AOSC OS 软件包的三大利器。

## 打包者

Debian 软件包（`.deb`）中的打包者或维护者信息应该按照以下格式填写：

```
Executed Packager <suffering@pakreq.work>
```

## 软件包名称

软件包名称（`PKGNAME=` 或 `$PKGNAME`）应为小写且应当符合包管理器对包名称的其它规定。

## 软件包说明

软件包说明（`PKGDES=` 或 `$PKGDES`）在格式上有以下的要求： 

- 使用大小字母开头。
- 结尾没有句号或其它标点符号。



值得注意的是，AOSC OS 软件包的包描述应该是描述性的，而不是定义性的。可接受的包描述应该类似于： 

    "Library with common API for various MATE modules"
		
这样写描述性就降低了：

    "MATE Desktop Library"

这样写定义性就太强了：

    "Library with concise and convenient API for various MATE modules"
    
# Spec 文件

在使用 [ACBS](https://github.com/AOSC-Dev/acbs)（Autobuild CI Build Service）时，会在名为 `spec` 的文件中定义多种变量，下面我们将讨论这些变量。 

## 软件包版本

软件包版本变量定义了软件包的版本号和修订号。

### VER=

`VER=` 或 `$VER` 变量定义软件包的主版本号。在为 AOSC OS 打包时，打包者应遵守下列规则。

| 情形 | 应当采取的措施 | 举例 |
|-------------------|-------------------------------------|-------------------|
| 版本号只带有半角句号 | 直接使用上游的版本号 | GNOME Clocks 3.32.1 -> `VER=3.32.1` |
| 版本号带有字母 | 将字母转为小写，将字母相邻的标点移除 | Bind 9.12.3-P4 -> `VER=9.12.3p4` |
| 版本号带有短横线（`-`） | 将短横线替换为加号（`+`） | ImageMagick 6.9.10-23 -> `VER=6.9.10+23` |
| 版本号带有下划线 (`_`) | 将下划线替换为半角句号（`.`） |  Icarus Verilog 10_2 -> `VER=10.2` |
| 版本号带有发布阶段标记（`alpha`、`beta`、`rc`） | 将字母转为小写，除 `rc` 外只保留第一个字母。将字母相邻的标点移除，并加一个波浪号（`~`）。 | Golden Dict 1.5.0-RC2 -> `VER=1.5.0~rc2` |
| 版本号基于日期或版本控制系统的提交哈希值 | 直接写日期（不要包含 `git` 或 `svn` 等），但要确保源码包始终能被获取，参考 `GITCO`| Shadowsocks 5ff694b2c2978b432918dea6ac104706b25cbf48 -> `VER=20181219` |

### REL=

`REL=` 或 `$REL` 变量定义软件包的修订号。此变量只应使用一个正整数作为其值。 

## 源码文件

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

`$CHKSUM` will become a required value in the near future.

### 版本控制系统变量

VCS (Version Control System) based sources may use any one of the each combinations.

| VCS | Required Variables | Additional Notes |
|--------|-------------------------------|---------------------------|
| Bazaar (BZR) | `BZRSRC=`, or `$BZRSRC`, which defines the Bazaar repository; `BZRCO=`, or `$BZRCO`, which defines the specific Bazaar revision. | |
| Git | `GITSRC=`, or `$GITSRC`, which defines the Git repository; `GITCO=`, or `$GITCO`, which defines the specific Git "checkout(s)" (a commit, or a tag) | Use Git over Hypertext Transfer Protocol Secure (HTTPS, https://) where possible. |
| Mercurial (HG) | `HGSRC=`, or `$HGSRC`, which defines the Meruciral repository. | Avoid using Mecurial source(s) where possible, as support for checking out a specific revision is not yet implemented. |
| Subversion (SVN) | `SVNSRC=`, or `$SVNSRC`, which defines the SVN repository; `SVNCO=`,  or `$SVNCO`, which defines the specific SVN checkout(s) (revision). | |

### DUMMYSRC=

The `DUMMYSRC=`, or `$DUMMYSRC` variable is used when a package is empty (meta package), or uses custom-generated sources. This variable takes a bolean value.

### 其它变量

Other variables may be used, so long as they are not any of the variables listed above. These variables are often used to aid with manipulating `$SRCDIR`, here's the `spec` file of `extra-devel/netbeans`, for instance.

```
VER=8.2
REL=1
SUB=201609300101
SRCTBL="http://download.netbeans.org/netbeans/$VER/final/zip/netbeans-$VER-$SUB.zip"
SUBDIR=.
```

# 依赖项

In the context of AOSC OS packaging, dependencies are arranged in two categories: run-time depedencies, and build-time dependencies. These dependencies are defined by `PKGDEP=` (`$PKGDEP`), and `BUILDDEP=` (`$BUILDDEP`), respectively.

## Run-time Dependencies

Run-time dependencies should be written in such a way that, not only does the package function (programs run, libraries link, etc.), all linkages to the package should also be included. In the case of `extra-multimedia/ario`, for instance, not only should `$PKGDEP` contain the following dependencies:

`avahi, curl, dbus-glib, gnutls, hicolor-icon-theme, libglade, libmpdclient, libnotify, libsoup, libunique, taglib, xdg-utils`

Which, through explicit and implicit dependencies, allows for a system environment that contains sufficient runtime for the program `/usr/bin/ario` to function.

By the quality assurance standard, defined in code [E432](/dev-sys-qa-issue-codes#class-4-dependencies), all direct dependencies on the ELF level should also be included in `$PKGDEP`, and thus the addition of `dbus` to `$PKGDEP` is necessary.

As of March 16th, 2019, 42.4% (1592/3705) of all packages provided in the Stable channel for `amd64` has the issue of insufficient ELF dependencies.

### Additional Notes

- When a package has a sole dependency on the GCC Runtime (`gcc-runtime`) or the GNU C Library (`glibc`), these dependencies should be included in `$PKGDEP`.

## Build-time Dependencies

Build-time dependencies should written in such a way that the package will compile, install, and package successfully in the BuildKit build environment. Given this, any packages included in the BuildKit environment will not need to be included in `$BUILDDEP`. For example...

- CMake (`cmake`) is required for building `extra-devel/extra-cmake-modules`, however, `cmake` is an integral part of BuildKit. Therefore, packagers are not required to include `cmake` in `$BUILDDEP`.
- Bazel (`bazel`) is required fo building `extra-scientific/tensorflow`. In this case, `bazel` must be included in `$BUILDDEP`, as `bazel` is not installed in BuildKit as standard.

# 软件包特性

When packaging for AOSC OS, please work in accordance to our [distribution feature guide](/sys-is-aosc-os-right-for-me). The table below digests some of the common considerations when building packages for AOSC OS.

| Considerations | Appropriate Actions |
|-------------------------|---------------------------------|
| Features | Enable all features, unless a feature is unmaintained, or violates any of the other considerations in this table. |
| Language packs (dictionaries, locale data, etc.) | Language packs must be included in the same package as the main executables, etc. | 
| Splitting packages | Packages are to be remained intact, unless package comes in multiple flavours, or otherwise agreed upon by the developer majority. |
| Telemetry | All telemetry functionalities must be stripped or disabled by default (opt-in), packages that do not function without such feature should only be accepted on a case-by-case basis (rejected by default). |
| Update checking | All update checking (notification, downloading, etc.) functionalities must be stripped, packages that do not function without such feature should only be accepted on a case-by-case basis (rejected by default). |

# 脚本编写

While most packages could be built with one of the pre-defined [Autobuild Types](https://github.com/AOSC-Dev/autobuild3/tree/master/build) (`$ABTYPES`), and that patches could be applied automatically from the `autobuild/patches` directory (or via a pre-defined `series` file to specify patch order), some packages require manual preparation, patching, and build. This section is dedicated to `prepare`, `patch`, `build`, and `beyond` under the `autobuild/` directory.

A general rule of thumb is to write such scripts secure (quoted) variables, sufficient comments, error control, architectural considerations, progression report, ... Writing easy-to-read and reliable build scripts is not easy, and the table below aims to aid you with making good scripting decisions.

| Criteria | Required/Recommended | Explanations |
|-------------|----------------------------------------|----------------------|
| Autobuild3 Build Templates (`ABTYPE`) | Required | Packager should utilise [Autobuild Types](https://github.com/AOSC-Dev/autobuild3/tree/master/build) where possible, without using `autobuild/build` or `ABTYPE=self`. |
| Error Handling | Required | Build errors should be captured and handled appropriately. By default, errors are handled automatically by Autobuild3 and will result in aborted build, however, `autobuild/build` is not yet covered due to a bug in Autobuild3. |
| Progression report | Requried | Progress should be reported by appropriately employing `abinfo` and `abwarn` wrappers, this is required for packages utilising the `autobuild/build`, or `ABTYPE=self`. |
| Citations and References | Required | When adapting/copying build scripts from other distributions, packager must include a comment indicating the source(s) of the build script(s) |
| Secure Variables | Required | Variables should be quoted, for example, all `"$SRCDIR"` and `"$PKGDIR"`. |
| File Directories | Required | All files manually installed from the source tree must be referenced with absolute paths, for instance, `"$SRCDIR"/desktop/foo.desktop`. |
| Architectural Considerations | Recommended | While it is convenient to write build scripts adapted to the `amd64` port, it is important to note that AOSC OS builds packages for more than five other architectures using the same scripts. |
| Comments | Recommended | Good scripts tend to be well commented. However, comments can be replaced with progression report clauses, see "Progression report". |

As many packagers tend to reference or copy build scripts from Arch Linux, please reference the [TODO: AOSC OS-Arch Rosetta Stone](#) for a comprehensive guide on translating PKGBUILD (Arch Linux) into Autobuild3 manifests (AOSC OS).

# 补丁命名

Patches should follow a (mostly) uniform file naming for clear arrangement and sorting, before they are included in `autobuild/patches/`.

## Git-based Sources

When dealing with Git-based sources, it is possible to create numbered patches from the following command:

```
git format-patch -n $HASH
```


Where `n` defines the amount of commits from the specific commit `$HASH`, including the specified commit. Alternatively, you can omit the `$HASH`...

```
git format-patch -n
```

To create a series of patches from `n` commits to the branch `HEAD`.

These commands generate a series patches like the following...

```
0001-contrib-autobuild-aoscarchive-one-more-syntax-fix.patch
0002-common_switches-add-sanitizer-support.patch
0003-contrib-autobuild-aoscarchive-fix-overlay-subdir-che.patch
0004-arch-_common_switches-fix-syntax.patch
0005-autobuild-aoscarchive-adapt-to-new-workflow.patch
```

## Other Sources

Without an automatic mean to generate patches, patches should be named in the following format.

```
NNNN-$CATEGORY-$CONTENT.patch
```

Where:

- `NNNN`, like the sample patch file names, is a "serial" number for sorting patches. 
- `$CATEGORY` defines the category of a patch, for instance, `bugfix`, `feature`, etc.
- `$CONTENT` defines "what is to be done" when a patch is applied, for instance, `fix-build-with-openssl-1.1`.

Likewise, when including patch(es) from other distributions, they should also be renamed in accordance to the guidelines above.

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

# Git 提交信息

When committing (or contributing, if you like) to the [AOSC OS ABBS Tree](https://github.com/AOSC-Dev/aosc-os-abbs/), please observe the commit message standards, shown in the table below.

| Action | Message Formatting | Sample Commit Message |
|-----------|-----------------------------------|-----------------------------------------|
| Introducing a new package | `$PKGNAME: new, $PKGVER` | `windowsnt-kernel: new, 5.1.2600` |
| Security fixes with version update | `$PKGNAME: update to $PKGVER; #NNN` | `bash: update to 5.2; #114514`, where `#114514` is a reference to the original security report (GitHub issue) |
| Security fixes without version update, utilising distribution patch(es) | `$PKGNAME: ($DISTNAME patch[es], $CHANNEL) #NNN` | `gnome-shell: (Ubuntu patches, 18.10) #2333`, where `#2333` is a reference to the original security report (GitHub issue) |
| Security fixes without version update, utilising upstream patch(es) | `$PKGNAME: (upstream patch[es]) #NNN` | `audacious: (upstream patches) #1919`, where `#1919` is a reference to the original security report (GitHub issue) |
| Updating a package | `$PKGNAME: update to $PKGVER` | `mate-desktop: update to 1.22.0` |
| Work-in-progress with a fail-to-build package | `$PKGNAME: ... (FTBFS)` | `chromeos-desktop: update to 99.0.9999 (FTBFS)`, note that "FTBFS" stands for "Failed To Build From Source", this term is used loosely |
| Working with a package | `$PKGNAME: ...` | `kde-workspace: add qt-5 dependency`, just say what you did in present tense |
| Working with a package, multiple actions | `$PKGNAME: ...; ...` | `gnome-shell: add at-spi2-core dependency; update to 3.32.0` |
| Working with a package, utilising distribution patch(es) | `$PKGNAME: ($DISTNAME patch[es], $CHANNEL) ...` | `qt-4: (Arch Linux patches) rebuild for openssl` |
| Working with a package, utilising upstream patch(es) | `$PKGNAME: (upstream patch[es]) ...` | `kodi: (upstream patch) fix lock-up on start-up` |
| Working with a QA issue | `$PKGNAME: ... ($ISSUECODE)` | `psiconv: rebuild for imagemagick (E431)`, for a list of QA issue codes, refer to this [list](/developers/list-of-package-issue-codes) |
| Working with an architecturally-exclusive package | `$PKGNAME: ... ($ARCH)` | `google-chrome: new, 100.0.9999.999 (amd64)` |
| Working with an architecturally-independent package | `$PKGNAME: ... (noarch)` | `mate-common: update to 1.22.0 (noarch)` |

## Long Messages

When more than one of the actions were committed, and that the short message goes beyond 50 characters (including space and punctuation marks), you should utilise a "long Git commit message", for example:

```
firefox: update to 64.0.2; #1536
    
    - Enable PGO on AMD64, patches from Fedora and upstream.
    - Clean up defines.
    - Remove deprecated --enable-pie option.
    - More vendor-specific preferences to further limit Pocket integration and telemetry.
```