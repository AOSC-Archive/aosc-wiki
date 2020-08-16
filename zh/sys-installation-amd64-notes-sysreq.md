---
title: Installation/AMD64/SysReq
description: AOSC OS System Requirements for AMD64/x86-64 Devices
published: true
date: 2020-08-16T11:32:05.224Z
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

- 所有兼容 EMT64/AMD64 的 x86-64 处理器。
- 必须要有 SSE3 支持。

## Base

This variant of AOSC OS is intended for non-graphical and minimal installation, users are provided with basic tools for system management, text editing, network connection, and Internet connectivity support. This variant is suitable for server configuration with additional software packages installed.

- Processor: Any processor supported.
- Memory: 128MB, 512MB recommended for heavier workload (Node.js).
- Storage: 2GB, 4GB recommended.
- Display: Not necessary (SSH, Serial).
- GPU: Not necessary.

## KDE/Plasma Desktop

KDE, or Plasma Desktop is a complete desktop interface and application suite. This is the heaviest variant of AOSC OS, highly taxing on processor, memory, and GPU. Not recommended for older laptops and desktops with integrated graphics (Intel i965 will do, but anything older will struggle).

- Processor: Any processor supported, Intel Sandy Bridge recommended.
- Memory: 2GB, 4GB or more recommended.
- Storage: 8GB, 16GB recommended.
- Display: XGA, 1080p or higher recommended.
- GPU: Required, with OpenGL 2.1+ support.

## GNOME

GNOME, with its GNOME Shell interface, is a fully featured desktop environment with relatively high demand on processor, memory, and GPU. Many x86-64 laptop, desktop of ~8 years of age should run GNOME just fine. A faster GPU may help with desktop performance.

- Processor: Any processor supported, Intel Sandy Bridge recommended.
- Memory: 2GB, 4GB or more recommended.
- Storage: 8GB, 16GB recommended.
- Display: XGA, 1080p or higher recommended.
- GPU: Required, with OpenGL 2.1+ support.

## MATE

A fork of GNOME 2, therefore less taxing on graphic card (GPU), this is *not* a lightweight desktop environment, however. Any x86-64-based system should be able to run MATE just fine. MATE Desktop is built against GTK+ 3, a working GPU with 2D acceleration may boost desktop performance dramatically.

- Processor: Any processor supported, Core 2 Duo recommended.
- Memory: 512MB, 1GB or more recommended for multitasking.
- Storage: 8GB, 16GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## XFCE

XFCE is a relatively lightweight, and fully modular desktop environment. XFCE has a smaller memory footprint, and takes less storage space. XFCE is based on GTK+ 2, therefore not exactly taxing on GPU (though it is expected that XFCE will switch to GTK+ 3, then system requirements will be expected to be on-par with that of [MATE Desktop](#mate)).

- Processor: Any processor supported.
- Memory: 256MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## LXDE

LXDE is lighter (yet) than XFCE, also fully modular, and based on GTK+ 3. Recommended for older x86-64-based systems, say, those equipped with a Pentium 4 Prescott (EMT64 supported).

- Processor: Any processor supported.
- Memory: 256MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## i3 Window Manager

i3 Window Manager variant of AOSC OS comes with Conky and i3block for system information monitor, and Compton for desktop composition support (disabled on framebuffer devices). This is the lightest variant that comes with a graphical interface, but also comes with a steeper learning curve.

- Processor: Any processor supported.
- Memory: 128MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

# Extra Notes

- Web Browsers, especially mainstream choices like Chromium (Google Chrome), and Firefox may require significantly more memeory capacity and processing power. It will be very difficult to browse any webpage smoothly with an older processor like Intel Pentium 4/D's or AMD Athlon64's with less than 2GB (arguably 4GB) of RAM. As of this note, this Wiki page is edited on a Lenovo ThinkPad T61, with an Intel Core 2 T9300 processor and 8GB of RAM, running Chromium 62.
- If you would like to use DKMS-based Linux Kernel addons, it is required for your system to compile these addon modules, which could require a significant amount of processing power - but not necessarily RAM.
- Installing system updates on a 5400 RPM or slower HDD (mechanical hard disk drives) will require significantly greater amount of time, even on older systems.