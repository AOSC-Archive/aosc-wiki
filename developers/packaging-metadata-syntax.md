---
title: Packaging Metadata Syntax
description: Reduced Bash syntax for describing packages
published: true
date: 2020-04-19T07:55:43.713Z
tags: 
---

# Packaging Metadata Syntax
`spec` and `defines` files currently use bash scripts for describing package metadata. The bash script syntax is not easy to parse strictly according to the bash manual. Therefore, we propose a reduced bash syntax for describing package metadata. The goal is to reduce parsing and transition cost.

## Example

```bash
VER=8.2
PKGDEP="x11-lib libdrm expat systemd elfutils libvdpau nettle \
        libva wayland s2tc lm-sensors libglvnd llvm-runtime libclc"
MESON_AFTER="-Ddri-drivers-path=/usr/lib/xorg/modules/dri \
             -Db_ndebug=true" 
MESON_AFTER__AMD64=" \
             ${MESON_AFTER__X86} \
             -Dlibunwind=true"
```


## Permitted bash syntax                   

* [Quoting](https://www.gnu.org/software/bash/manual/bash.html#Quoting)
	*	**OK**
  	* Escape Character: `\newline`, `\"`
    * Single Quotes: `'a'`
    * Double Quotes: `"a"`
  * **Not OK**
  	* ANSI-C Quoting: `$'a\nb'`
   	*	Locale-Specific Translation: `$"a"`
* [Comments](https://www.gnu.org/software/bash/manual/bash.html#Comments): `# comment`
* [Shell Expansions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions)
  * **OK**:
  	* Brace Expansion:
  * **Not OK**:
  	* Brace Expansion: `~/.config`
    * 

