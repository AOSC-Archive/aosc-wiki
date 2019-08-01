<!-- TITLE: AOSC OS Maintenance Guidelines (RFC) -->
<!-- SUBTITLE: General Procedural Guidlelines for AOSC OS Package Maintenance -->

# Introduction

AOSC OS is a multi-branch, seasonal rolling Linux distribution available with over 5,000 packages for multiple microprocessor architectures. As such, maintaining AOSC OS is not a trivial task, and all maintainers should work in accordance to a written guideline to avoid confusion and (the resulting) inconsistent quality.

# The Branches, Cycles, and Ports

The concepts of branches, cycles, and ports are three main aspects that maintenance work revolves around. In this section, we will defines these concepts in the scope of AOSC OS.

## The Branches

AOSC OS is maintained *concurrently* across four branches:

- Stable (`stable`): The main maintenance branch which most users should be using, updates include security updates, bug fixes, [exceptional updates](https://wiki.aosc.io/developers/aosc-os/cycle-exceptions) and [patch-level updates](https://wiki.aosc.io/developers/aosc-os/known-patch-release-rules).
	- Stable, Proposed Updates (`stable-proposed`): The branch that feeds said updates into `stable`, unless the current `stable` already requires bug fixes (for instance, a currently available `stable` package has broken dependency).
- Testing (`testing`): The main feature branch which users with particular interest in following the latest development and changes should be using, security updates, feature/major updates, and new packages are introduced from the `explosive` branch and tested *minimally* before shipping. Updates made available through this branch will be available for `stable` by the end of each update cycle.
	- Testing, Proposed Updates (`explosive`): The branch that feeds said updates into `testing`, packages are introduced and *build-time tested*. No one should be using this branch, no matter what.

## The Cycles

AOSC OS is maintained on an seasonal cycle, and thus revolves around a three-month schedule:

- The first two months:
	- Iteration Plans (e.g. the *[Iteration Plan for Summer 2019](https://github.com/AOSC-Dev/aosc-os-abbs/issues/1896)*)are drafted and published on GitHub as a milestone issue, and updated frequently by maintainers as new updates are made available and built.
	- Updates are built on all branches and for all ports when applicable.
- The last month - or the freezing period:
	- The `stable`, `stable-proposed`, and `explosive` branches continues to receive updates as usual.
	- The `testing` branch will no longer accept updates unless they are intended for security or bug fixes.

## The Ports

AOSC OS is available for many different microprocessor architectures (and thus many different kinds of devices). While some ports will received full-feature packages, there are distinctions based on the nature of the ports. Here below is a brief description:

- AMD64, or x86_64 (`amd64`), the *de facto* main port.
	- All packages will be build-time tested for this architecture.
	- This is the only architecture to build `explosive` updates as a preliminary procedure.
- AArch64 (`arm64`), ARMv7 (`armel`), Little Endian POWER (`ppc64el`), and RISC-V (`riscv64`).
	- All packages should be built for these architectures unless non-applicable or unbuildable.
	- These architectures do not track the `explosive` branch.
- Big Endian PowerPC 32/64-bit (`powerpc` and `ppc64`, respectively), and i586 (`i586`).
	- These architectures are considered part of the "AOSC OS/Retro" project, and do not follow all the rules specified so far. (DOCUMENTATION PENDING).
	- These architectures only track the `stable` branch, with a limited selection of packages deemed usable on older hardware.

