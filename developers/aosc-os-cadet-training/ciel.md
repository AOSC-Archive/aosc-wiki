<!-- TITLE: Ciel -->
<!-- SUBTITLE: Using standardised and containerised environments for AOSC OS packaging. -->

# What is Ciel?

Ciel is a multi-instance management frontend for [BuildKit](/developers/buildkit) containers based on [systemd-nspawn](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) and [OverlayFS](https://www.kernel.org/doc/Documentation/filesystems/overlayfs.txt). Ciel also manages Autobuild3/ACBS configurations, ABBS trees, APT repositories, network behaviours, and various other aspects that affect the build environments.

Thanks to the utilisation of OverlayFS, Ciel also provides mechanism for quickly rolling back build environments to their minimal "clean" state, while retaining a "base" layer for persisting system updates.

# Using Ciel

## Deploying Ciel

If your host machine runs AOSC OS, Ciel is available from the [community repository](https://repo.aosc.io/):

```
# apt install ciel
```

Otherwise, you will have to build Ciel yourself, and take note of the following system requirements:

- The system runs on [systemd](https://www.freedesktop.org/wiki/Software/systemd/), and `systemd-nspawn` is available (found in `systemd-container` for Debian and derivatives, for instance).
- OverlayFS support (the `overlayfs` Kernel module).
- A working Go toolchain (Google or GNU; >= 1.8 tested).

And now, build Ciel:

```
$ git clone https://github.com/AOSC-Dev/ciel
$ make
# make install
```

Finally, find a good place to initialise Ciel:

```
$ cd /path/to/your/future/workspace
# ciel init
```

## Deploying a Container

By default, Ciel downloads and extracts the newest BuildKit tarball found in the repository - simply execute the command below:

```
# ciel load-os
```

*You may also choose to download any other system tarballs from the [system release site](https://releases.aosc.io/), or any other system tarballs that uses the systemd init system (for the purpose of systemd-nspawn).*

## Global Configuration

