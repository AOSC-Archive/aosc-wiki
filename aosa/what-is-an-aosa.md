<!-- TITLE: What is an AOSA? -->
<!-- SUBTITLE: A Quick Pre-face on AOSC OS Security Advisories -->

# AOSA?

The system of AOSC OS Security Advisories (AOSA) is introduced in 2017 to aid in the tracking of security vulnerabilities found in AOSC OS, and to help making our users aware of security updates available - which are often mixed-in with feature and other bug-fix updates.

# Formatting and Numbering

AOSAs follow a very simple (but organised) format with an aim to provide the immediate action suggestions, like in the example below:

```
 AOSA-2018-0008: Update WildMIDI to 0.4.2
|----|----|----|-------------------------|
```

- `AOSA-`, just to show that it's our advisory, as in contrary to `DSA-` from Debian, `ASA-` from Arch Linux, etc.
- `2018-`, to indicate the year in which the advisory was published.
- `0008`, a simple number counter to show how many advisories have been published before this particular one.
- `Update WildMIDI to 0.4.2`, developer suggestion for user action(s).

# Workflow for Security Advisories

The publication/announcement of a security advisory indicates the end of a security update cycle. Typically, this cycle could be summarised as the following workflow...

- Security contributor(s) subscribes to security mailing lists (like `oss-security`) and advisory mailing lists of upstream projects and other \*nix distributions