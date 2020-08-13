---
title: AOSC OS/Retro：面向用户与维护者的介绍（征求意见稿）
description: 在古董设备上使用 AOSC OS
published: true
date: 2020-08-13T06:08:50.661Z
tags: sys-retro
editor: markdown
---

这个页面的主要用途是介绍 AOSC/Retro 的设计规范和维护目标。想要了解这个分支的基本理念，请阅读 [这个页面](/en/sys-retro-rationale)。

# 设计规范

无论是从用户体验的角度，还是从维护的角度出发，AOSC OS/Retro 都是标准的 AOSC OS 发行版。但是依赖关系、特性和维护计划会和主线版本有所不同。简而言之，AOSC OS/Retro 将：


- 只为有限的古董架构提供支持。
- 和主线版本共享同一 [软件包树](https://github.com/AOSC-Dev/AOSC-os-abbs/) 和 [系统核心组件](https://github.com/AOSC-Dev/AOSC-os-abbs/blob/stable/README.CORE.md)。
- 和主线版本共享同一维护工具集。
- 为了节省存储空间和内存，对软件包特性进行精简。
- 和主线版本相比，提供不同的变种版本。
- 在多数情况下，更新速度会慢于主线版本。

下面，我们将展开介绍 AOSC/Retro 和主线版本的共同点和不同点。

## 目标架构与设备

AOSC OS/Retro 目前为下面的架构与设备提供支持：

- 32 位的 Intel 80486 和与其兼容的（IBM）Personal Computers（不需要浮点运算单元）以及 Personal System/2（PS/2）。
- 32 位的基于 Big Endian PowerPC 的 Apple Macintosh 电脑（需要有 [New World ROM](https://en.wikipedia.org/wiki/New_World_ROM) 支持）。

## 维护工具集

AOSC OS/Retro 是 AOSC OS 的一个分支，而不是 AOSC OS 的派生版本，它将和主线版本共享软件包树、系统核心组件以及维护工具集：

- 软件包树：[aosc-os-abbs](https://github.com/AOSC-Dev/aosc-os-abbs) 的 [`retro`](https://github.com/AOSC-Dev/aosc-os-abbs/tree/retro) 分支。
    - 这意味着 AOSC/Retro 会使用 systemd 作为初始化系统。事实上我们曾在一台搭载 Intel Pentium 75MHz 处理器，有着 16MiB RAM 和 810MiB HDD 的 Toshiba T4900CT 上做过测试，systemd 确实能良好地工作。
- AOSC OS Core 是被共享的，但只有适用于 AOSC OS/Retro 的更新会被同步到 `retro` 分支。详见 [维护计划](#维护计划).
- 打包工具和维护工具：
    - [Autobuild3](https://github.com/AOSC-Dev/autobuild3)，用于运行构建脚本。
    - [ACBS](https://github.com/AOSC-Dev/acbs)，用于软件包树管理和打包。
    - [Ciel](https://github.com/AOSC-Dev/ciel)，用于容器管理。
    - [一些实用脚本](https://github.com/AOSC-Dev/scriptlets)。

## 依赖项

考虑到 AOSC OS/Retro 通常被安装在储存空间较少且性能较差的设备上，所以和主线版本不同，我们只会为 AOSC OS/Retro 提供启用了最小功能集的软件包：

- 最小化系统需要的所有软件包（包括 `admin-base`、`boot-base`、`core-base`、`editor-base`、`kernel-base`、`network-base`、`systemd-base`、`util-base` 和 `web-base`）不得通过依赖关系引入 Python（`python-2`、`python-3`）和 Perl（`perl`）。
- 在非必要情况下，语言绑定（Java、Perl、Python 等等）默认不启用。 
- Glibc 仅启用 `C` 和 `C.UTF-8` 区域支持，其他区域支持需要用户在 `/etc/locale.gen` 反注释相关行以启用。
- 移除所有可选依赖项，除非该软件包是 [核心软件包](https://github.com/AOSC-Dev/AOSC-os-abbs/blob/groups/build-core) 或经维护人员讨论决定不作移除。
- 所有软件包均应在启用链路时间优化的情况下构建，除非此类优化会导致构建失败。
- 非性能关键型应用将使用 `-Os`优化级别来构建（即在 `autobuild/defines` 中使用 `AB_FLAGS_OS=1`），以节省储存空间。 
- 我们会提供 Manpage 和 Texinfo 文档，但不会提供所有其它形式的文档（例如 HTML、gtk-doc 等等）。
- 在没有 Dracut 的情况下也应当可以正常引导 Linux 内核（除非启用了 RAID），系统不预装 Dracut。 

## 发行版特性

AOSC OS/Retro 将提供两个版本，Base 和 Base/X11。

- Base 版本包含一个最小的可引导的命令行系统，包含系统管理、文本编辑、网络连接、电源管理、文档查看等所必需的工具。
- Base/X11 版本包含一个最小的可引导的图形化系统，除了 Base 版本提供的软件外，还提供一个基于 X11 的桌面环境和一些实用的图形化工具。
- 两个版本均提供本地化支持（只需启用了相应的 Locale）以及通用、原生的 Linux 内核，并使用 NetworkManager 作为网络管理工具。

The Base/X11 variant will come with the following additional components (the list below is subject-to-change)...

- Desktop Environment: IceWM with Panel.
    - IceWM is chosen for its familiar (Windows-like) interface, (partial) FD.o compliance, as well as CJK (Chinese, Japanese, and Korean) support.
- Fonts: Standard X11 fonts (bitmap) and Unifont (bitmap and vectorised).
    - Unifont supplied to provide complete Unicode text displaying support.
- Audio and Video: MPV and Cmus; FFmpeg; PulseAudio.
    - MPV is chosen for its lightweight SDL2-based interface, as well as FFmpeg support.
    - Cmus is chosen for its curses-based terminal UI, minimalising graphical hardware requirement.
    - PulseAudio will come standard for proper multi-device and multi-application audio support.
- Image Viewer: Feh.
    - Feh is chosen for its minimal interface based on standard X widgets.
- Web Browser: Dillo, w3m, and Lynx.
    - Dillo is chosen for its lightweight FLTK interface, as well as partial HTML5 compliance.
    - w3m and Lynx are shipped as standard under the `web-base` metapackage.

Extra packages, such as Firefox and more feature-complete desktop environments will be available from the [community repository](https://packages.aosc.io/), however, hardware requirement checks will be enforced based on processor and memory installed on your AOSC OS/Retro device (i.e., package installation will be aborted when attempting to install Firefox on a computer without SSE2 SIMD support).

## 维护计划

<!-- Note from bobby285271: If you want to edit the heading of this chapter, please update the links in the "维护工具集" chapter accordingly. -->

AOSC OS/Retro will be maintained on the [`retro`](https://github.com/AOSC-Dev/aosc-os-abbs/tree/retro/) branch, sharing the same [package tree](https://github.com/AOSC-Dev/aosc-os-abbs/) with the mainline distribution. However, in interest of both the maintainer's reasonable maintenance effort, as well as the longevity and usability of the target devices, AOSC OS/Retro will update on an *annual schedule*.

After the first update cycle of a year, the `retro` branch will *merge from* the `stable` branch from the mainline distribution (`stable` => `retro`). After which, *no further merge or reverse merge* will be allowed. Package versions in the `retro` branch will remain constant unless...

- An [patch-level update](/dev-sys-known-patch-release-rules) is made available.
- A security update is made available that *requires* a version update. If necessary, changes could be [cherry-picked](https://git-scm.com/docs/git-cherry-pick) from the `stable` branch.

At the end of each annual cycle, a new distribution tarball will be made available on the [downloads page](https://aosc.io/downloads/), as well as an update CD image containing a local repository containing all system updates. A full AOSC OS/Retro repository will also be provided in forms of a tarball or a set of CD/DVD image.

# 维护目标

AOSC OS/Retro will be maintained with a few goals in mind, relating to system performance, storage requirements, and peripheral support. This chapter will also serve to outline AOSC OS/Retro's system requirements.

This chapter will then be split into sections, containing requirements and metrics shared and specific to each of our target architectures.

## Common Metrics

- AOSC OS/Retro's "Base" flavour should install onto a 540MB hard disk drive (largest capacity available for non-LBA systems, such as earlier 486-class systems), while the "Base/X11" flavour should install onto a 1.2GB drive (commonly found on Intel x86 computers from ~1996).
    - After the system is installed, there should be enough space for a 64MiB swap area and future system updates (assuming one package is cached onto the hard disk at a time, using the update CD).
    - Users should expect to conserve ~100MiB of hard disk space for network- or internet-based system updates.
- AOSC OS/Retro should not require any form of network access for normal usage, assuming the user has obtained a copy of local repository.
- AOSC OS/Retro should support common ISA/EISA (or PCMCIA), PCI (or CardBus), PCI Express (or ExpressCard), SCSI, as well as USB (1.1/2.0), PS/2, Serial and Parallel peripherals.
- AOSC OS/Retro should support dial-up, 10/100/1000Mbps Ethernet, as well as 802.11a/b/g/n/ac wireless connections.
- AOSC OS/Retro should boot from IDE/EIDE/CE-ATA/SATA/SCSI-based hard disk drives, SCSI configuration should be supported but will require extra components. AOSC OS/Retro may boot from USB, optical media, or other forms of external/removable storage, but this will not officially supported.

## System Performance (x86)

On the 32-bit x86 architecture, AOSC OS/Retro "Base" requires the following
system components...

- Processor: Intel 80486 or compatible, FPU (Floating Point Unit) not required.
- System Bus: ISA, EISA, PCI, or PCI Express based system devices. MCA (Micro Channel Architecture) not supported.
- RAM: 16MiB (32MiB swap).
- Storage: 540MB (~514MiB).
    - (Ultra) DMA via PCI Bus Mastering will significantly improve system performance.
- Input Device: PS/2 or Serial Port Keyboard. Mouse not required.
- Display: VGA or compatible, or serial terminal.

AOSC OS/Retro "Base/X11" requires the following system components...

- Processor: Intel 80486 or compatible, FPU (Floating Point Unit) not required.
    - Intel Pentium II 233MHz, AMD K6, Cyrix MediaGX, Via C7 or above will significantly improve graphical experience.
    - Intel Pentium III 500MHz, AMD K6-II/III or above recommended for video playback using MPV.
- System Bus: ISA, EISA, PCI, or PCI Express based system devices. MCA (Micro Channel Architecture) not supported.
- RAM: 32MiB (32MiB swap).
    - 128MiB or above recommended for Internet browsing.
- Storage: 1.2GB (~1141MiB).
    - 4.0GB (~3814MiB) recommended for local multimedia storage.
    - (Ultra) DMA via PCI Bus Mastering will significantly improve system performance.
- Input Device: PS/2 or Serial Port Keyboard and Mouse.
    - Touchscreen will be supported via I2C or Serial Port.
- Display: VGA or compatible.
    - ISA/EISA video cards *not recommended*, VESA Local Bus will significantly improve video performance.
    - PCI and PCI Express video cards recommended, especially those with OpenGL 2.1 support (often found after ~2002), as this will allow for GPU-based video playback acceleration.

## System Performance (PowerPC 32-bit, Big Endian)

AOSC OS/Retro "Base" or "Base/X11" should run on all supported devices on this architecture - that is, PowerPC-based Apple Macintosh computers with New World ROM support.

- Portables...
   - PowerBook G3 "Lombard" and "Pismo" models.
   - All iBook G3, iBook G4, PowerBook G4 models.
- Desktops...
   - Power Macintosh G3 "Blue and White" models.
   - All Power Macintosh G4 and G5 models.
   - All iMac G3 and G4 models.
   - All eMac models.
   - All G4-based Mac Mini models.
   - All G4- and G5-based Xserve models.
