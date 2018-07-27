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
	- Automation
		- grep, sed, awk
		- python(2,3), perl, tcl
		- parallel
		- diffutils, patch
	- Connectivity
		- ssh, mosh
		- rsync
		- wget, curl, aria2, ftp
		- privoxy, shadowsocks
	- File format
		- bsdtar, zip, xz, lbzip2, pigz, zstd, lzo, lz4
		- sqlite
		- file, chardet
	- Editor
		- nano, vim
	- Monitor
		- htop, iotop
		- bmon, nethogs
	- File System
		- e2fsprogs, xfsprogs, btrfs-progs, ntfs-3g
		- fuse, fuse-exfat
		- fatattr