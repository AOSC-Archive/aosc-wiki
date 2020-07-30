---
title: AOSC OS 适合我吗？
description: 一些重要的和一些没那么重要的信息...
published: true
date: 2020-07-30T05:42:37.511Z
tags: sys-info
editor: markdown
---

# AOSC OS 是什么？

好问题，我们一直都在探讨这个问题。但是经过七年的开发，我们越来越清楚地意识到，我们正在构建的 Linux 发行版有着一些我们认为非常重要的特性。在此之前，让我们回到出发点...

自 2014 年以来，AOSC OS 就一直是一个独立构建和维护的发行版，这和 Ubuntu 等发行版不同，Ubuntu 是基于 Debian 的软件包构建的，而 AOSC OS 开发者则是从源码包的处理，配置的修改，到二进制包的制作都是一手包办的。并不是说这是一个明显的特点，但是这样做能让我们按照社区的哲学和建议来制作我们真的想要的 Linux 发行版。

AOSC OS 只是几千个 Linux 发行版中的其中一个，它建立在强大的 Linux 内核和由世界各地的开发者开发的各类应用程序之上。我们知道选择 Linux 发行版是很困难的，这个维基页面的目标只是为 AOSC OS 提供一个多维度的描述，以帮助您决策。

# 我们在尝试实现什么？

AOSC OS 是由一群志愿者经过多年努力构建的，这些志愿者对制作 Linux 发行版有一定程度的了解。AOSC OS 并不是从它诞生的第一天开始就是一个独立的发行版的，最初的三年我们尝试的是制作基于 openSUSE 和 Debian 的衍生发行版。到了 2014 年，我们才决定独立构建 AOSC OS 以全面掌控软件包及其更新的设计和维护。 

在后面的数年中，我们一直致力于将 AOSC OS 建立在以下目标和理念之上：

- 不必要的分包应该被避免（例如将 `libsndfile` 拆分为运行时库 `libsndfile` 和开发包 `libsndfile-devel`）。
- 软件包应该尽量贴近上游，只有上游的软件包有重大漏洞有待修复或在 AOSC OS 无法工作才考虑打补丁。
- 尽可能给不同语言的使用者相同的体验（包括字体渲染、本地化显示等等），特别是中日韩语言使用者。
- 虽然开源和自由的软件应该是软件源最重要的组成部分，闭源和非自由软件也不应被刻意排除在外（除非软件许可证要求）。
- 尽可能地减少 Branding 和其它发行版相关却无关紧要的元素，因为发行版本身只应该是一个满足用户需要的工具。
- 更新不应该破坏用户的使用体验，尤其不应该破坏系统的可用性。但是软件更新也应该尽可能早地发布。
- AOSC OS 应该在不同架构上提供尽可能相近的功能和体验，只在必要的情况下作增减。

# AOSC OS 到底长什么样子？

AOSC OS 基本上就是按照上面提到的这些理念来构建的。为了避免长篇大论，我们会把我们对这个问题的回答分为两部分，优点和缺点。

## 优点

- AOSC OS installs quickly, works out of the box, and comes with most tools necessary to begin working from the 0th minute, including utilities necessary for Internet connection in restrictive countries and regions.
- It's relatively simple, when it comes to software installation and removal.
- All "language packs" along with appropriate fonts are pre-installed, and a simple command or GUI (GNOME and Plasma Desktop) allows for quick language switching.
	- Some AOSC developers have been participating in L10n (localisation, or simply translation) work upstream, mainly for zh_CN (Simplified Chinese). Upstreams like WineHQ, FreeDesktop.org, GNOME, MATE, and LMMS, etc. benefit from our contribution, and these improvements benefit users of other Linux distributions or these specific applications on other operating systems.
- Adequate support for 32-bit x86 applications (natively on AMD64, and via Qemu user emulation on other architectures) and Windows applications via Wine, with adequate testing and adaptation for CJK languages.
- Apart from the installation process, you should feel no difference in appearance and usage with AOSC OS across our [supported architectures](/users/information/arch-specs) - of course, depending on the performance of your devices, your mileage may vary.
- Proprietary software, like the closed-source NVIDIA Unix driver packages, Google Chrome, Opera, and device firmwares (`firmware-nonfree` necessary for many wireless cards and graphics cards) can be easily obtained via our main repository.
- A relatively strong software collection built upon user suggestions and active work of our developers.
	- Software addition and updates could be easily requested on our community IRC and Telegram groups via the `/pakreq` and `/updreq` bot commands; additionally, any optimisation suggestions could be similarly made via the `/optreq` command. Developers can be easily reached most of the time.
