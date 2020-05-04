---
title: Intro To Package Maintenance
description: Introductory Guide to AOSC OS Packaging
published: true
date: 2020-05-04T04:55:40.127Z
tags: 
---

Welcome! If you are reading this, you are probably interested in contributing to the AOSC OS project. This guide will guide you through all the tools and techniques you need to create, update, and maintain AOSC OS packages.

# Table of contents
## Basics
- [Meet the tools](/developers/intro-to-package-maintenance/basics#meet-the-tools)
- [Release model](/developers/intro-to-package-maintenance/basics#release-model)
- [Setting up the environment](/developers/intro-to-package-maintenance/basics#setting-up-the-environment)
- [Building our very first package!](/developers/intro-to-package-maintenance/basics#building-our-very-first-package)
- [Adding a new package](/developers/intro-to-package-maintenance/basics#adding-a-new-package)
- [A complete example: light](/developers/intro-to-package-maintenance/basics#a-complete-example-light)

## Advanced Techniques
- [Advanced Operations in Autobuild3](/developers/intro-to-package-maintenance/advanced-techniques#advanced-operations-in-autobuild-3)
	- [Manually Select Different Build Systems](/developers/intro-to-package-maintenance/advanced-techniques#manually-select-different-build-systems)
	- [Custom Build System/Compiler Parameters](/developers/intro-to-package-maintenance/advanced-techniques#custom-build-system-compiler-parameters)
	- [Custom Build Scripts](/developers/intro-to-package-maintenance/advanced-techniques#custom-build-scripts)
	- [Post-Build Tweaks](/developers/intro-to-package-maintenance/advanced-techniques#post-build-tweaks)
	- [The autobuild/override Directory](/developers/intro-to-package-maintenance/advanced-techniques#the-autobuild-override-directory)
	- [Advanced Patch Management](https://wiki.aosc.io/developers/intro-to-package-maintenance/advanced-techniques#advanced-patch-management)
- [Dealing with Package Groups](/developers/intro-to-package-maintenance/advanced-techniques#dealing-with-package-groups)

## Appendix
Here are some useful documentations about the tools we use.
- [Autobuild3 Manual](/developers/intro-to-package-maintenance/autobuild3-manual)
- [Ciel Manual](/developers/intro-to-package-maintenance/ciel-manual)