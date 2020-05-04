---
title: KB-SYS-00001: Configuration of APT Repositories
description: Information on Enabling or Disabling APT Repositories on AOSC OS
published: true
date: 2020-05-04T03:37:25.665Z
tags: 
---

# APT Configuration

This page describes the `apt-gen-list` utility pre-installed with AOSC OS. This utility is used to configure (enable or disable) mirrors used for APT software repositories, and to generate `/etc/apt/sources.list` for APT to use. This could be handy, with knowledge of fastest repository mirrors at your location.

Shown below is the original usage message for this utility:

```
Utility for generating sources.list for APT according to available
repository configurations.

Usage:  /bin/apt-gen-list -e "repo1 repo2 ..." -d "repo1 repo2 ..."

-l      List available repositories.
-c      Create a default sources.list.
-e      Enable repositories.
-d      Disable repositories.
-h      Display help.

If -c is specified, I will only repositories with a rating of 1,
and disable all others.
```
