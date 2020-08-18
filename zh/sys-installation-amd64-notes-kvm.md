---
title: Installation/AMD64/KVM (简体中文)
description: 在 KVM 安装 AOSC OS
published: true
date: 2020-08-18T15:11:46.745Z
tags: sys-installation
editor: markdown
---

AOSC OS installation on Qemu/KVM is the same as installing on a regular AMD64/x86_64 system, this section is intended to aid you with configuring the virtual machine, and un-tar-ing the tarballs from outside of the virtual machine.

These two steps below replaces the "Preparing an Installation Environment", "Preparing partitions", and "Un-tar!" sections in the [regular installation guide](/en/sys-installation-amd64).

# 注意

- 所有以 `# ` 开头的命令都需要你使用 `root` 来运行。

# 准备 VM 硬盘映像

Create an empty hard disk image called `aosc.img` with the size of `20GiB`, you will need at least 8GB to use AOSC OS for any practical functions.

```
# qemu-img create -f raw aosc.img 20G
```

为 `aosc.img` 建立分区：

```
# fdisk aosc.img

Welcome to fdisk (util-linux 2.28.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0xd683cfec.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-41943039, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-41943039, default 41943039):

Created a new partition 1 of type 'Linux' and of size 20 GiB.

Command (m for help): w
The partition table has been altered.
Syncing disks.
```

打印目前的分区表：

```
$ fdisk -l aosc.img
Disk aosc.img: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd683cfec

Device     Boot Start      End  Sectors Size Id Type
aosc.img1        2048 41943039 41940992  20G 83 Linux
```

创建一个回环设备，例如 /dev/loop0。请注意下面参数中 offset 是 `start * sectorsize`，而 sizelimit 是 `sectors * sectorsize`：

```
# losetup --offset $((2048*512)) --sizelimit $((512*41940992)) --show --find aosc.img /dev/loop0
```

将其格式化：

```
# mkfs.ext4 /dev/loop0
```

挂载你的回环设备，不妨将其挂载到 `/mnt`：

```
# mount /dev/loop0 /mnt
```

# 解压 Tarball

是时候解压你已经下载好的 AOSC OS 系统 Tarball 了！

```
$ cd /mnt
# tar pxvf /path/to/tarball.tar.xz
```

现在你可以卸载你的映像文件了：

```
# umount ${MOUNT}
# losetup -d /dev/loop0
```

# 配置引导器

Here comes the most interesting part. Boot configuration is needed for the un-tar-ed system to boot and initialize.

This part require you to have a working VM. To chroot on your physical system simply won't work as expected. Before continue with installing GRUB as described in the [regular installation guide](/en/sys-installation-amd64), create a VM with the prepared hard disk file, and boot the VM from a LiveCD.

Now you may continue the installation in the VM with the Live system.
