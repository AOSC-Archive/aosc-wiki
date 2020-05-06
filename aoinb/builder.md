---
title: aoinb-builder
description: The WIP builder component of AOINB.
published: true
date: 2020-05-06T13:02:12.568Z
tags: dev-automation
---

# Requirements

- `python3`, obviously.
- `python-tomlkit` for TOML parsing.

# Usage

## Config

`aoinb-builder` requires a configuration folder to run. The folder should looks like this:

``` bash
aoinb-builder
├── config.toml
└── source_lists
    ├─── stable.list
    ├─── testing.list
    └─── ... and so on.
```

`config.toml` is the main configuration file. Here is an example.

``` toml
# workDir: where all the work happens.
workDir = "/var/cache/aoinb/work" 
# arch: the architecture of the build machine. only used to determine BuildKit download url for now.
arch = "amd64"
```

The `source_lists` folder contains the `sources.list` file used inside the container. You can change branch and use preferred mirror by editing the corresponding file inside this folder.

## Executable

`aoinb-builder` will automatically populate all the directory it need, so be careful when choosing the working directory in the config file as mentioned before.

Notice that since AOSC OS\'s building process involves mounting OverlayFS and using `systemd-nspawn`, root permission is required.

By default `/var/cache/aoinb/conf` (Maybe move to `/etc`?) is used to search for configs, but you can manually specify the path by offering `-c $PATH`.

There are three possible sub-commands: `build`, `load-buildkit` and `cleanup`. The general workflow is to first load buildkit, then just build packages you want.

### build

This is the main command we will be using. To use it, simply run:

``` bash
python3 aoinb-builder.py $PATH_TO_BUILD_BUNDLE $BRANCH
```

The so-called \"build bundle\" is the folder that contains `spec` file and `autobuild/` folder.

The BRANCH used in the command is the destination branch to build upon. A corresponding `$BRANCH.list` must exists in the `source_lists` folder in the configuration folder, otherwise the build will fail.

### load-buildkit

This command installs BuildKit to the working directory for future builds.

By default, this command will automatically download the latest BuildKit from *repo.aosc.io*. But if that\'s too slow, or you already have a copy of BuildKit, you can just place it in `workdir/buildkit_amd64.tar.xz` and the builder will use that directly.

### cleanup

This command will destroy all contents of the `.builder` directory, which means the extracted buildkit and all instances will be destroyed. Handy if you feel like a fresh start.

# Internals

There are two layers of OverlayFS used.

```
workspace-dir
------------- -> workspace-overlay, workspace-workdir
instance-dir
------------- -> instance-overlay, instance-workdir
buildkit-dir
```

Workspace layer is the temporary place for each build. It will be cleaned up every time the build is finished (weather successful or not).

Instance layer is the layer for different branched. An `apt update && apt upgrade` will be made to this layer each time a build begins.

BuildKit layer is the base layer. It will never be changed unless a manual update command is used (TODO).

Before every build, the \"build bundle\" and the `toolbox` is copied to `/buildroot` inside the container. `build.sh` is called by `systemd-nspawn` for the build process. The script will call `download_source.sh` (and eventually call `downloader.tcl`) to download source code as `$VER.bin` and extract it to `/buildroot/build` (For packages that use SRCTBL). Finally, `build.sh` will execute `autobuild` to actually build the package.

# Goals
* ? Improve `build.sh`
* ? Implement `update-buildkit`
* ? Add support to GITSRC and other types of VCS
Question: Still use bash scripts to prepare source codes in the future?
* ? Monitor system resource usage during build 
