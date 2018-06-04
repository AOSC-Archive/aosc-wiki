<!-- TITLE: Information/ArchSpecs -->
<!-- SUBTITLE: AOSC OS Architecture Naming Schemes and Specifications -->

This page contains information of all architectures supported by AOSC OS, and their architecture-specific requirements, notes, and toolchain triplets. The architectures listed below are in AOSC-style short names.

# Currently Supported

## x86_64 (amd64)

For systems with 64-bit x86 processors.

- Triplet: `x86_64-aosc-linux-gnu`

### x86

For systems with 32-bit x86 processors. 32-Bit x86 architecture support is implemented as **32Subsystem**. 

- Triplet: `i686-aosc-linux-gnu`
- Note: Requires Pentium Pro (i686) or higher.

## armel

For systems with 32-bit little endian ARMv7-A+ processors.

- Triplet: `armv7a-aosc-linux-gnueabihf`
- Note: Requires NEON FPU support; Hard Float ABI.

## arm64

For systems with 64-bit little endian ARMv8-A+ processors (AArch64).

- Triplet: `aarch64-aosc-linux-gnu`

## mipsel

For systems with 32-bit little endian MIPS processors, O32 ABI.

- Triplet: `mipsel-aosc-linux-gnu`
- Note: Requires MIPS-II ISA or higher.

## mips64el

For systems with 64-bit little endian MIPS processors, N64 ABI.

- Triplet: `mips64el-aosc-linux-gnu`
- Note: Requires MIPS64r2 ISA or higher, optimized for Loongson 3A.

## powerpc

For systems with 32-bit big endian PowerPC processors.

- Triplet: `powerpc-aosc-linux-gnu`

## ppc64

For systems with 64-bit big endian PowerPC processors.

- Triplet: `powerpc64-aosc-linux-gnu`
- Note: Requires AltiVec support.


## rv64g

For systems with 64-bit RISC-V processors, `gc` variant.

- Triplet: `riscv64-aosc-linux-gnu`