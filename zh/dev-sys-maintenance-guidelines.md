---
title: AOSC OS 维护指南（征求意见稿）
description: 过好生活，打好包包
published: true
date: 2020-08-07T11:36:00.632Z
tags: dev-sys
editor: markdown
---

# 概述

AOSC OS 是一个有着多分支的半滚动更新 Linux 发行版，具有 5000 个以上的软件包，支持多种电脑处理器架构，因此维护 AOSC OS 并不是一件容易的事。为了确保 AOSC OS 的发布质量，我们编写了下面的指南，希望所有维护者都能遵循。

# 分支、发行周期与架构

分支、发行周期与架构在 AOSC OS 维护过程中是三个非常重要的概念。在本节中，我们将在发行版的范围内提供这些概念的定义。

## 分支

AOSC OS 在同时维护下面的四个分支：

- **稳定分支**（`stable`）：一般用户应使用的分支，我们通常向这个分支推送安全更新、漏洞修复、[异常更新](/dev-sys-cycle-exceptions) 和 [补丁级更新](/dev-sys-known-patch-release-rules)。
  - 通常使用 `stable-proposed` 向 `stable` 输送上述更新（紧急情况除外，如稳定分支出现了严重的依赖破坏）。
- **测试分支**（`testing`）：发烧友可以在这里获取经过**少量测试**的最新的功能性更新、安全性更新以及新软件包。在每个发行周期结束之前，此分支提供的更新会合并入稳定分支。
  - 通常使用 `testing-proposed` 向 `testing` 输送上述更新（前者也是多数软件包最初上传的地方）。
- **不稳定分支**（`explosive`）：在发行周期外接受**任何**新软件包和更新。所有人都不应该使用此分支。
  - 此分支还用于上传需要大规模重建的更新（例如 Python 3.7 ➙ 3.8）。这样做的目的是：由于 `explosive` 从未冻结，如果这些更新无法在当前发行周期发布或推送，则不会影响到其它更新。
- **RC 版内核测试分支**（`rckernel`）：作为 `stable-proposed` 的一个补充，用于测试候选版本的 Linux 内核及其工具链。

## 发行周期

AOSC OS 采用的是半滚动更新模型，通常每个发布周期都为时三个月。

- 前两个月为开发期：
  - 我们会在 GitHub 上发布迭代计划（例如 [2019 年夏季迭代计划](https://github.com/AOSC-Dev/aosc-os-abbs/issues/1896)），并由维护者实时更新迭代进展。
  - 维护者可随时为合适的分支和架构上传更新。
- 第三个月为冻结期：
  - `stable`、`stable-proposed`、`explosive` 和 `rckernel` 照常接收更新。
  - `testing` 将不再接受除了安全更新和漏洞修复之外的其它更新。
  - `testing` 将作充分的测试以准备合并入 `stable`。

## 架构

AOSC OS 支持多种电脑处理器架构，适配多种设备。然而，AOSC OS 对不同架构提供的支持不一定完全相同：

- AMD64（`amd64`，也被称为 x86_64)
  - 此架构是我们最为关心的架构。
  - 所有软件包在构建时都将针对此架构进行测试。
  - 当我们为 `explosive` 分支构建更新时只考虑此架构。
- AArch64（`arm64`）、ARMv7（`armel`）、Little Endian POWER（`ppc64el`）以及 RISC-V（`riscv64`）
	- 所有软件包在打包时都应该提供适用于这些架构的版本，除非确实无法构建。
  - `explosive` 分支不适用于上述架构。
- Big Endian PowerPC 32/64-bit（分别为 `powerpc` 和 `ppc64`）以及 i586（`i586`）
	- 这些架构属于 AOSC OS/Retro 项目的一部分，不适用到目前为止指定的所有规则。
  - 我们只为这些架构提供 `stable` 分支，并提供在旧硬件上可用的有限的软件包。

# 实践与行动

本节列出了 AOSC OS 维护者应遵循的事项。

## 工具集

所有软件包维护者都应该使用下面的标准工具集以保证软件包的可用性和构建流程的可重现性。

