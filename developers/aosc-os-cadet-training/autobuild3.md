<!-- TITLE: Autobuild3 -->
<!-- SUBTITLE: A Multi-Backend Packaging Toolkit -->

# What is Autobuild3?

Autobuild3 is a toolkit that automatically compiles and packages software from a *pre-processed* (i.e. extracted) source. Autobuild3 relies on a set of configurations and an `autobuild/` directory containing files and directories that are called "Autobuild3 Manifests". 

Autobuild3 manifests constitutes a powerful set of files that takes care of package metadata (package name, version, revision, dependencies, etc.) used for both the build-time and run-time, specifies the build sequence and behaviours, and the resulting package(s). There are a lot of files and switches contained in these manifests, and as such, there are a lot of information to be presented.

In the interest of readability, this guidebook will be divided into several sections:

- The `autobuild/defines` file.
- Build scripts, scriptlets, and extra files.
- Running and Utilising Autobuild3.

# The `autobuild/defines` File