<!-- TITLE: Installation/ARM* -->
<!-- SUBTITLE: Installing AOSC OS on ARMv7/AArch64 Devices -->

# ARMv7/AArch64 Installation

There really isn't any set way to install AOSC OS on various kinds of ARM devices, this page serves as a index to guide you to various other guides for specific devices.

Please refer to our ARM Board Support Package [Wiki](https://github.com/AOSC-Dev/aosc-os-arm-bsps/wiki) for more information on devices that AOSC OS currently supports, and their installation guides.

## Tarballs?

All tarballs provided in the [Download](https://aosc.io/os-download/) page comes with a unconfigured mainline Linux Kernel, it will not work on most devices without extra configuration, we use the [aosc-mkrawimg](https://github.com/AOSC-Dev/aosc-mkrawimg) to create device-specific images from them - using shell-based recipes.

Device-specific RAW images for MicroSD cards and eMMC storage could be found in the same location.

## Variants and System Requirements

Currently, the following variants of AOSC OS are available for ARMv7/AArch64 devices/systems. Additional consideration is needed for whether your device is capable for a specific variant, please consult the [ARM system requirements](/users/installation/arm-notes-sysreq) page for more information:

### Bootable

- Base
- GNOME
- MATE
- XFCE
- LXDE
- i3 Window Manager

### Non-bootable

- Container
- BuildKit

We are not going to discuss the deployment of Container and BuildKit in this guide, please check for the guide in [AOSC Cadet Training](https://github.com/AOSC-Dev/aosc-os-abbs/wiki).
