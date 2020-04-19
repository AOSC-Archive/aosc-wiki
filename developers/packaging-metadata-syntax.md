---
title: Packaging Metadata Syntax
description: Reduced Bash syntax for describing packages
published: true
date: 2020-04-19T09:16:45.389Z
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

### Lines

Only variable definitions are allowed, eg. `a=b`.

### Syntax

* [Quoting](https://www.gnu.org/software/bash/manual/bash.html#Quoting)
  * Escape Character: `\newline`, `\"`
  * Single Quotes: `'a'`
  * Double Quotes: `"a"`
  * **Not Permitted**:
  	* ANSI-C Quoting: `$'a\nb'`
   	*	Locale-Specific Translation: `$"a"`
* [Comments](https://www.gnu.org/software/bash/manual/bash.html#Comments): `# comment`
* [Shell Expansions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions)
  * Shell Parameter Expansion:
  	* `${parameter:offset}`
    * `${parameter:offset:length}`
    * `${parameter#word}`
    * `${parameter##word}`
    * `${parameter%word}`
    * `${parameter%%word}`
    * `${parameter/pattern/string}`
    * `${parameter//pattern/string}`
  	* **Not OK**:
      * `${parameter:-word}`
      * `${parameter:=word}`
      * `${parameter:?word}`
      * `${parameter:+word}`
      * `${!prefix*}`
      * `${!prefix@}`
      * `${!name[@]}`
      * `${!name[*]}`
      * `${#parameter}`
      * `${parameter^pattern}`
      * `${parameter^^pattern}`
      * `${parameter,pattern}`
      * `${parameter,,pattern}`
      * `${parameter@operator}`
      * `${parameter/#pattern/string}`
  * **Not OK**:
  	* Brace Expansion: `a{d,c,b}e`
    * Tilde Expansion: `~/.config`
    * Command Substitution: `$(command)` or \`command\` (zip, x264+32, xl2tpd, chibi-scheme, uemacs, llvm, qt-5)
    * Arithmetic Expansion: `$(( expression ))`
    * Process Substitution: `<(list)` or `>(list)`
    * Filename Expansion

### Patterns

[Pattern Matching](https://www.gnu.org/software/bash/manual/bash.html#Pattern-Matching) is only used in `${parameter/pattern/string}`. Only `*` and `?` is supported.

