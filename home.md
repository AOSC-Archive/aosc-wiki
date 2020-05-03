---
title: AOSC Wiki
description: Where people come to know... Oh wait, sorry MSFN :D
published: true
date: 2020-05-03T08:40:47.182Z
tags: 
---

# Welcome

Welcome to the AOSC Community Wiki!

This is where all documentations, information, and guides will gather for users and developers alike. This community wiki is currently a work-in-progress, so do expect constant content changes and movements. Anyone is welcome to make changes to this wiki as a guest, as user management backend is not yet complete on Wiki.js upstream.

The wiki is split into multiple big sections, targeting different audiences. Do use the search bar if lost though, it could come in handy.

Be sure to note our [AOSC Inter-Personal Guidelines](community-guidelines).

# Users

This section contains information useful for AOSC OS users with pages arranged by topics.

- [Is AOSC OS Right for Me?](is-aosc-os-right-for-me)

## System Installation

This topic provides information on installation of AOSC OS, and considerations suggested when choosing different editions of AOSC OS.

- How-to/Guides
	- [AMD64/x86-64 Systems](installation-amd64)
	- [ARMv7/AArch64 Devices](installation-arm)
	- [PowerPC/PPC64-based Macintosh](installation-powermac)
	- *MIPS 32/64-Bit Systems*
	- [ZFS Root](/users/installation/zfs-root)
- System Requirements
	- [AMD64/x86-64 Systems](/users/installation/amd64-notes-sysreq)
	- [ARMv7/AArch64 Devices](/users/installation/arm-notes-sysreq)
	- [PowerPC/PPC64-based Macintosh](/users/installation/powermac-notes-sysreq)
	- *MIPS 32/64-bit Systems*

## System Information and Specifications

This topic provides information on AOSC OS's system specifications, naming schemes, etc.

- [Architecture Namings and Specifications](/users/information/arch-specs)
- [Filesystem Hierarchy](/users/information/fs-hierarchy)
- *Optimisation Overlays*

## AOSC OS/Retro

This sub-section provides guidebooks and developer information for AOSC OS/Retro maintainers.

- [AOSC OS/Retro: A Rationale (RFC)](/developers/retro/aosc-os-retro-rationale)
- [AOSC OS/Retro: An Introduction to Users and Maintainers (RFC)](/developers/retro/aosc-os-retro-intro)

# Developers

This section contains guidebooks for AOSC developers, as well as more technically-inclined content for developers using AOSC OS.

## Wiki and Contribution

This sub-section provides basic information for contributions to this Wiki.

- [How to Contribute to This Wiki](developers/how-to-contribute-md)

## Public Developer Resources

This sub-section contains general information about public servers and resources made available to AOSC developers.

- [AOSC Buildbot Information](developers/buildbots)

## Infrastructure

This sub-section contains information on APIs and technical details from our community infrastructure.

- [Community Portal](developers/community-portal)
- [Packages Site](developers/packages-site)
- [List of Package Issue Codes](/developers/list-of-package-issue-codes)
- [p-vector](/developers/p-vector)

## AOSC OS

This sub-section provides guidebooks and developer information for AOSC OS maintainers.

- [Exceptions to the Update Cycles](/developers/aosc-os/cycle-exceptions)
- [Known Patch Release Rules](/developers/aosc-os/known-patch-release-rules)
- Resources for Packagers
	- [Intro To Package Maintenance](/developers/intro-to-package-maintenance)
	- [AOSC OS Maintenance Guildelines (RFC)](/developers/aosc-os-maintenance-guidelines)
	- [AOSC OS Package Styling Manual](/developers/aosc-os-package-styling-manual)
  
### Automation

Developers intersted in AOSC OS maintenance/packaging automation should refer to this meta page.

- [AOSC OS Maintenance Automation: Design RFCs and Progression](/developers/automation/aosc-os-maintenance-automation-design-rfcs-and-progression)

## AOSC OS/Retro

This sub-section provides guidebooks and developer information for AOSC OS/Retro maintainers.

- [AOSC OS/Retro: A Rationale (RFC)](/developers/retro/aosc-os-retro-rationale)
- [AOSC OS/Retro: An Introduction to Users and Maintainers (RFC)](/developers/retro/aosc-os-retro-intro)

## Conferences

- [AOSCC 2019](/aoscc/2019/info)

# Knowledge Base

This section contains pages describing common questions, tips, and directions when using AOSC OS.

- [KB-SYS-00001: Configuration of APT Repositories](/kbs/sys/00001-apt-gen-list)
- [KB-SYS-00002: Configuration of Input Methods](/kbs/sys/00002-imchooser)
- [KB-SYS-00003: Configuration of JACK](/kbs/sys/00003-jack-configuration)

# Errata

This section contains known unresolved and resolved issues found in AOSC OS - for developers' and users' reference.

- [ERR-SYS-00001: Disappearing Windows with KDE/Plasma Desktop using Intel DDX](/err/x11/00001-kde-window-intel-ddx)
- [ERR-SYS-00002: Noto Mono Fonts Displayed as Sans Serif with Recent Font Package Update](/err/x11/00002-noto-mono-font-name-change)
- [ERR-SYS-00003: Errors when Running the "sensors" Command on a HP MicroServer Gen8](/err/x11/00003-sensors-dmesg-error-microserver-gen8)
- [ERR-SYS-00004: NetEase Cloud Music < 1.1.0 Fails to Launch](/err/x11/00004-netease-cloud-music-sandbox-error)
- [ERR-SYS-00005: X11 Graphical Interface Fails to Start on Systems with Dedicated NVIDIA Graphics](/err/x11/00005-nvidia-x-failure-without-nouveau-blacklist)
- [ERR-SYS-00006: VLC 3.0 and Above May Fail to Start in GNOME](/err/x11/00006-vlc-fails-to-launch-in-gnome)
- [ERR-SYS-00007: Performance Issues with Fontconfig <= 2.12.92](/err/x11/00007-fc-cache-performance-issues)
- [ERR-SYS-00008: CUPS "Filter Failed" with Printers Using the HPLIP Driver](/err/x11/00008-hplip-proprietary-plugins-version-mismatch)
- [ERR-SYS-00009: Error Updating to Systemd 242](/err/systemd/00009-error-updating-to-systemd-242)

# AOSC OS Security Advisories

A brief introduction to the definition, format, and workflow of AOSC OS Security Advisories can be read in the article below.

- [What is an AOSA?](/aosa/what-is-an-aosa)

This section lists all current and historic AOSC OS Security Advisories.

- [List of Announced AOSAs (2017)](/aosa/archive/2017)
- [List of Announced AOSAs (2018)](/aosa/archive/2018)
- [List of Announced AOSAs (2019)](/aosa/archive/2019)

# Mirrors

This section contains information and guides regarding AOSC's Community Repository.

- [How To Mirror the Community Repository](/mirrors/how-to)
