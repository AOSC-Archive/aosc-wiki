<!-- TITLE: LiveKit -->
<!-- SUBTITLE: The design of Livekit -->

# LiveKit
LiveKit is a live environment for installing and maintaining AOSC OS or other Linux distributions.

## Packages
- LXDE
- X11 drivers for display and basic input (libinput)
- Browsers
	- Pale Moon
	- w3m, lynx
- NetworkManager (with Applets)
- Data recovery
	- ddrescue, ddrescueview
	- testdisk
	- gparted
- Default wallpaper with reduced quality
- Remove all documentation from /usr/share/doc
- Accessibility (Orca + Onboard)
- Fonts inclusion as specified in SMALL_ARCH
- Utilities
	- System admin
		- dash, bash, bash-completion
		- bc, units (calculators)
		- screen, tmux, byobu
		- pv, progress
		- busybox
		- util-linux
		- genfstab
		- fbterm
	- Automation
		- grep, sed, awk
		- python(2,3), perl, tcl
		- parallel
		- diffutils, patch
		- expect
	- Connectivity
		- ssh, mosh
		- wget, curl, aria2, ftp
		- privoxy, shadowsocks
		- darkhttpd
	- File format
		- bsdtar, zip, cpio, xz, lbzip2, pigz, zstd, lzo, lz4
		- sqlite
		- file, chardet, dos2unix
		- gnupg
		- dmg2img
	- Editor
		- ed
		- nano, vim
	- Monitor
		- htop, iotop, dstat
		- bmon, nethogs
	- File System
		- e2fsprogs, xfsprogs, btrfs-progs, ntfs-3g, f2fs-tools
		- cryptsetup, lvm2
		- fuse, fuse-exfat
		- fatattr
		- convmv
		- duff
		- parted, gptfdisk
	- Backup
		- rsync
		- unison
	- Hardware
		- dmidecode
		- crazydiskinfo
		- f3