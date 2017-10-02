<!-- TITLE: Buildbots -->
<!-- SUBTITLE: Buildbots that can be used by AOSC developers -->

# Buildbots

This page contains information about AOSC Buildbots.

AOSC buildbots are all connected to our central relay node (relay.aosc.io), and are allocated fixed port numbers. To be specific, ports are allocated as per the buildbot's architecture:

- **AMD64**: 2001 - 3000
- **MIPS**: 3001-4000
- **ARM**: 4001-5000
- **PowerPC**: 5001-6000
- **RISC-V**: 6001-7000

Between the relay and the buildbots, [Popub](https://github.com/m13253/popub) is used to forward your SSH port to our relay server. For usage of Popub, please read their README.

Each buildbot is allocated 2 contiguous ports under the range; the smaller one is for connection between your machine and the relay, and the larger one is for outside connections to the relay. For example, the AMD64 buildbot _W541_ is using 2345 and 2346, where _W541_ forwards its SSH port using `popub-local` to 2345 on the relay, and the relay exposes this port to the outside on port 2346.

You can log into these buildbots over SSH (by using `ssh -p <port_number> <username>@relay.aosc.io`). For usernames, passwords, and SSH public key exchange, you may contact the owners of buildbots first.

If you are willing to contribute your machine to AOSC, please make sure your machine have an usable AOSC Buildkit, and contact Junde Yhi \<lmy441900 at aosc dot xyz\>, providing:

- A name for your machine (optional, just for fun)
- The port number you want to expose on the relay server
- The Pre-Shared Key of Popub

## List of Buildbots

NOTE: 

- `port_numer - 1` is occupied by that machine. See information above.
- A machine that marked as **{Daily}** means this machine is also used daily by the owner (that means many programs unrelated to development, e.g. X11, will be running on this machine). Please make sure that you do not make OOM on those machine!

---

- **AMD64** (2001 - 3000)
	- **Yhi64 {Daily}**, owned by _Junde Yhi_, on **2048**
	- **W541**, owned by _Jeff Bai_, on **2346**
	- **EPSON-AOSC**, owned by _Zamir Sun_, on **2718 (e)**
	- **TS140**, owned by _Staph_, on **2729**
- **MIPS** (3001-4000)
	- **Gobson**, owned by _Jeff Bai_ (currently _Xiaoxing Ye_), on **3072**
	- **Godson**, owned by _Junde Yhi_, on **3141 (pi)**
- **ARM** (4001-5000)
	- **Tegra**, owned by _Jeff Bai_, on **4096**
	- **p64**, owned by _Icenowy Zheng_, on **4064**
- **PowerPC** (5001-6000)
- **RISC-V** (6001-7000)