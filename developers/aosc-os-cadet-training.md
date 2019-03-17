<!-- TITLE: AOSC OS Cadet Training -->
<!-- SUBTITLE: Introductory Guide to AOSC OS Packaging -->

The *AOSC OS Cadet Training* is a brief introduction to AOSC OS packaging using our [Autobuild3](https://github.com/AOSC-Dev/autobuild3) package manager abstraction toolkit, and [ACBS](https://github.com/AOSC-Dev/acbs), or Autobuild CI Build Service to maintain Autobuild3 build configurations/manifests in a tree-like
manner.

# Table of Content

## Introduction

- [Preparing a build environment](/developers/aosc-os-cadet-training/Preparing)
	- [What is BuildKit?](/developers/aosc-os-cadet-training/Preparing-What-is-BuildKit)
	- [Getting BuildKit](/developers/aosc-os-cadet-training/Preparing-Getting-BuildKit)
	- [Deploying BuildKit](/developers/aosc-os-cadet-training/Preparing-Getting-BuildKit#deploying)
		- [Chroot](/developers/aosc-os-cadet-training/Preparing-Getting-BuildKit#chroot)
		- [systemd-nspawn](/developers/aosc-os-cadet-training/Preparing-Getting-BuildKit#systemd-nspawn)
		- [Docker](/developers/aosc-os-cadet-training/Preparing-Getting-BuildKit#docker)
- [Grab the tools](/developers/aosc-os-cadet-training/Preparing-Grab-the-Tools)
	- [Autobuild3](/developers/aosc-os-cadet-training/Preparing-Grab-the-Tools#autobuild3)
	- [ACBS](/developers/aosc-os-cadet-training/Preparing-Grab-the-Tools#acbs)
	- [ABBS](/developers/aosc-os-cadet-training/Preparing-Grab-the-Tools#abbs) (Deprecated)

## Autobuild3

- [Introduction](/developers/aosc-os-cadet-training/Autobuild3)
- [Installing and deploying](/developers/aosc-os-cadet-training/Autobuild3#installing-and-deploying)
- [Initial configurations](/developers/aosc-os-cadet-training/Autobuild3#initial-configurations)
- [General structure](/developers/aosc-os-cadet-training/Autobuild3#general-structure)
	- [The "defines" file](/developers/aosc-os-cadet-training/Autobuild3#the-defines-file)
	- [Build Types](/developers/aosc-os-cadet-training/Autobuild3#build-types)
		- [autotools](/developers/aosc-os-cadet-training/Autobuild3#autotools)
		- [cmake](/developers/aosc-os-cadet-training/Autobuild3#cmake)
		- [waf](/developers/aosc-os-cadet-training/Autobuild3#waf)
		- [plainmake](/developers/aosc-os-cadet-training/Autobuild3#build-types)
		- [haskell](/developers/aosc-os-cadet-training/Autobuild3#build-types)
		- [perl](/developers/aosc-os-cadet-training/Autobuild3#build-types)
		- [python](/developers/aosc-os-cadet-training/Autobuild3#python)
		- [qtproj](/developers/aosc-os-cadet-training/Autobuild3#qtproj)
		- [ruby](/developers/aosc-os-cadet-training/Autobuild3#build-types)
	- [The "prepare" file](/developers/aosc-os-cadet-training/Autobuild3#the-prepare-file)
	- [The "patches" directory](/developers/aosc-os-cadet-training/Autobuild3#the-patches-directory)
	- [The "patch" file](/developers/aosc-os-cadet-training/Autobuild3#the-patch-file)
	- [The "build" file](/developers/aosc-os-cadet-training/Autobuild3#the-build-file)
	- [The "beyond" file](/developers/aosc-os-cadet-training/Autobuild3#the-beyond-file)
	- [The "overrides" directory](/developers/aosc-os-cadet-training/Autobuild3#the-overrides-directory)
- [Architectural manipulation](/developers/aosc-os-cadet-training/Autobuild3-Architectural-Manipulation)
- [Quirks](/developers/aosc-os-cadet-training/Autobuild3-Quirks)
- [Scriptlets](/developers/aosc-os-cadet-training/Autobuild3-Scriptlets)
- [File filters/manipulators](/developers/aosc-os-cadet-training/Autobuild3-Filters)
- [Package managers](/developers/aosc-os-cadet-training/Autobuild3-Package-Managers)
- [Contributed scripts](/developers/aosc-os-cadet-training/Autobuild3-Contributed-Scripts)
- [Tips and tricks](/developers/aosc-os-cadet-training/Autobuild3-Tips-and-Tricks)

## ACBS
- [Introduction](/developers/aosc-os-cadet-training/ACBS)
	- [Getting ACBS](/developers/aosc-os-cadet-training/ACBS#getting-acbs)
- [Trees?](/developers/aosc-os-cadet-training/ACBS-Tree)
- [Management](/developers/aosc-os-cadet-training/ACBS-Tree#management)
	- [Planting a tree](/developers/aosc-os-cadet-training/ACBS-Tree#planting-a-tree)
	- [Maintaining a tree](/developers/aosc-os-cadet-training/ACBS-Tree#maintaining-a-tree)
- [The AOSC OS tree](/developers/aosc-os-cadet-training/ACBS-AOSC)
	- [Rationale](/developers/aosc-os-cadet-training/ACBS-AOSC#rationale)
	- [Routines and protocols](/developers/aosc-os-cadet-training/ACBS-AOSC#routines-and-protocols)
	- [Get in touch](/developers/aosc-os-cadet-training/ACBS-AOSC#get-in-touch)

## Ciel

## ABBS (Deprecated)

- [Introduction](/developers/aosc-os-cadet-training/ABBS)
	- [Getting ABBS](/developers/aosc-os-cadet-training/ABBS#getting-abbs)
- [Trees?](/developers/aosc-os-cadet-training/ABBS-Tree)
- [Management](/developers/aosc-os-cadet-training/ABBS-Tree#management)
	- [Planting a tree](/developers/aosc-os-cadet-training/ABBS-Tree#planting-a-tree)
	- [Building from a tree](/developers/aosc-os-cadet-training/ABBS-Tree#building-from-a-tree)
	- [Maintaining a tree](/developers/aosc-os-cadet-training/ABBS-Tree#maintaining-a-tree)
- [The AOSC OS tree](/developers/aosc-os-cadet-training/ABBS-AOSC)
	- [Rationale](/developers/aosc-os-cadet-training/ABBS-AOSC#rationale)
	- [Routines and protocols](/developers/aosc-os-cadet-training/ABBS-AOSC#routines-and-protocols)
	- [Get in touch](/developers/aosc-os-cadet-training/ABBS-AOSC#get-in-touch)

## Errata

Found an error in this documentation? Suggestions? Requesting clarifications? Create an issue at the [aosc-wiki](https://github.com/AOSC-Dev/aosc-wiki) repository.