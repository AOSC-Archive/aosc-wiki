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

- **AMD64** (2001 - 3000)
- **MIPS** (3001-4000)
- **ARM** (4001-5000)
- **PowerPC** (5001-6000)
- **RISC-V** (6001-7000)