---
title: Installation/AMD64/SysReq
description: AOSC OS 系统要求（适用于 AMD64/x86-64 设备）
published: true
date: 2020-08-16T13:00:16.769Z
tags: sys-installation
editor: markdown
---

这个页面将为你提供 AOSC OS 各个 x86_64 变种版本对处理器、内存、显示设备、GPU 和储存空间的具体要求，旨在帮助您选择合适的变种版本。因为 Container 版本和 BuildKit 版本并非可引导可安装变种，在这里不做讨论。

# 变种版本

AOSC OS 提供了以下可安装可引导的变种版本：

- [Base](#base)
- [KDE/Plasma Desktop](#kde-plasma-desktop)
- [GNOME](#gnome)
- [MATE](#mate)
- [XFCE](#xfce)
- [LXDE](#lxde)
- [i3 Window Manager](#i3-window-manager)


每个变种版本都有各自的特性和系统要求，但是它们对处理器的最低要求都是一样的：

- 必须是兼容 EMT64/AMD64 的 x86-64 处理器。
- 必须要有 SSE3 支持。

## Base

Base 变种版本提供了一个不带有桌面环境的最小化系统，它为用户提供了用于系统管理、文本编辑、网络连接的基本工具。只需稍作配置，即可将此变种版本用于小型服务器。

- Processor: Any processor supported.
- Memory: 128MB, 512MB recommended for heavier workload (Node.js).
- Storage: 2GB, 4GB recommended.
- Display: Not necessary (SSH, Serial).
- GPU: Not necessary.

## KDE/Plasma Desktop

KDE 由一个桌面环境（Plasma Desktop）和一个庞大的应用程序集组成。Plasma Desktop 是重量级桌面环境，对处理器、内存和 GPU 要求苛刻，因此不推荐在旧设备上使用这个变种版本，特别是没有独显的老式笔记本。

- Processor: Any processor supported, Intel Sandy Bridge recommended.
- Memory: 2GB, 4GB or more recommended.
- Storage: 8GB, 16GB recommended.
- Display: XGA, 1080p or higher recommended.
- GPU: Required, with OpenGL 2.1+ support.

## GNOME

GNOME 是一个基于 GNOME Shell 界面的功能齐全的桌面环境。GNOME 也是重量级桌面环境，对处理器、内存和 GPU 也有着高要求，最近八年出厂的计算机应该能较好地运行 GNOME，更快的 GPU 可能有助于提高桌面性能。

- Processor: Any processor supported, Intel Sandy Bridge recommended.
- Memory: 2GB, 4GB or more recommended.
- Storage: 8GB, 16GB recommended.
- Display: XGA, 1080p or higher recommended.
- GPU: Required, with OpenGL 2.1+ support.

## MATE

MATE 是 GNOME 2 的一个分支版本，对显卡的要求较低，但这也不是一个轻量级桌面环境。任何基于 x86_64 的系统都应该可以运行 MATE。MATE 是基于 GTK 3 构建的，一个支持 2D 加速的 GPU 可以显著提高桌面性能。

- Processor: Any processor supported, Core 2 Duo recommended.
- Memory: 512MB, 1GB or more recommended for multitasking.
- Storage: 8GB, 16GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## XFCE

XFCE 是一个相对轻量级、模块化的桌面环境，基于 GTK 3。XFCE 的内存占用更小，占用的存储空间更少。

- Processor: Any processor supported.
- Memory: 256MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## LXDE

LXDE 比 XFCE 更加轻量，也是完全模块化的，基于 GTK 3。推荐在旧式设备上使用这一变种（如配备 Pentium 4 Prescott 的设备）。

- Processor: Any processor supported.
- Memory: 256MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## i3 Window Manager

这是带有图形界面的最轻量的变种版本，但你可能需要花费一定的时间熟悉 i3 窗口管理器的使用。

- Processor: Any processor supported.
- Memory: 128MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

# 备注

- 部分网络浏览器（例如 Chromium 和 Firefox）可能需要更好的处理器和内存。如果你使用 Intel Pentium 4/D 或 AMD Athlon64 这样的老处理器或者不足 2GB 的内存，很难流畅地使用这些浏览器浏览网页。这个页面的英文版初稿是在联想 ThinkPad T61（Intel Core 2 T9300 处理器，8GB RAM）上完成的，使用的是 Chromium 62；中文版初稿是在戴尔 Inspiron 7591（Intel Core i5 9300H 处理器，8GB RAM）上完成的，使用的是 Firefox 78。
- 如果您想使用基于 DKMS 的 Linux 内核插件，由于你的系统需要编译这些插件模块，同样也需要更好的处理器和内存。
- 在转速低于 5400 RPM 机械硬盘上安装系统更新将需要消耗较多的时间。