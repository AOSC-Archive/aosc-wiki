<!-- TITLE: AOSC OS Package Styling Manual -->
<!-- SUBTITLE: Comprehensive (and updated) Styling Manual for AOSC OS Packagers -->

# Introduction and Rationale

The [AOSC OS ABBS Tree](https://github.com/AOSC-Dev/aosc-os-abbs/) is now home to over 5000 (and counting) packages, which are maintained by over 20 (historical and current) packagers. With more than 5 years of history behind some packages, issues began to emerge in the following fashion:

- Packages lack dependencies.
- Insecure (unquoted) variables in build scripts.
- Hard-to-read build scripts, with little comments or indication of progression.
- Undescriptive package description(s) (`$PKGDES`).
- Non-standard package section(s) (`$PKGSEC`).
- ...

Without a comprehensive guildeline for packaging work in the future, these issues will not go away - rather, the list will grow longer and further degrade package qualities in our distribution. This Styling Manual seeks to provide sectioned and targetted sets of guidelines for fixing existing and new packages.

# Metadata

In the current implementation of [ACBS](https://github.com/AOSC-Dev/acbs) (AOSC CI Build Service), three categories of variables are defined in a file named `spec` - these variables will be discussed below.

## Versioning Variables

Versioning variable define the package's version and revision levels. 

### VER=

The `VER=`, or `$VER` variable defines the main version of the resulting package. When packaging for AOSC OS, packagers should take note of the following requirements:

- 

# Dependencies

# Scripting

# Package Structure