- According to user feedback, AOSC OS has satisfactory out-of-the-box energy conservation policies, and it runs relatively cool on laptop computers.
- AOSC OS optimises its binary according to a "maximised set" of instructions and SIMDs available to particular architectures.
- You'd barely feel the existence of AOSC OS, no logo, no branding, no in-your-face banners.


## 缺点

- As it currently stands, AOSC OS does not provide an automatic (or even better, graphical) installation wizard, and requires manual input to install - which would most likely be daunting for first-timers. It also lacks a Live medium for user try-outs.
- It's **heavy**, as packages and software are installed on a larger unit, it requires more available storage than most Linux distributions with similar amount of applications installed.
	- This issue is further exacerbated with pre-installed language data and fonts, especially CJK fonts, which could take up to a gigabyte of space in most default configurations.
	- AOSC OS's modularity is noticeably hampered by this feature.
- No `multilib` or `multiarch`, making cross-architecture development more difficult than "universal" distributions like Fedora and Debian - heavy reliance on Qemu user emulation.
- AOSC OS will not make it into the Free Software Foundation's [List of Free GNU/Linux Distributions](https://www.gnu.org/distros/free-distros.en.html) in the foreseeable future, for it provides non-free software, and is (probably?) illegal to re-distribute in countries like the United States.
- Compared to more mature Linux distributions, AOSC OS should still grow its software repository.
- Limited human resource, which means package updates and additions (actively or requested) could be delayed.
	- Bugs may exist and come unnoticed, and they can also take time to be fixed.
- You'd barely feel the existence of AOSC OS, or for others to realise what you are running - no show-off for you, by default.
	- Also, let's face it, we are not particularly well-known.

# 我应该尝试 AOSC OS 吗？

相信你已经看厌了各种营销文案，这里会尽量提供一份主观但合理（?）的指引供你决策。

## 一些适合你使用 AOSC OS 的场景

- 你有一台有着 [AMD64](/users/installation/amd64-notes-sysreq)、[ARMv7](/users/installation/arm-notes-sysreq)、[AArch64](/users/installation/arm-notes-sysreq)、[Big-Endian PowerPC 32-bit 64-bit Macintosh](/users/installation/powermac-notes-sysreq) 中任一架构的电脑。
  - 根据您的判断，无论是现在还是将来，这台电脑都会有着足够的储存空间。
  - 这台电脑有着稳定的网络连接，以用于接收版本更新和安全修复。
- 你并不特别喜欢配置你的系统，而是期望无需过多的调整就能获得一个能用的系统。
- 你是中日韩文字的使用者。
- 你认为这个系统能提供满足你日常使用需要的软件，并准备好在找不到所需的软件的时候向社区发出请求。
- 你有一定的耐心等候新的更新推送，且不指望更新推送在上游发布新版本的第一天就立刻有。
- 你重视系统的安全性，而且希望尽可能快地获得安全更新。
- 你重视电脑的电源情况，而且不希望运行 Linux 时你的系统过热。
- 你不看重发行版的 Branding。
- 你希望参与到 AOSC OS 开发中或者探究 AOSC OS 的实现。

## 一些不建议你使用 AOSC OS 的场景

- 你从未使用过 Linux，也没有掌握基本的 Linux/\*nix 命令，也不愿意尝试阅读各种文档并按照上面的指示完成各步。
- 你期望一个轻量级的、模块化程度高的 Linux 发行版。这并不是 AOSC OS 被创建的原因。
- 你讨厌频繁的更新推送，或对 cutting-edge 特性感到厌烦。
- 你希望系统 100% 开箱即用，或者要求系统在你的需求下必须要有绝对的稳定性。很抱歉我们还没能做到。
- 你是个狂热的开源软件或自由软件的爱好者，不希望任何闭源或非自由软件出现在你的电脑中。
- 你仅仅是希望别人知道你在使用 AOSC OS。
