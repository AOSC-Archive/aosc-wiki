<!-- TITLE: Is AOSC OS Right for Me? -->
<!-- SUBTITLE:  -->


## What is AOSC OS, anyways?

Good question, we have been exploring this question for a long time now. But through the seven years of its development, it has become increasingly clear that we are building this Linux distribution with several subtle features that we find essential. Though first, let's return to the basics...

AOSC OS in its recent years (since 2014), is an independently built and maintained distribution, meaning that it is in contrast to examples like Ubuntu, which was built upon packages from Debian - AOSC OS developers sources softwares provided in the distribution independently and builds them with configurations maintained by the same developers. Not that it is an overtly noticable feature, but it allows for our own, say, "creative freedom" when it comes to making the distribution into what we really wanted it to be - according to our philosophies and the suggestions from our community.

That said, AOSC OS is only one of the several thousand Linux distributions out there, it builds upon the powerful Linux Kernel and the applications made by many around the world. We understand that it could be difficult to determine a Linux distribution to go with - whether it be the first time, or the 100th time - the goal of this Wiki page is to offer a multi-aspectual description of AOSC OS to help you in the decision process.

## Philosophies of AOSC OS

AOSC OS is built in a multi-year effort by a collective of volunteers, who possess certain degrees of knowledge about the making of a Linux distribution. AOSC OS started its life as an independent distribution not since its first day of existence, but after three years of experimentations with AnthonOS as derivative distributions built on openSUSE and Debian. In 2014, we have determined to build AOSC OS independently to gain full control on the design and maintaining of packages and their updates.

Through the years following we have discovered that AOSC OS has been built upon the following goals and philosophies:

- There should be no package splitting (like `libsndfile` splitting into a runtime package `libsndfile` and a development package `libsndfile-devel`) unless absolutely necessary or reasonably justified.
- There should be minimal changes to the packages themselves unless its a bug-fix unmerged at the upstream, or a necessary change to make it work properly in AOSC OS.
- There should be little variation between users of different languages, with particular attention to CJK users - specifically, in font rendering and application display/layouts.
- While open source and free software makes up the majority of the software repository, proprietary and commercial software should not be omitted unless their licensing suggests otherwise.
- The distribution should attain minimal branding or distribution specific information, regarding the distribution itself as a tool designed according to the users' needs, not to their "brand recognition".
- There is no point providing an update if it breaks the user experience - specifically, when it renders basic functionalities unusable. However, software updates should be provided as soon as possible.
- AOSC OS should provide similar (but reasonably reduced or added) functionalities across various architectural ports, while regarding them as personal devices capable of desktop and/or server usage.

## How Does AOSC OS Compare?

Agreeing to these goals and philosophies, AOSC OS is manifested very similarly to many semi-rolling distributions, but with our own features, here's a table to compare several known distributions against ourselves:

|    | AOSC OS | Arch Linux | Fedora | Debian |
|----|---------|------------|--------|--------|
| Always Splits Packages | No | No | Yes | Yes |
| Update Model | Semi-Rolling | Rolling | Releases | Releases |
| Supports non-x86 Architectures | Yes | No | Yes | Yes |
| Provides non-free Software Officially | Yes | Yes | No | Yes |
