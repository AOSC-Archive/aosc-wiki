---
title: Installation/ARM*/SysReq (简体中文)
description: 
published: true
date: 2020-08-19T15:37:19.752Z
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

- ARMv7：必须是支持 NEON FPU 的 ARMv7-A+ 处理器。
- AArch64：必须是 64 位 ARMv8 处理器。

## Base

Base 变种版本提供了一个不带有桌面环境的最小化系统，它为用户提供了用于系统管理、文本编辑、网络连接的基本工具。只需稍作配置，即可将此变种版本用于小型服务器。

- Processor: Any processor supported.
- Memory: 128MB, 512MB recommended for heavier workload (Node.js).
- Storage: 2GB, 4GB recommended.
- Display: Not necessary (SSH, Serial).
- GPU: Not necessary.

## GNOME

GNOME 是一个基于 GNOME Shell 界面的功能齐全的桌面环境。GNOME 是重量级桌面环境，对处理器、内存和 GPU 有着高要求。能流畅运行 GNOME 桌面的 ARM 设备不多。

- Processor: Any processor supported, quad core recommended.
- Memory: 512MB, 2GB or more recommended for multitasking.
- Storage: 8GB, 16GB recommended.
- Display: XGA, 1080p or higher recommended.
- GPU: Recommended, with OpenGL 2.1+ or GLES2 support.

## MATE


MATE 是 GNOME 2 的一个分支版本，对显卡的要求较低。这并不是一个轻量级桌面环境，但是 ARMv7 开发板（如 Raspberry Pi 2）基本可以流畅运行 MATE 桌面。MATE 是基于 GTK 3 构建的，一个支持 2D 加速的 GPU 可以显著提高桌面性能。 

- Processor: Any processor supported, dual core recommended.
- Memory: 512MB, 1GB or more recommended for multitasking.
- Storage: 8GB, 16GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## XFCE

XFCE 是一个相对轻量级、模块化的桌面环境，基于 GTK 3。XFCE 的内存占用更小，占用的存储空间更少。

- Processor: Any processor supported, dual core recommended.
- Memory: 256MB, 512MB or more recommended for multitasking.
- Storage: 6GB, 12GB recommended.
- Display: SVGA, XGA or higher recommended.
- GPU: Recommended, but framebuffer device support will do.

## LXDE

LXDE 比 XFCE 更加轻量，也是完全模块化的，基于 GTK 3。在 ARM 设备和系统上运行 LXDE 基本无压力。如果有节电或高负载的需要，推荐使用这一变种。

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
