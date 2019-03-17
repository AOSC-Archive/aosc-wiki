<!-- TITLE: AOSC OS Cadet Training -->
<!-- SUBTITLE: Introductory Guide to AOSC OS Packaging -->

The *AOSC OS Cadet Training* is a brief introduction to AOSC OS packaging using our [Autobuild3](https://github.com/AOSC-Dev/autobuild3) package manager abstraction toolkit, and [ACBS](https://github.com/AOSC-Dev/acbs), or Autobuild CI Build Service to maintain Autobuild3 build configurations/manifests in a tree-like
manner.

# Table of Content

## Introduction

- [Preparing a build environment](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing)
	- [What is BuildKit?](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-What-is-BuildKit)
	- [Getting BuildKit](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Getting-BuildKit)
	- [Deploying BuildKit](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Getting-BuildKit#deploying)
		- [Chroot](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Getting-BuildKit#chroot)
		- [systemd-nspawn](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Getting-BuildKit#systemd-nspawn)
		- [Docker](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Getting-BuildKit#docker)
- [Grab the tools](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Grab-the-Tools)
	- [Autobuild3](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Grab-the-Tools#autobuild3)
	- [ACBS](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Grab-the-Tools#acbs)
	- [ABBS](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Grab-the-Tools#abbs) (Deprecated)

## Autobuild3

- [Introduction](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3)
- [Installing and deploying](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#installing-and-deploying)
- [Initial configurations](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#initial-configurations)
- [General structure](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#general-structure)
	- [The "defines" file](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#the-defines-file)
	- [Build Types](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#build-types)
		- [autotools](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#autotools)
		- [cmake](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#cmake)
		- [waf](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#waf)
		- [plainmake](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#build-types)
		- [haskell](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#build-types)
		- [perl](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#build-types)
		- [python](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#python)
		- [qtproj](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#qtproj)
		- [ruby](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#build-types)
	- [The "prepare" file](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#the-prepare-file)
	- [The "patches" directory](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#the-patches-directory)
	- [The "patch" file](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#the-patch-file)
	- [The "build" file](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#the-build-file)
	- [The "beyond" file](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#the-beyond-file)
	- [The "overrides" directory](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3#the-overrides-directory)
- [Architectural manipulation](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3-Architectural-Manipulation)
- [Quirks](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3-Quirks)
- [Scriptlets](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3-Scriptlets)
- [File filters/manipulators](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3-Filters)
- [Package managers](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3-Package-Managers)
- [Contributed scripts](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3-Contributed-Scripts)
- [Tips and tricks](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Autobuild3-Tips-and-Tricks)

## ACBS
- [Introduction](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS)
	- [Getting ACBS](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS#getting-acbs)
- [Trees?](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-Tree)
- [Management](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-Tree#management)
	- [Planting a tree](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-Tree#planting-a-tree)
	- [Maintaining a tree](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-Tree#maintaining-a-tree)
- [The AOSC OS tree](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-AOSC)
	- [Rationale](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-AOSC#rationale)
	- [Routines and protocols](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-AOSC#routines-and-protocols)
	- [Get in touch](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ACBS-AOSC#get-in-touch)

## Ciel

## ABBS (Deprecated)

- [Introduction](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS)
	- [Getting ABBS](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS#getting-abbs)
- [Trees?](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-Tree)
- [Management](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-Tree#management)
	- [Planting a tree](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-Tree#planting-a-tree)
	- [Building from a tree](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-Tree#building-from-a-tree)
	- [Maintaining a tree](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-Tree#maintaining-a-tree)
- [The AOSC OS tree](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-AOSC)
	- [Rationale](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-AOSC#rationale)
	- [Routines and protocols](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-AOSC#routines-and-protocols)
	- [Get in touch](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/ABBS-AOSC#get-in-touch)

## Errata

Found an error in this documentation? Suggestions? Requesting clarifications? Create an issue at the [aosc-wiki](https://github.com/AOSC-Dev/aosc-wiki) repository.