<!-- TITLE: Exceptions to the Monthly Update Cycle -->
<!-- SUBTITLE: A Suggested List of Packages to be Updated Immediately and Made Available in Stable Repositories -->

Rationale
==========
Since the monthly update cycle was introduced to AOSC OS in July of 2017, packages which represents feature, and non-bugfix/security updates should first have their build configurations pushed to the [staging](https://github.com/AOSC-Dev/aosc-os-abbs/tree/staging) of the [ABBS Tree](https://github.com/AOSC-Dev/aosc-os-abbs), uploaded to the [testing] repositories - and made available in the stable repository at the end of each monthly cycle after testing.

However, given the bugfix/security update may rely - and limited to - on backporting of patches, there are some other packages which could be...

- **Category 1:** Hard (or impossible in case of binary packages) to backport security/bugfix patches to, and frequently updated in mixture of feature and security updates. This category is most well represented by (larger) Web browsers, for example, Firefox and Chromium.
- **Category 2:** Heavily relied on frequent updates to remain functional. This category is most well represented by tools which reads online APIs/page contents for its basic functionality, for example, youtube-dl.
- **Category 3:** Essential to basic Internet access. This category is most well represented by network utilities which have to work around new blockades and constraints, for example, shadowsocks.

Exception List
========
The list below is a **concrete and specific** list of packages which could be considered as a part of the exception list - meaning that these packages' build configurations could be pushed to the [bugfix](https://github.com/AOSC-Dev/aosc-os-abbs/tree/bugfix) branch, as a part of the effort to fix "bugs" (non-functional) or "security issues", and made available in the stable repositories after internal testing - regardless of the version changes, and whether new features are to be introduced.


| Project Name | Package Name | Category |
| ------------------ | -------------------- | ------------ |
| Brave Browser | `brave-browser` | 1 |
| Chromium | `chromium` | 1 |
| Google Chrome | `google-chrome` | 1 |
| Mozilla Firefox  | `firefox` | 1 |
| Opera | `opera` | 1|
| Pale Moon | `palemoon` | 1 |
| Thunderbird | `thunderbird` | 1 |
| Vivaldi | `vivaldi` | 1 |
| Flash Player PepperAPI Plugin | `flashplayer-ppapi` | 1 |
| 