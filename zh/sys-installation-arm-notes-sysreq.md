---
title: Installation/ARM*/SysReq (简体中文)
description: 
published: true
date: 2020-08-19T15:29:49.814Z
tags: sys-installation
editor: markdown
---

这个页面将为你提供 AOSC OS 各个 ARMv7 变种版本和 AArch64 变种版本对处理器、内存、显示设备、GPU 和储存空间的具体要求，旨在帮助您选择合适的变种版本。请注意我们只会提供通用的指标，并不会针对某个具体的设备展开讨论。除此之外，因为 Container 版本和 BuildKit 版本并非可引导可安装变种，在这里不会做讨论。

# 变种版本

AOSC OS 为 ARMv7 架构和 AArch64 架构提供了以下可安装可引导的变种版本：

- [Base](#base)
- [GNOME](#gnome)
- [MATE](#mate)
- [XFCE](#xfce)
- [LXDE](#lxde)
- [i3 Window Manager](#i3-window-manager)

每个变种版本都有各自的特性和系统要求，但是它们对处理器的最低要求都是一样的：

- ARMv7: Any processor with ARMv7-A+, and NEON FPU support.
- AArch64: Any AArch64, 64-bit ARMv8 processor.


## Base

This variant of AOSC OS is intended for non-graphical and minimal installation, users are provided with basic tools for system management, text editing, network connection, and Internet connectivity support. This variant is suitable for server configuration with additional software packages installed.

- Processor: Any processor supported.
- Memory: 128MB, 512MB recommended for heavier workload (Node.js).
- Storage: 2GB, 4GB recommended.
- Display: Not necessary (SSH, Serial).
- GPU: Not necessary.

## GNOME

GNOME, with its GNOME Shell interface, is a fully featured desktop environment with relatively high demand on processor, memory, and GPU. Not many ARM devices are capable of running GNOME smoothly.

- Processor: Any processor supported, quad core recommended.
- Memory: 512MB, 2GB or more recommended for multitasking.
- Storage: 8GB, 16GB recommended.
- Display: XGA, 1080p or higher recommended.
- GPU: Recommended, with OpenGL 2.1+ or GLES2 support.

## MATE

A fork of GNOME 2, therefore less taxing on graphic card (GPU), this is *not* a lightweight desktop environment, however. ARMv7 boards like the Raspberry Pi 2 can run MATE Desktop just fine. MATE Desktop is built against GTK+ 3, a working GPU with 2D acceleration may boost desktop performance dramatically.

- Processor: Any processor supported, dual core recommended.
- Memory: 512MB, 1GB or more recommended for multitasking.
- Storage: 8GB, 16GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## XFCE

XFCE is a relatively lightweight, and fully modular desktop environment. XFCE has a smaller memory footprint, and takes less storage space. XFCE is based on GTK+ 2, therefore not exactly taxing on GPU (though it is expected that XFCE will switch to GTK+ 3, then system requirements will be expected to be on-par with that of [MATE Desktop](#mate)).

- Processor: Any processor supported, dual core recommended.
- Memory: 256MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## LXDE

LXDE is lighter (yet) than XFCE, also fully modular, and based on GTK+ 3, performance shouldn't be an issue on most ARM devices/systems, recommended for energy saving and higher-workload configurations.

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
