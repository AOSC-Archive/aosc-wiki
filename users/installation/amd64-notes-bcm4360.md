<!-- TITLE: Installation/AMD64/BCM4360 -->
<!-- SUBTITLE: Notes for AOSC OS Installation on Devices with BCM4360-based Wireless Cards -->

Notes for Broadcom BCM4360 wireless
===============

Uh-oh. You've reached where __excessive manual intervention__ is required. Most linux does not support BCM4360 out-of-the-box, which means you will not be able to access internet once you entered linux environment if Broadcom's (notoriously) restricted copyrighted drivers. But to compile the driver, you need network access in the first place.

First time setup
-------------

### The easy way: bootstrap state of mind

#### Find another handy wireless card

If you have an USB wireless card, try it. Another good idea is to connect your Android Phone to a wifi network and enable USB tethering.

#### Installation

If you managed to obtain internet access in this way, proceed to [regular installation guide](/users/installation/amd64). Go for the hard way otherwise.

__After__ AOSC OS installation is correctly working on your computer (except for the builtin wireless card), get ready to [deploy buildkit](#Deploy-buildkit) and [build broadcom driver for system](#build-driver-for-system).

### The hard way: everything before hand

Before starting [regular installation guide](/users/installation/amd64). Please download the following beforehand to a handy USB drive:

#### Tarballs:

1. the __system__ tarball of your choice
2. the __buildkit__ tarball
3. a `tar.gz` snapshot of [cth451's tree](https://github.com/cthbleachbit/back-to-the-abbs)

Follow [regular installation guide](/users/installation/amd64) with the __system__ tarball that you've just downloaded.

__After__ AOSC OS installation is correctly working on your computer  (except for the builtin wireless card)...

#### Dependency packages for offline compilation

Use your old OS (which has internet access), search in [Software repository](https://repo.aosc.io/os-amd64/os3-dpkg/) for the following packages. The package is stored in the folder named after the starting characters of package name and is a file named `<package name>_<version number>_amd64.deb`. For example to download `gcc`, look in folder `g` and download `gcc_6.3.0-0_amd64.deb`.

__This wiki might not reflect actual file name on the repository, since the version number will change as the software is updated from time to time. BE SURE TO DOWNLOAD THE LATEST ONE.__

1. `dkms`
2. `gcc`
3. `binutil`
4. `mpc`
5. `isl`
6. `mpfr`

After obtaining these packages, copy them on to a disk drive. Get ready to [deploy buildkit](#Deploy-buildkit), [manually install required packages](#Install-dependencies) and [build broadcom driver](#build-driver-for-system).

#### Deploy buildkit

Refers to [AOSC Cadet training](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Getting-BuildKit) on how to setup buildkit on your local environment. Skip all the steps that require network access.

#### Install dependencies

Copy all the packages you've downloaded to the drive to both your home directory in __system__ and a folder inside __buildkit__ environment.

All commands here in this section should be run __twice__, once in buildkit environment, once in actual system.

Say, if you copied the packages into `/path/to/packages`, execute the command as root below:

```
cd /path/to/packages
dpkg -i *.deb
```

#### Build driver for system

Say if you stored buildkit in `/path/to/buildkit` and extracted the snapshot of cth451's acbs tree to `/path/to/buildkit/path/to/cth451-tree`, proceed as root.

Enter buildkit as said in [AOSC Cadet training](https://github.com/AOSC-Dev/aosc-os-abbs/wiki/Preparing-Getting-BuildKit).

Edit `/etc/acbs/forest.conf`, append the following lines:

```
[btta]
location = /path/to/cth451-tree
```

Then execute `acbs-build -t btta broadcom-wl`, expect an error on fail to load module. Exit buildkit, you will find built driver in `/path/to/buildkit/os-amd64/os3-dpkg/b/`, use `dpkg -i /path/to/buildkit/os-amd64/os3-dpkg/b/*.deb` in _actual system_ as root.

Reboot your computer, wireless card should be working now.

Troubleshooting
------------

### When installing completed driver package my system says broken dependencies.

Check if you installed the dependency packages you [downloaded](#Dependency-packages-for-offline-compilation) is installed __outside__ buildkit environment.

### When building the driver package my system says broken dependencies.

Check if you installed the dependency packages you [downloaded](#Dependency-packages-for-offline-compilation) is installed __inside__ buildkit environment.

### Other problems?

It is a convoluted wiki page, as is convoluted as the broadcom driver itself. Please feel free to reach us at channel #aosc on freenode or make an issue under [cth451's abbs tree](https://github.com/cthbleachbit/back-to-the-abbs) about broadcom drivers.
