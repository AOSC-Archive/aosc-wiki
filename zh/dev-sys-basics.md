---
title: 软件包维护入门：基础
description: 了解 AOSC OS 打包流程
published: true
date: 2020-08-03T03:41:37.119Z
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
      - 可以调用 Autobuild3 以读取 PKGBUILD 并构建指定的软件包。
  - [Autobuild3](https://github.com/AOSC-Dev/autobuild3/)
      - 用于读取软件包的 PKGBUILD 并运行构建脚本。
  - [pushpkg](https://github.com/AOSC-Dev/scriptlets/tree/master/pushpkg)
      - 将构建好的软件包推送到官方软件仓库。 

# 版本发布模式

AOSC OS 采用的是半滚动更新模型，通常每个发布周期都为时三个月。这意味着 AOSC OS 和 Arch Linux 等滚动发行版一样是没有版本号的。但是，在 [aosc-os-abbs](https://github.com/AOSC-Dev/aosc-os-abbs) 树中，[AOSC OS Core](https://github.com/AOSC-Dev/AOSC-os-abbs/blob/testing-proposed/README.CORE.md) 是由一组软件包构建而成的，这组软件包由运行时库（例如 GNU C Library）和工具链（例如 GCC）组成。这组包以版本化的方式更新（Core 7.0.1、7.0.2、7.1.1 等）。此外，AOSC OS 软件仓库中所有更新都需要在一个 `*-proposed` 软件仓库中作经过一段时间的测试。 

我们有两个主要分支，分别是 `stable` 和 `testing`，还有三个开发分支 `stable-proposed`、`testing-proposed` 和 `explosive`。

`stable-proposed` 始终接受新的更新，但只限于补丁版本（也就是说版本号 `x.y.z` 中只有 `z` 变动了）、安全更新、漏洞修复，还有 [一些例外](https://wiki.aosc.io/developers/aosc-os/cycle-exceptions)，这个分支的更新每周都会被合并到 `stable` 分支。

`testing-proposed` 通常是新软件包和主要版本更新最先上传的地方，我们的工作主要在这里开展。开发工作遵循三个月一周期的迭代计划（可以参考 [Winter 2020 的迭代计划](https://github.com/AOSC-Dev/AOSC-os-abbs/issues/2073))。在前两个月，开发人员会构建新软件包或主要版本更新，随后在 `testing-proposed` 进行测试。 

在最后一个月的开头，`testing-proposed` 会被合并到 `testing`。在这个月，启用 `testing` 软件仓库的用户将收到更新，并帮助测试它们。如果一切顺利，到月底，`testing` 就会被合并到 `stable`，从而完成整个周期。在这段时间内，`testing-proposed` 分支则被冻结了。

`explosive` 则像一个实验田，通常用于放置不适合当前周期的软件包和更新。在 `testing-proposed` 冻结期间，开发人员可能会提前向这一分支推送更新，因为 `explosive` 会在新周期开始时被合并到 `testing-proposed`。

# 配置打包环境

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

好了现在我们已经把打包环境配置好，我们可以尝试构建一个已有的包。让我们从一个相对简单的例子开始，`extra-multimedia/flac`。

在此之前，我们需要创建一个 Ciel 实例。建议对不同的分支使用不同的实例： 

``` bash
ciel add stable # Since we are going to build on stable
```

确保我们在 `stable` 分支上。 

``` bash
cd TREE
git checkout stable
cd ..
```

然后，我们需要配置 Ciel 以使用正确的软件仓库。为了避免产生错误的依赖关系，打包环境应该使用与分支匹配的包（`stable-proposed` 只使用来自 `stable` 的依赖项，是一个例外）。比方说，我们需要 `stable` 软件仓库来构建 `stable` 软件包，需要`testing`、`stable-proposed` 和 `stable` 来构建 `testing` 软件包。 

``` bash
ciel config -i stable
```
首先输入你的信息，选择是否启用 DNSSEC，然后 Ciel 会询问你是否编辑 `source.list`，选择是并编辑。

``` INI
# For building stable packages
deb https://repo.aosc.io/debs stable main

# For building testing packages
deb https://repo.aosc.io/debs testing main
deb https://repo.aosc.io/debs stable-proposed main
deb https://repo.aosc.io/debs stable main

# And you get the idea.
```

现在我们可以正式开始构建我们的软件包啦！只需要执行：

``` bash
ciel build -i stable flac
# -i is used to select the instance used to build
```

如果没有报错出现，并能见到 `Build Summary`，那么祝贺你成功构建了你的第一个软件包！现在你应该能在 `OUTPUT/debs` 看到构建好的 DEB 包。

# 添加一个新的软件包

已经掌握了如何构建一个已有的软件包？接下来我们再进一步，尝试从零开始构建一个软件包。

进入 `TREE` 文件夹，你会看到很多文件夹，包括一些以 `base-` 和 `core-` 开头的文件夹，还有一些以 `extra-` 开头的文件夹，我们使用这些文件夹给软件包分类。在文件夹里面，你会发现各种包及其 PKGBUILD 文件。 

例如说 `i3`，这个包很显然能在 `TREE/extra-wm/i3` 被找到。进入这个目录之后，应该能见到以下的目录树：

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

我们会进一步探索这些目录。

## `spec`

这个文件会告诉 `acbs` 在什么地方下载源码文件，并声明软件包的版本号和发布号。一个 `spec` 文件看起来应该是这样子的：

``` bash
VER=4.17.1  # Version of the software.
# REL=0 The package revision. If not specified, it's 0.
SRCTBL="https://i3wm.org/downloads/i3-$VER.tar.bz2" # Download address for the source code.
CHKSUM="sha256::1e8fe133a195c29a8e2aa3b1c56e5bc77e7f5534f2dd92e09faabe2ca2d85f45" # Checksum of the source tarball.
```

有一点值得注意的是发布号。如果你在创建一个新的包，你可以忽略这一行，但在某些情形下（例如应用一个安全补丁），版本号不会更改，但我们仍然需要通知用户电脑上的包管理器有可用的更新。在这种情况下，只需将 `$REL` 变量增加 1。

## `autobuild/`

这是所有 `Autobuild3` 脚本和声明文件所在的目录。`Autobuild3` 是一个复杂的构建系统，它可以自动规划构建的流程，比如使用哪个构建系统，使用哪个构建参数等等。 

## `autobuild/defines`

这个文件包含了一些核心的配置项，例如：

  - `PKGNAME` : 软件的名称。
  - `PKGDES` : 软件的描述。
  - `PKGSEC` : 软件的类别。
  - `PKGDEP` : 软件的生成和运行时必须先行安装的软件列表。
  - `PKGCONFL` : 与软件有冲突关系的软件列表。
  - `BUILDDEP` : 仅在软件生成时需要的软件包列表。
  - `PKGRECOM` : 软件的生成和运行时推荐先行安装的软件列表。

这些只是最常见的配置项。有更多的配置项，但对于大部分软件这些配置就足够了。`Autobuild3` 可以自动填充其他信息，比如使用哪个 C 编译器标志，使用哪个构建系统。

这是 `TREE/extra-multimedia/i3` 的配置：

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

实际上，你可以在 `defines` 中写 Bash 逻辑。这在为特定平台添加标志或依赖项时很有用，但我们**不建议**你这样做，将来也可能直接禁止这样做。要为特定平台添加信息，请使用 `$VAR__$ARCH`。 

要查看完整的可用配置项，请查看 [Autobuild3 维基](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3)。

## `autobuild/prepare`

此文件是将在构建过程开始之前执行的脚本。通常用于文件准备或设置构建过程中需要到的环境变量。

## `autobuild/patches/`

这个目录用于放置所有需要在构建开始前添加到源码的补丁。只需要将补丁丢进这个目录就行了。:)

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