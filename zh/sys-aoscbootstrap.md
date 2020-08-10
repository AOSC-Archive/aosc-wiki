---
title: AOSCBootstrap
description: 
published: true
date: 2020-08-10T11:33:43.423Z
tags: 
editor: markdown
---

# AOSCBootstrap

## 依赖

AOSCBootstrap 依赖下面这些 Perl 模块：

- [LWP](https://metacpan.org/pod/LWP)
- [Try::Tiny](https://metacpan.org/pod/Try::Tiny)

在 AOSC OS，你可以使用下面的命令安装它们：

```bash
apt install perl-try-tiny libwww-perl
```

## 用法

```
aoscbootstrap.pl --arch=<architecture> --include=[additional package] --include-file=[list of packages] <branch> <path/to/target> [mirror URL]
```

`[mirror URL]` 参数是可选的，不提供的情况下默认设定为 `https://repo.aosc.io/debs`。

`--include=` 和 `--include-file=` 参数同样也是可选的，可以多次指定，也可以同时指定。

例如，如果你要使用 `localhost` 作为镜像，在 `/root/aosc` 引导使用 `stable` 分支的 `amd64` 系统：

```
aoscbootstrap.pl --arch=amd64 stable /root/aosc http://localhost/debs/
```

如果你想添加更多的软件包，例如 `network-base` 和 `systemd-base`：

```
aoscbootstrap.pl --arch=amd64 --include=network-base --include=systemd-base stable /root/aosc http://localhost/debs/
```

你还可以将你希望添加的软件包写到一个文本文件中：

```
network-base
systemd-base
editor-base
[...]
```
假设你将文件保存为 `base.lst`，你可以利用下面的方式将软件包列表传递给 AOSCBootstrap：

```
aoscbootstrap.pl --arch=amd64 --include-file=base.lst stable /root/aosc http://localhost/debs/
```

## 与 Ciel 配合使用

要配合 [Ceil](https://github.com/AOSC-Dev/ciel) 及其插件使用 AOSCBootstrap：

1. Create your work directory and `cd` into it.
1. Run `ciel init`.
1. Run `aoscbootstrap.pl --arch=<architecture> <branch> $(pwd)/.ciel/container/dist/ [mirror URL]`.
1. When finished, you may proceed to other tasks you may want to perform such as `ciel generate` and `ciel release`.

## Full Bootstrap to a Larger Base System

You can use AOSCBootstrap to bootstrap a larger base system or even a base system with a graphic user interface based desktop environment like GNOME or KDE.

To do this, you need a list of required packages. Fortunately, you can find the recipe [inside CIEL!'s source tree](https://github.com/AOSC-Dev/ciel/raw/master/plugin/ciel-generate).

To help you convert this bash script to a plaintext file containing the required packages, there is a conversion script resides in the `recipes` folder.

Simply do `perl recipes/convert.pl ./ciel-generate ./recipes` and all the base variants currently supported by `ciel generate` command will be dumped into the `recipes` folder.

Now, to use those recipes, for instance, bootstrapping a `kde` variant of the system, you can execute AOSCBootstrap like this:

```
aoscbootstrap.pl --arch=amd64 --include-file=./recipes/kde.lst stable /root/aosc http://localhost/debs/
```

AOSCBootstrap will prepare a ready-to-use KDE flavored AOSC OS located at `/root/aosc`.