- [Autobuild3](https://github.com/AOSC-Dev/autobuild3)：软件包构建工具，它将复杂的构建过程和软件包元数据抽象成了 Autobuild3 清单。
- [ACBS](https://github.com/AOSC-Dev/acbs)：以树状方式组织和管理 Autobuild3 清单，详情请查看 [aosc-os-abbs](https://github.com/AOSC-Dev/aosc-os-abbs)。
- [Ciel](https://github.com/AOSC-Dev/ciel)：管理用于打包工作的 `systemd-nspawn` 容器。支持 Autobuild3/ACBS 配置以及容器系统升级、配置和回滚。
- [pushpkg](https://github.com/AOSC-Dev/scriptlets/tree/master/pushpkg)：一组用于将软件包上传到 [软件仓库](https://repo.aosc.io) 的脚本。

你可能需要一个 LDAP 凭证来上传软件包或访问我们的中继服务器（[Buildbots](/en/infra-buildbots)）。

## Buildbots

尽管你可以使用上面提到的工具在自己的设备上打包，社区也提供了一些高性能机器供维护者使用。

要了解更多，请参阅社区维基 [Buildbots](/en/infra-buildbots) 一文。

## 软件包接收

原则上，AOSC OS 允许任何类型的软件包进入其软件仓库，但以下情况除外：

- 供应商或上游不允许我们分发该软件或在 [软件包样式指南](/dev-sys-package-styling-manual) 要求的情形下不允许我们修改文件路径。
- 供应商或上游拒绝或没有提供安全漏洞修复。
- 该包已被供应商或上游弃用。
- 维护者投票反对引入（或维护）该软件包。

## The Builds

While building packages, the build environments *must* be controlled, updated, and minimal, where packages are only installed as required by the build-and-run-time dependencies.

- For instance, when building for `stable`, make sure that *only* the `stable` branch is enabled in your repository configuration; `explosive`, `testing-proposed`, `testing`, `stable-proposed`, and `stable` are enabled when building for `explosive`; ...
- There is an exception when building for `stable-proposed` and `rckernel`, *only* the `stable` branch should be enabled.

## Branch Merging

In the AOSC OS maintenance procedures, branch mergings are bi-directional.

### Merging

With the branch and cycle descriptions specified above - as branch merging serves as the main mean of update introduction for the non-Proposal branches, there are certain limitations applied to the merging procedures.

- During the active phase of an interation cycle, the branch merging follows the direction of: `explosive`; `testing-proposed` → `testing`; `rckernel` → `stable-proposed` → `stable`.
- At the end of each iteration cycle, a full merge would be made: `explosive` → `testing-proposed` → `testing` → `stable-proposed` → `stable`.
- During the one-month freezing periods: the `rckernel` → `stable-proposed` → `stable` merges are permitted, while all other mergings are *not permitted*.

### Reversed Merging

Reversed merges, or `stable` → `stable-proposed` → `testing` → `explosive` merges should be done periodically regardless of the cycle periods. However, during major merges, notifications will be made to prevent inconsistent merging.

The `rckernel` branch should also receive periodic reverse merging: `stable` → `stable-proposed` → `rckernel`.

## Stable Update Testing

Updates for the `stable` branch, unless known to be involved with one or more 0-day security vulnerability(ies) or already broken in `stable`, are committed, built, and tested through the `stable-proposed` branches. `stable-proposed` updates are merged in the following procedure/schedule:

- Every Saturday at midnight, UTC: An issue is generated with a *comprehensive* list of updates committed and bulit on the `stable-proposed` branch.
	- Package names, version deltas (e.g. 1.0.2 → 1.0.3), and changelogs (linked to specific [AOSC OS Packages](https://packages.aosc.io) pages).
	- Checkboxes by each entry.
- Testing procedures are defined case-by-case.
	- TODO: Autobuild3/ACBS automatic quality assurance and reports.
	- Basic functionalities (Launches? Comes with necessary files? Documentation readable and complete?).
	- Styling checked against the [Styling Manual](/dev-sys-package-styling-manual).
- Packages, when tested, will have their respective entry(s) ticked, and packages moved on the repository from the `stable-proposed` pool to the `stable` pool on the package unit (one package "ticked", one moved).
- The weekly issues will remain open for tracking testing work, and closed upon *full completion* (all checkboxes ticked).