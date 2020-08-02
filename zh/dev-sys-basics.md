---
title: 软件包维护：入门
description: 了解 AOSC OS 打包流程
published: true
date: 2020-08-02T15:31:33.313Z
tags: 
editor: markdown
---

**注意**：这篇指南假定你对 Linux 和它的命令行界面有一定的认识。此外，你还需要有一台你本人拥有 `root` 访问权限的装有 Linux 的电脑。

# 需要到的工具

我们需要这些工具来构建软件包。先不用纠结怎么用它们，我们会在后面提及。

  - [Ciel](https://github.com/AOSC-Dev/ciel/)
      - 用于管理 systemd-nspawn(1) 容器。
  - [ACBS](https://github.com/AOSC-Dev/acbs/)
      - 用于管理软件包树（例如我们的 [aosc-os-abbs](https://github.com/AOSC-Dev/aosc-os-abbs)）。
      - 可以调用 Autobuild3 以读取并构建指定的软件包。
  - [Autobuild3](https://github.com/AOSC-Dev/autobuild3/)
      - 用于读取软件信息并运行构建脚本。
  - [pushpkg](https://github.com/AOSC-Dev/scriptlets/tree/master/pushpkg)
      - 将构建好的软件包推送到官方软件仓库。 

# 版本发布模式

AOSC OS 采用的是半滚动更新模型，通常每个发布周期都为时三个月。这意味着 AOSC OS 和 Arch Linux 等滚动发行版一样是没有版本号的。但是，在 [aosc-os-abbs](https://github.com/AOSC-Dev/aosc-os-abbs) 树中，[AOSC OS Core](https://github.com/AOSC-Dev/AOSC-os-abbs/blob/testing-proposed/README.CORE.md) 是由一组软件包构建而成的，这组软件包由运行时库（例如 GNU C Library）和工具链（例如 GCC）组成。这组包以版本化的方式更新（Core 7.0.1、7.0.2、7.1.1 等）。此外，AOSC OS 软件仓库中所有更新都需要在一个 `*-proposed` 软件仓库中作经过一段时间的测试。 

我们有两个主要分支，分别是 `stable` 和 `testing`，还有三个开发分支 `stable-proposed`、`testing-proposed` 和 `explosive`。

`stable-proposed` 始终接受新的更新，但只限于补丁版本（也就是说版本号 `x.y.z` 中只有 `z` 变动了）、安全更新、漏洞修复，还有 [一些例外](https://wiki.aosc.io/developers/aosc-os/cycle-exceptions)，这个分支的更新每周都会被合并到 `stable` 分支。

`testing-proposed` 通常是新软件包和主要版本更新最先上传的地方，我们的工作主要在这里开展。开发工作遵循三个月一周期的迭代计划（可以参考 [Winter 2020 的迭代计划](https://github.com/AOSC-Dev/AOSC-os-abbs/issues/2073))。在前两个月，开发人员会构建新软件包或主要版本更新，随后在 `testing-proposed` 进行测试。 

在最后一个月的开头，`testing-proposed` 会被合并到 `testing`。在这个月，启用 `testing` 软件仓库的用户将收到更新，并帮助测试它们。如果一切顺利，到月底，`testing` 就会被合并到 `stable`，从而完成整个周期。在这段时间内，`testing-proposed` 分支则被冻结了。

`explosive` 则像一个实验田，通常用于放置不适合当前周期的软件包和更新。在 `testing-proposed` 冻结期间，开发人员可能会提前向这一分支推送更新，因为 `explosive` 会在新周期开始时被合并到 `testing-proposed`。

# 配置开发环境

首先我们要在电脑上安装 `ciel`。在 AOSC OS，直接在官方软件仓库获取并安装即可。Ciel 管理的是标准化的 AOSC OS 构建环境（或者说 [BuildKit](https://aosc.io/downloads/#buildkit)），而构建的流程不一定要在 AOSC OS 上进行，如果你在使用 Arch Linux，你也可以在 AUR 获取 Ciel。

接下来，我们会初始化一个 Ciel 的工作区，这里我们会使用 `~/ciel` 作为演示。请注意你需要使用 `root` 运行 Ciel 而且你不能在 Docker 容器里运行 Ciel。

``` bash
mkdir ~/ciel
cd ~/ciel
ciel init
```

接下来我们部署 BuildKit。BuildKit 是一个最小化的 AOSC OS 变体，专门用于打包或容器化开发。它包含了 ACBS 和 Autobuild3，因此我们不需要做额外的配置。 

``` bash
ciel load-os
# Or if you have already downloaded BuildKit
ciel load-os PATH_TO_BUILDKIT
```

接下来我们强烈推荐你将 BuildKit 更新到最新状态（AOSC OS 打包者则必须这样做）。

``` bash
ciel update-os
```

下一步，我们加载 ACBS 树。这里我们加载的是我们的 `aosc-os-acbs` 树。

``` bash
ciel load-tree # By default, ciel will load the official tree.
# Or, you can just clone the desired repository to ciel/TREE
```

# 构建我们的第一个软件包！

Now that we have a build environment set-up, we can try to build a package that is already in the tree. Let's start with a relatively trivial one, `extra-multimedia/flac`.

Before that, we need to create a Ciel instance. It is recommended to use separate instances for different branches. Run:

``` bash
ciel add stable # Since we are going to build on stable
```

And make sure we are actually on the stable branch.

``` bash
cd TREE
git checkout stable
cd ..
```

Then, we need to configure Ciel to use the correct repositories. In order to prevent incorrect dependencies, the build environment should use packages that matches the branch (with the exception of `stable-proposed`, which will only use dependencies from `stable`). For example, we need `stable` repository to build `stable` tree, and `testing`, `stable-proposed`, and `stable` to build `testing` packages.

``` bash
ciel config -i stable
```

First enter your info, whether to enable DNSSEC. And when ciel ask if you want to edit `source.list`, say yes, and modify.

``` INI
# For building stable packages
deb https://repo.aosc.io/debs stable main

# For building testing packages
deb https://repo.aosc.io/debs testing main
deb https://repo.aosc.io/debs stable-proposed main
deb https://repo.aosc.io/debs stable main

# And you get the idea.
```

Now we can actually build the package\! Simply type:

``` bash
ciel build -i stable flac
# -i is used to select the instance used to build
```

If the build completes without error, and a `Build Summary` is present, congratulations on your first successful build\! You should be able to find the generated deb inside `OUTPUT/debs`.

# 添加一个新的软件包

But surely you won't be satisfied by simply building existing packages, right? Here we will discover how to construct a new package from scratch.

Dive into the `TREE` folder, you will find a lot of categories of folders, including some beginning with `base-` and `core-` prefixes, as well as some with `extra-`. These folders are for organizing purposes, and inside them you will find the various packages (and their build specifications) organised in each of their own directory.

We will use `i3` as an example. This package can be found at `TREE/extra-wm/i3` for obvious reasons. Upon entering the directory, you should see a file structure as follows:
``` bash
    .
    ├── autobuild
    │   ├── beyond
    │   ├── conffiles
    │   ├── defines
    │   ├── overrides
    │   │   └── usr
    │   │       ├── bin
    │   │       │   └── i3exit
    │   │       └── share
    │   │           └── pixmaps
    │   │               └── i3-logo.svg
    │   ├── patches
    │   │   └── 0001-Use-OVER-operator-for-drawing-text.patch
    │   └── prepare
    └── spec
```

We will go through which each file is for.

## `spec`

This file is responsible for telling `acbs` where to download the source file, and the package's version and revision. A basic `spec` file should look like this:

``` bash
VER=4.17.1  # Version of the software.
# REL=0 The package revision. If not specified, it's 0.
SRCTBL="https://i3wm.org/downloads/i3-$VER.tar.bz2" # Download address for the source code.
CHKSUM="sha256::1e8fe133a195c29a8e2aa3b1c56e5bc77e7f5534f2dd92e09faabe2ca2d85f45" # Checksum of the source tarball.
```

One thing worth noting is the revision number. You can ignore this line if you are creating a new package, but sometimes (like applying an emergency security patch), the version number is not changed, but we still need to inform the package manager on users computer that there is an update available. In these circumstances, just increase the `$REL` variable by 1.

## `autobuild/`

This is the directory where all the `Autobuild3` scripts and definitions live. `Autobuild3` is a sophisticated build system that can automatically determine a series of build-time processes, like which build system to use, which build parameter to use, and so on.

## `autobuild/defines`

This file contains the core configuration like:

  - `PKGNAME` : Package name.
  - `PKGDES` : Package description.
  - `PKGSEC` : Section (or category) where the package belongs to.
  - `PKGDEP` : Package dependencies.
  - `PKGCONFL` : Package conflicts.
  - `BUILDDEP` : Build dependencies (packages which are required during build-time, but not for run-time).
  - `PKGRECOM` : Recommended dependencies, installed automatically, but could be removed by user discretion.

These are only the most common configuration entries. There are much more configurations, but if the software is fairly standard, these configuration should be enough. Other information like which C compiler flags to use, which build system to use, can be filled automatically by `Autobuild3`.

Here is a basic example taken from `TREE/extra-multimedia/i3`:

``` bash
PKGNAME=i3
PKGSEC=x11
PKGDEP="dmenu libev libxkbcommon pango perl-anyevent-i3 perl-json-xs \
        startup-notification xcb-util-cursor xcb-util-keysyms \
        xcb-util-wm yajl xcb-util-xrm"
PKGRECOM="i3lock i3status"
BUILDDEP="graphviz doxygen xmlto"
PKGDES="Improved tiling WM (window manager)"

PKGCONFL="i3-gaps"
```

Notice here that you can actually write bash logic inside `defines`. This is useful when adding platform-specific flags or dependencies, but this is **NO LONGER** recommended, and will be prohibited in the future. For adding platform specific info, use `$VAR__$ARCH`.

For a complete list of available parameters, visit [Wiki for Autobuild3](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3).

## `autobuild/prepare`

This file is the script that will be executed before the build process begins. Usually it is used to prepare files or set environment variables used in the build process.

## `autobuild/patches/`

This is a directory containing all the patches that will be applied to the source codes before the build.

Simple as dropping it in. :)

# 一个完整的例子：`light`

That's all the basic knowledge you need to build a simple package\! Now, we will try to build a really simple program: [light](https://github.com/haikarainen/light).

This program is used to provide a easy command to control the backlight of laptop. Since it only uses file API to interact with the backlight subsystem, this program is very simple and does not require and dependency other than `glibc`.

Return to the `TREE` directory (assuming you have Ciel set-up). First, make sure that you are on the right branch. As mentioned above, during the first two months of the cycle, use `testing-proposed`. For the last month, use `explosive`.

Since this program is obviously a utility, we create a directory called `light` under the directory `TREE/extra-utils`.

``` bash
cd TREE/extra-utils
mkdir light
cd light
```

Then, we create the `spec` file. Look up the project website and find out the download URL for the latest version. After manually checking the `sha256` checksum of the latest tarball, we can fill in the file.

``` bash
VER=1.2.1
SRCTBL="https://github.com/haikarainen/light/archive/v$VER.tar.gz"
CHKSUM="sha256::53d1e74f38813de2068e26a28dc7054aab66d1adfedb8d9200f73a57c73e7293"
```

Notice here that we replaced the version number inside the tarball URL with an environment variable `$VAR`. This is considered as a good practice (since it reduces the modification required when updating the package), and should be used when possible.

Then, we create the `autobuild` folder, and create the `defines` file.

Since this is an application used in the GUI environment, we give it the section of `x11`. The complete `defines` file looks like the following:

``` bash
PKGNAME=light
PKGSEC=x11
PKGDES="Program to easily change brightness on backlight-controllers."
```

And we are done\! We can now head back to the base directory of the Ciel environment (`~/ciel`, and run the following command:

``` bash
ciel build -i stable light
```

Although we didn't write anything about how to build this program, `Autobuild3` automatically figured out that this should be built with `autotools` (i.e., the classic `./configure && make && make install` logic), and should build the program successfully. If you want to double check, use `dpkg-deb -c DEB_FILE` to check the files inside the deb file.

## Git conventions

AOSC OS has strict conventions about git logs. We will only mention the most used ones here. For the full list of package styling and development guidelines, please refer to the *<https://wiki.aosc.io/en/dev-sys-package-styling-manual>*.

For example, we are adding a new package to the tree. Then the log should be something like this:

    light: new, 1.2.1
    $PKG_NAME: new, $VER

If you are updating the version of an exisiting package, it should be like this:

    bash: update to 5.2
    $PKG_NAME: update to $NEW_VER

And please mention all the specific changes made to the package (i.e., dependency changes, feature enablement, etc.) in the long log, for instance:

    bash: update to 5.2
    
    - Make a symbolic link from /bin/bash to /bin/sh for program compatibility.
    - Install HTML documentations.
    - Build with -O3 optimisation.

## Pushing packages to the repository

After a successful build, maintainers will push local Git changes to the tree, and the respective packages to the official repository.

The second task can be done using [pushpkg](https://github.com/AOSC-Dev/scriptlets/tree/master/pushpkg). Grab the script, add the script to PATH, make sure it is executable (0755). Then, invoke `pushpkg` inside the `OUTPUT` directory. You will need to provide your LDAP credentials and the destination repository (`stable`, `testing`, etc.).

# 后记

That's it\! You have learned the basics about creating new packages for AOSC OS from scratch, as well as how to update, build, and uploading them\!

However, as you may see, this article only covers the basics of what you need to know as you continue to prime for further involement in AOSC OS maintenance. When dealing with more complicated build systems, or updating a batch of packages, there's still many skills to learn. Please refer to the [Way to AOSC OS Maintainence: Advanced Techniques](/en/dev-sys-advanced-techniques)
