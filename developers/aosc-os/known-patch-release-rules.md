<!-- TITLE: Known Patch Release Rules -->
<!-- SUBTITLE: Useful list of release rules that defines packages that could be updated in the Stable channel -->

# Description

The AOSC OS "Stable" channel under the new *TODO: SEASONAL UPDATE MODEL* requires that only packages with *known* patch-level release rules could be updated in this channel, for example, when GNOME Terminal (`gnome-terminal`) is to be update from `3.30.0` to `3.30.1`, this is permitted, as we know that this is a patch release as [dictated](https://developer.gnome.org/programming-guidelines/stable/versioning.html.en#stable-unstable-versions) by GNOME - on the contrary, an update from `3.30.0` to `3.31.0` (stable to unstable) or even from `3.30.0` to `3.32.0` (stable to next-stable) are not acceptable.

Presented below is a table that defines all known patch release rules for upstream projects, if it isn't found here, do not push any updates to the "stable" channel - or these updates will be reverted. However, do note that all packages listed in the list of [Exceptions](https://wiki.aosc.io/developers/aosc-os/cycle-exceptions) could be updated in the "Stable" channel without consulting the table below.

# Table of Known Update Rules

| Project or Package Name | Rule Description |
| -------------------------------------------- | ----------------------------- |
| GNOME Desktop and Components | If projects have versions, say, `x.y.z`  or even at times `x.y.z.n`, then any updates to `z` or `n` could be regarded as patch-level releases. `y` must be even numbers. [Source](https://developer.gnome.org/programming-guidelines/stable/versioning.html.en#stable-unstable-versions). |
| KDE Frameworks | If projects have versions, say, `x.y.z`  or even at times `x.y.z.n`, then any updates to `z` or `n` could be regarded as patch-level releases. `y` may be even or odd numbers. [Source](https://community.kde.org/Guidelines_and_HOWTOs/Application_Versioning). |
| Plasma Desktop and Components | If projects have versions, say, `x.y.z`  or even at times `x.y.z.n`, then any updates to `z` or `n` could be regarded as patch-level releases. `y` may be even or odd numbers. [Source](https://community.kde.org/Guidelines_and_HOWTOs/Application_Versioning). |
| KDE Applications | If projects have versions, say, `x.y.z`  or even at times `x.y.z.n`, then any updates to `z` or `n` could be regarded as patch-level releases. `y` may be even or odd numbers. [Source](https://community.kde.org/Guidelines_and_HOWTOs/Application_Versioning). |
| PostgreSQL | Versions usually formatted as `x.y`, where `y` is considered the patch-level version. Updates, say, from `10.4` to `10.5` is acceptable as a patch-level update. [Source](https://www.postgresql.org/support/versioning/). |
| OpenSSL | Versions usually formatted as `x.y.z(n)`, where `(n)` is usually a Latin letter indicates the patch-level version. Updates, say, from `1.0.2o` to `1.0.2p` is acceptable as a patch-level update. [Source](https://wiki.openssl.org/index.php/Versioning) |
| Bind | Versions usually formatted as `x.y.z-P(n)`, where `(n)` is an integer, which represents the patch-level. Additionally, only when `y` is an even number should the version be considered as stable. [Source](https://www.isc.org/downloads/software-support-policy/). |
| Semantic Versioning, in general | If projects have versions, say, `x.y.z`  or even at times `x.y.z.n`, then any updates to `z` or `n` could be regarded as patch-level releases. may be even or odd numbers, unless otherwise specified, like in the example of GNOME. [Source](https://semver.org/). |