---
title: AOSC OS Maintenance Automation: Design RFCs and Progression
description: Current Design Goals and Progression, Also Serves as an Index for AOINB-related Documentation
published: true
date: 2020-05-01T05:15:52.652Z
tags: 
---

# Automation for Metadata Updates

Automated updates to the ABBS Git repo.

## Requirements

  - Automatically detect updates from the
    upstreams.

  - Commit, or propose, updates to the Git trees…

      - Changing version number, removing REL, etc (what
        if update is only provided as patch? E.g. Bash, Readline,
        …).

      - Updating checksums.

      - Source link migration (The GnuTLS project
        changed their release server topology and they must be changed -
        without 301/302 from the server side - to ensure correct
        building)?

  - Notifying packagers with potential changes with a
    given update…

      - Send along a copy of changelog so dependencies
        and build parameters may be changed with manual
        intervention?

## Current Implementation

  - Information source

      - PISS
        ([https://github.com/AOSC-Dev/piss](https://github.com/AOSC-Dev/piss)):
        upstream version checker.

      - Anitya
        ([https://release-monitoring.org](https://release-monitoring.org/)):
        Fedora’s upstream version checker.

      - Repology
        ([https://repology.org](https://repology.org/)):
        Compare between distro’s package versions.

  - Auto-editing

      - Findupd
        ([https://github.com/AOSC-Dev/scriptlets/blob/master/findupd](https://github.com/AOSC-Dev/scriptlets/blob/master/findupd)):
        List and modify packages’ latest version. Actually two
        implementations.

      - Bump-rel
        ([https://github.com/AOSC-Dev/scriptlets/tree/master/bump-rel](https://github.com/AOSC-Dev/scriptlets/tree/master/bump-rel)):
        bump release number.

      - Increaserel
        ([https://github.com/AOSC-Dev/abbs-meta/blob/master/tools/increaserel.py](https://github.com/AOSC-Dev/abbs-meta/blob/master/tools/increaserel.py)):
        also bumps release number.

      - Addchksum
        ([https://github.com/AOSC-Dev/abbs-meta/blob/master/tools/addchksum.sh](https://github.com/AOSC-Dev/abbs-meta/blob/master/tools/addchksum.sh)):
        download and add checksums to the spec file.

      - Commit-o-matic
        ([https://github.com/AOSC-Dev/scriptlets/tree/master/commit-o-matic](https://github.com/AOSC-Dev/scriptlets/tree/master/commit-o-matic)):
        make git commits.

## Planned Implementation

## Changes needed

  - More information about upstream.

      - Spec files should contain enough information
        about how to get the latest version numbers.

      - Current packages.aosc.io JSON does not detect
        version bumps when its parent link changes (for instance,
        [https://download.gnome.org/sources/gnome-shell/3.24](https://download.gnome.org/sources/gnome-shell/3.24)
        versus
        [https://download.gnome.org/sources/gnome-shell/3.30](https://download.gnome.org/sources/gnome-shell/3.30)).

      - JSON is also unreliable with foo-n (n is a
        number) package names, as it cuts off at the last “-”.

  - Addchksum needs a wrapper script - so packagers
    wouldn’t have to write a for loop just to update checksums
    (automatic patching would also be appreciated).

      - -\> Bai: There is a wrapper script:
        addchksum.**sh**

      - FTP handling (although these source links are
        currently being deprecated, some are here to stay).

# Automation for Building

Build task distribution and collection.

## Requirements


  - Autobuild3 must be more reliable with ABTYPE=self
    packages.

      - Re-implementation needed?\\

      - **Lion**: what is self?

  - Better source spec and downloader.

      - Multiple sources.

      - Patches.

      - Parallel download for many packages.
        (aria2c/multiple wget/etc.)

      - **-\> Yhi:** proxy for download

  - Better dependency resolution by ACBS (abbs-meta
    integration?).

      - De-shell-ification in autobuild/defines.

      - Lion RFC: merge spec and defines?

  - Build result feedback.

      - Package builds failed/succeeded.

      - Package build time/memory/disk requirement
        stats.

          - Build time benchmark (per core) (glibc? LFS
            unit?)

          - Parallel utilization (average CPU% / core
            number)

          - Max memory/disk requirements

      - Build logs.

  - QA.

      - QA throughout the build process (before and
        after checks).

      - Package installation test (may be separated from
        the building process).

          - Can run. (in a container/virtual
            environment).

          - Other specified tests.

      - Package relationship checks (4xx issues).

      - Unified QA feedback (packages.aosc.io).

  - Distributed buildbots.

      - Low-end central server (VPS).

      - BOINC-like, Out-of-box compiling.

      - Distribute tasks based on resources
        available.

      - Machine and package statistics.

      - Provide a path for manual packaging.

      - E-mail reports.

  - Auto-rebuild.

      - Rebuild all reverse sodeps on sover/soname
        changes.

      - Rebuild for Python, Perl, … language packages
        which also require vendor path changes.

      - Implemented by editing spec/define files
        accordingly.

## Current Implementation

  - Ab3: Automatic packaging from specifications
    (autobuild/defines) and scripts (everything else).

  - ACBS: Build process management.

  - Ciel: Build environment (container)
    management.

  - p-vector: apt source generation, and QA
    analysis.

  - Packages site (abbs-meta): metadata collection and
    presentation.

## Planned Implementation

Write your planned implementation here.

### (gumblex)

  - spec file: add following

      - VERCHECK: method of checking updates.

          - -\> Lion: use your syntax

          - github:org/repo

          - gitlab:org/repo

          - gnu:bash
            (对应到http://git.savannah.gnu.org/cgit/
            系列

          - gnu:coreutils

          - freedesktop:dbus/dbus

          - freedesktop:libreoffice/core

          - dirlist:https://example.com/software/

      - CHKPARAM: other parameters for this
        method.

      - VERFMT: format of this package’s version
        number.

          - Which is major, minor, patch / which break
            things.

          - Need discussion.

          - **-\> Yhi:** this looks like a *schema* for
            ab3 defs...

      - WEBSITE: homepage of this package (information
        only).

  - autobuild/tests folder: tests specification. (See
    jeffbai’s plan)

  - ACBS/ADBS: Modularized solution. One tool
    (underlined) for one function.

      - Metadata:
        abbs-meta,
        packages-site/dpkgrepo.py.

          - Move dpkgrepo.py to abbs-meta?

      - Source downloader:
        (adbs/downloader.py).

      - Dependency resolver and topology sorting:
        ([https://github.com/AOSC-Dev/abbs-dep](https://github.com/AOSC-Dev/abbs-dep)).

          - Dependency search stops when the same
            package name and version (abbs=repository) exists.

          - NEW TOOL, DONE: Topology sorting, without
            checking repository version.

      - Build environment manager:

          - Container:
            CIEL

          - Process tree (group):
            (new ACBS)

              - Install deps via apt

              - Use systemd-run --scope for more control
                and stats.

          - QA

              - Ab3

              - Independent
                installation tester running in containers.

                  - Can run?

                  - Check sover problems:
                    bigcat-dbg

      - Build process
        manager: the central server

          - Use PostgreSQL

          - Buildbot management

          - Parse and store uploaded logs and
            stats

          - Do statistics (in database)

          - Move p-vector’s QA function here

      - APT Repo manager:
        p-vector

          - Generate apt files

          - NEW: Manage incoming .deb files (staging
            area)

          - NEW: Manage .deb pool (stacked repo)

  - Two set of solutions, composed using above
    tools

      - Centralized: BOINC-like workflow

      - Distributed: current manual packaging; user
        repo

#### \[gumblex: Added on 2020-02-22\]

We split the system into these connected components,
which can be distributed or centralized.

  - Build Trigger

      - Multiple trigger mode

          - Manual

          - On git update

          - When rebuild required

      - Has full git repo

      - ←Mirror Manager: Has access to deb metadata
        database (optional, rebuild)

      - →Scheduler: Repo url, base commit and
        diff

      - →Scheduler: Build list, with deps?

  - Scheduler (“BOINC Server”)

      - ←Build trigger: local abbs tree snapshot

      - ←Log\&stats server (optional): Estimated build
        time

      - Solve dependencies

      - Use SQLite for build process schedule

      - Download sources (if worker is local)

      - →multiple Worker: JSON Metadata and package
        specs (one folder)

      - ←Worker: .deb package copy to

          - Local proposed repo

          - Online proposed repo (if not manually
            triggered)

  - Worker (ciel & abbs/acbs, “BOINC Client”)

      - Has access to

          - Main repo (eg. stable on mirror)

          - Online proposed repo (eg. stable-proposed on
            mirror)

          - Scheduler’s proposed repo (eg.
            stable-proposed on local machine)

          - ←Scheduler: One single package’s
            specs

          - ←Scheduler: Downloaded sources (if
            local)

      - Download sources (if worker is remote)

      - Create container

      - Install dependencies

      - Make snapshot (1)

      - Install build dependencies

      - Run ab3 in systemd-run --scope

          - Ab3: make structured logs about issue
            numbers

          - Collect stats and logs

          - Watchdog

      - Run automated install tests in the snapshot (1)
        container

      - →Log\&stats server: Upload stats and logs

      - →Scheduler: build status and .deb package

  - Log\&Stats Server

      - ←Worker: Collect build logs, stored in file
        system

      - ←Worker: Collect build stats, stored in
        database

      - →Scheduler: Estimate build time

  - Mirror Manager (p-vector)

      - Scan current packages

      - Store metadata in database (should support
        SQLite)

      - Make catalogues for apt source

          - Stacked repo

      - Relationship QA checks (PostgreSQL
        required)

### (jeffbai)

  - QA in Autobuild3 as a component (or enhancing the
    current implementation).

      - Testing templates defined in Autobuild3.

          - Existing tests include custom/non-standard
            DPKG sections, dangling files, incorrect paths (/usr/local
            in distribution packages, for instance).

          - To add…

          - Source tree permissions (DPKG refuses to
            package files found in a SUID’d directory, so fix that
            automatically).

          - DOS line ending, non-unicode content, …, in
            document files (detect the usual suspects, NEWS, CHANGELOG,
            LICENSE, \*.txt, …).

          - Package naming, versioning, …, according to
            the Styling Manual
            ([https://wiki.aosc.io/developers/aosc-os-package-styling-manual](https://wiki.aosc.io/developers/aosc-os-package-styling-manual)).

          - Files downloaded from non-encrypted/secure
            links (http://, ftp://).

          - Files downloaded without checksum
            verification (may introduce a standardised format to carry
            out this task. Gumble: If multiple source files are to be
            supported in ACBS, then this and the one above will be
            non-issues).

          - Unquoted $SRCDIR and $PKGDIR.

          - Warn packager if hardening is disabled via
            AB\_FLAGS\_SPECS=0, also with other platform-specific
            optimisations (LTO for AMD64 and AArch64, for
            instance).

          - … Will hear from other developers.

      - autobuild/tests/ containing custom test
        scripts.

          - autobuild/tests/pre, inspects source trees
            and fixes potential problems before build.

          - autobuild/tests/post, inspects build results
            before files are installed in $PKGDIR.

          - autobuild/test/install, inspects installed
            files in $PKGDIR (expected files, expected file
            attributes/permissions/ownership/…).

### (lmy441900)

  - A complete re-implementation of all the components
    we are using

      - I personally call it “Ciel 3”

          - If somebody feels confused then we just
            change the name...

      - **All-in-one solution; I am enough of
        remembering new tool names**

      - A distribution-agnostic design

          - First assume Ciel 3 can be used in any Linux
            distribution workflows

              - So the building process is
                abstracted

          - Then integrate AOSC OS specific building
            methods into Ciel 3

              - By writing “provider” scripts hooking to
                various stages of package building defined by Ciel
                3

              - Provider scripts will be in charge of
                parsing and executing distribution-specific files (in
                our case, autobuild3 scripts)

  - Imagine DABBS, which was coined in AOSCC 2016

      - **cield**: There is a
        **central** **controller node**.
        All information gathering and task scheduling are done on this
        node. This is not covered in this document; I haven’t had a
        clear view of it so far

      - **ciel**: when run as a daemon, ciel accepts
        command from cield and perform building (while keeping
        information uploaded back to cield)

  - Ciel 3 incomplete mock-up


  - Another one (about abstracted building
    process)

![](AOSC%20Automation%20RFC_Talk_Discussion_html_255b1143aeeaab78.png)
![](AOSC%20Automation%20RFC_Talk_Discussion_html_606e0b300d25dd80.png)

  - Explanation:

      - The abstracted building process is defined in
        Ciel 3; Ciel 3 executes all procedures in sequence (from top to
        bottom)

      - Distribution provider scripts **hook** to
        different stages of the process, so that when Ciel 3 is
        executing a stage, they will be instead invoked

        1.  Multiple methods can be hooked onto one
            stage

        2.  Each stage has **before** and **after**
            sub-stages for extra work (they can be treated as general
            stages too, though)

      - There is (almost definitely) a set of API
        between provider scripts and Ciel for information passing (if
        you know Lua, imagine it)

      - In theory any distribution can use this model,
        and AOSC OS is only one of the supported distribution of
        Ciel

  - Ciel 3 examples:

      - **ciel build linux-kernel -C**: build package
        linux-kernel defined in the default tree in the current
        environment (not using a container)

      - **ciel tree add aosc /home/foo/aosc-os-abbs**:
        register /home/foo/aosc-os-abbs/ as a ciel managed building
        script / definition tree, named “aosc”

      - **ciel container instance list**: list running
        containers used by Ciel

      - **ciel config packager.identity “Foo Bar
        \<fbar@aosc.xyz\>”:** set the ID of packager (which will be used
        by Ciel when necessary)

      - Command plugins (as is done in Ciel 2)? Of
        course.

  - Algorithm?

      - You have been thinking beyond me, so I decide
        not to spend time on it

      - There are two sets of problems we need to think
        about

        1.  Algorithms running on the builder
            machine

              - Package dependency calculation (whether
                the current packing environment satisfies the
                need)

              - ...

        2.  Algorithms running on (or can be split to)
            the controller machine

              - New packages availability

              - Package dependency calculation (the
                sequence of building)

              - …

      - I personally am against a completely distributed
        resolution

  - Build definition refinements

      - Just one thing: if it’s bash, then why don’t we
        read it as bash scripts?

  - Frankly speaking, I am still not clear about all the
    tools used in our community

      - Maybe someone can draw a diagram for me; I can’t
        think about things that I cannot imagine (this is why I am
        extremely poor in mathematics)
