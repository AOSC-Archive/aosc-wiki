<!-- TITLE: Buildbots -->
<!-- SUBTITLE: Buildbots that can be used by AOSC developers -->

# General Information

This page contains information about AOSC Buildbots.

AOSC buildbots are all connected to our central relay node (`relay.aosc.io`), and are allocated fixed port numbers. To be specific, ports are allocated as per the buildbot's architecture:

- **AMD64**: 2001 - 3000
- **MIPS**: 3001-4000
- **ARM**: 4001-5000
- **PowerPC**: 5001-6000
- **RISC-V**: 6001-7000

Between the relay and the buildbots, [Popub](https://github.com/m13253/popub) is used to forward your SSH port to our relay server. For usage of Popub, please read their README.

Each buildbot is allocated 2 contiguous ports under the range; the smaller one is for connection between your machine and the relay, and the larger one is for outside connections to the relay. For example, the AMD64 buildbot _Ry1800X_ is using 2332 and 2333, where _Ry1800X_ forwards its SSH port using `popub-local` to 2332 on the relay, and the relay exposes this port to the outside on port 2333.

You can log into these buildbots over SSH (by using `ssh -p <port_number> <username>@relay.aosc.io`). For usernames, passwords, and SSH public key exchange, you may contact the owners of buildbots first.

If you are willing to contribute your machine to AOSC, please make sure your machine have an usable AOSC Buildkit or a Ciel configuration, and contact Junde Yhi \<lmy441900 at aosc dot xyz\>, providing:

- A name for your machine (optional, just for fun).
- The port number you want to expose on the relay server.
- The Pre-Shared Key of Popub.

# List of Buildbots

NOTE: 

- `port_number - 1` is occupied by that machine. See information above.
- A machine that marked as **Daily** means this machine is also used daily by the owner (that means many programs unrelated to development, e.g. X11, will be running on this machine). Please make sure that you do not make OOM on those machine!

---

## **AMD64** (2001 - 3000)

|Name|Port|CPU|Memory|Maintainer|Note|
|---|---|---|---|---|---|
|**SITS**|2729(2SAT)|Intel Xeon CPU E3-1225 v3 @ 3.20GHz|16GiB|_S. aureus_||
|**Yhi64**|2048|||_Junde Yhi_|**Daily**|
|**Ry1800X**|2333|AMD Ryzen 7 1800X @ 3.60 - 4.10GHz|32GiB|_Mingcong Bai_||
|**EPSON-AOSC**|2718(e)|Intel Core 2 Duo T8100 @ 2.10GHz|4GiB|_Zamir Sun_|Available time: 8:00 - 21:30 UTC+8|

## **MIPS** (3001-4000)

|Name|Port|CPU|Memory|Maintainer|Note|
|---|---|---|---|---|---|
|**Gobson**|3072|||_Xiaoxing Ye_|Owned by _Mingcong Bai_|
|**Godson**|3141(pi)|Loongson 3A-2000C (R2) @ 1GHz (A1602)|8GiB|_Junde Yhi_||

## **ARM** (4001-5000)

|Name|Port|CPU|Memory|Maintainer|Note|
|---|---|---|---|---|---|
|**p64**|4064|Quad Core ARM Cortex-A53 @ 1.2GHz (Allwinner A64, Pine64 Plus)|2GiB|_Icenowy Zheng_||
|**Tegra**|4096|NVIDIA Tegra X1 @ 1.74GHz (NVIDIA Jetson TX1 Development Kit)|4GiB|_Mingcong Bai_||
|**Pine64**|4399|Quad Core ARM Cortex-A53 @ 1.2GHz (Allwinner A64, Pine64 Plus)|2GiB|_Mingcong Bai_||

## **PowerPC** (5001-6000)

|Name|Port|CPU|Memory|Maintainer|Note|
|---|---|---|---|---|---|
|**G5-AOSC**|5120|IBM PowerPC 970MP @ 2.5GHz (PowerMac G5, Quad, 2005)|8GiB|_Mingcong Bai_|Please avoid usage after 2 A.M. at America/Chicago.|

## **RISC-V** (6001-7000)