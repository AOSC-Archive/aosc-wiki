---
title: Autobuild3: Dependency Scan
description: 
published: true
date: 2020-05-27T05:02:38.486Z
tags: 
---

# Autobuild3 Automated Dependency Audit

Autobuild3 with version newer than <undefined> contains a security feature where it will scan the dependency tree of the project (if supported) for known vulnerabilities.

## Programming Language Specific Support

Currently Autobuild3 supports the audit for the following programming languages:

| Language | Required Package         | Bulletin Source(s)               |
|----------|--------------------------|----------------------------------|
| Node.js  | `node` (`npm`) or `yarn` | https://www.npmjs.com/advisories |
| Rust     | `cargo-audit`            | https://rustsec.org/advisories   |

## Limitations

- The automatic auditors can only identify a vulnerability that is currently publicly known
- The automatic auditors assume the current project will not modify any of its dependencies

## Recommended Remeditions

In this section, we will discuss possible remedies if `autobuild3` reported error regarding to the audit or found one or more vulnerabilities during the build process.

### Common Strategies

#### Upgrade the package to a newer upstream version

This strategy can usually resolve the problem without any issue. A responsible upstream should always keep their software free from any potential vulnerabilities.

#### Drop the package if unmaintained

If the upstream decided to cease the development of the software, and the software has been replaced or simply not used by anyone can be removed from the AOSC OS package tree(s).

### Programming Language Specific Solutions

If it's not possible or very difficult to follow the suggestions provided in the common strategies or you encountered an error during the audit process, you can consult the solutions provided in this section.

### Node.js Specific Solutions

#### Sympton: Error Message `The project does not contain any lock files. Unable to conduct the audit.`

This error message is shown if the source tree does not contain a lock file for the dependencies. This is very bad since the package management tools are free to pick any version they want within the acceptable range specified in the `package.json` file.

>Possible solutions:
>
>- Contact the upstream to include a lock file in the source tree (whether it's `package-lock.json` or `yarn.lock`, depending on their preferred tooling)
>- Include `npm install --package-lock-only` in the `autobuild/prepare` (create one if not exist)
{.is-info}

#### Sympton: Vulnerabilities Found: No Additional Descriptions

In this case, an automated tool might be able to fix the problem for you.

>Possible solutions:
>
>Make a copy of the lock file (see which lock file to use below) and then:
>
>- (If you see `yarn.lock` file in the source tree) Run `yarn audit fix`
>- (If you see `package-lock.json` file in the source tree) Run `npm audit fix`
>- (If you see both) This is a bad practice for upstream, you can pick one of the solutions above
>
>Then use `diff` to generate a patch.
{.is-info}

#### Sympton: Vulnerabilities Found: Manual Review Required

The message similar to the following is shown:

```
┌──────────────────────────────────────────────────────────────────────────────┐                                                                                             
│                                Manual Review                                 │                                                                                             
│            Some vulnerabilities require your attention to resolve            │
│                                                                              │                                                                                             
│         Visit https://go.npm.me/audit-guide for additional guidance          │      
└──────────────────────────────────────────────────────────────────────────────┘                                                                                 
```

Unfortunately, in this case you are out of luck.

This message indicates that:
- The fix is unavailble for some reason (e.g. not released, current fix not valid ...)
- The version included the fix is too far away from the version specified by the current project

>Possible solutions:
>- Contact the upstream about it
>- Have someone with decent understanding of the project or the programming language (at least) to come up with a patch
{.is-info}

### Rust Specific Solutions

#### Sympton: Error Message `No lock file found -- Dependency information unreliable. Unable to conduct audit.`

This error message is shown if the source tree does not contain a lock file for the dependencies. This is very bad since the package management tools are free to pick any version they want within the acceptable range specified in the `Cargo.toml` file.

>Possible solutions:
>
>- Contact the upstream to include a lock file in the source tree
>- Include `cargo update` in the `autobuild/prepare` (create one if not exist)
{.is-info}

#### Sympton: Vulnerabilities Found: No Additional Descriptions

In this case, an automated tool might be able to fix the problem for you.

>Possible solution:
>
>- Use the `cargo-audit` to fix the issue
>1. Make a copy of the `Cargo.lock` file and run `cargo audit fix`
>1. Then use `diff` to generate a patch.
{.is-info}

#### Sympton: Vulnerabilities Found: Manual Review Required

If not all the vulnerabilites are automatically fixable, or compilation errors encountered after the automated fix, then...

>Possible solutions:
>- Contact the upstream about it
>- Have someone with decent understanding of the project or the programming language (at least) to come up with a patch
{.is-info}
  