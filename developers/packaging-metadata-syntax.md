---
title: Packaging Metadata Syntax
description: Reduced Bash syntax for describing packages
published: true
date: 2020-04-20T01:15:19.592Z
tags: 
---

# Packaging Metadata Syntax
`spec` and `defines` files currently use Bash syntax to define package metadata, which is not easy to parse strictly according to the [Bash Reference Manual](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html). Therefore, we propose the use of a reduce set of Bash syntax to reduce parsing and transition cost, as packaging tools will be refactored in languages other than Bash.

## Example

```bash
PKGVER=8.2
PKGDEP="x11-lib libdrm expat systemd elfutils libvdpau nettle \
        libva wayland s2tc lm-sensors libglvnd llvm-runtime libclc"
MESON_AFTER="-Ddri-drivers-path=/usr/lib/xorg/modules/dri \
             -Db_ndebug=true" 
MESON_AFTER__AMD64=" \
             ${MESON_AFTER__X86} \
             -Dlibunwind=true"
```


## Permitted Bash Syntax

* Variable declarations are allowed, eg. `a=b`.
* [Comments](https://www.gnu.org/software/bash/manual/bash.html#Comments): `# comment`
* [Quoting](https://www.gnu.org/software/bash/manual/bash.html#Quoting)
  * Escape Characters: `\newline`, `\"`
  * Single Quotes: `'a'`
  * Double Quotes: `"a"`
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


## Prohibited Bash Syntax

* [Quoting](https://www.gnu.org/software/bash/manual/bash.html#Quoting)
	* ANSI-C Quoting: `$'a\nb'`
	*	Locale-Specific Translation: `$"a"`
* [Shell Expansions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions)
   Brace Expansion: `a{d,c,b}e`
  * Tilde Expansion: `~/.config`
  * Command Substitution: `$(command)` or \`command\` (zip, x264+32, xl2tpd, chibi-scheme, uemacs, llvm, qt-5)
  * Arithmetic Expansion: `$(( expression ))`
  * Process Substitution: `<(list)` or `>(list)`
  * Filename Expansion
  * Shell Parameter Expansion:
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


### Patterns

[Pattern Matching](https://www.gnu.org/software/bash/manual/bash.html#Pattern-Matching) is only used in `${parameter/pattern/string}`. Only `*` and `?` is supported.

