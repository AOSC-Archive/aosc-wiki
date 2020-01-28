<!-- TITLE: Exceptions to the Update Cycles -->
<!-- SUBTITLE: A Suggested List of Packages to be Updated Immediately and Made Available in Stable Repositories -->

# Rationale and Definition

Since the monthly update cycle was introduced to AOSC OS in July of 2017 (and later, seasonal update cycles introduced in July 2018), packages which represents feature, and non-bugfix/security updates should first have their build configurations pushed to the [staging](https://github.com/AOSC-Dev/aosc-os-abbs/tree/staging) of the [ABBS Tree](https://github.com/AOSC-Dev/aosc-os-abbs), uploaded to the [testing] repositories - and made available in the stable repository at the end of each seasonal cycle after testing.

However, given the bugfix/security update may rely - and limited to - on backporting of patches, there are some other packages which could be...

- **Category 1:** Hard (or impossible in case of binary packages) to backport security/bugfix patches to, and frequently updated in mixture of feature and security updates. This category is most well represented by (larger) Web browsers, for example, Firefox and Chromium.
- **Category 2:** Heavily relied on frequent updates to remain functional. This category is most well represented by tools which reads online APIs/page contents for its basic functionality, for example, youtube-dl.
- **Category 3:** Essential to basic Internet access in certain regions. This category is most well represented by network utilities which have to work around new blockades and constraints, for example, shadowsocks.
- **Category 4:** AOSC OS distribution-specific development toolkits, which should be the newest at all times.

# Exception List

The list below is a **concrete and specific** list of packages which could be considered as a part of the exception list - meaning that these packages' build configurations could be pushed to the [bugfix](https://github.com/AOSC-Dev/aosc-os-abbs/tree/bugfix) branch, as a part of the effort to fix "bugs" (non-functional) or "security issues", and made available in the stable repositories after internal testing - regardless of the version changes, and whether new features are to be introduced.


| Project Name | Package Name | Category |
| ------------------ | -------------------- | ------------ |
| Brave Browser | `brave-browser` | 1 |
| Chromium | `chromium` | 1 |
| Google Chrome | `google-chrome` | 1 |
| Intel Processor Microcode Data File | `intel-ucode` | 1 |
| Mozilla Firefox  | `firefox` | 1 |
| Min Browser | `min` | 1 |
| OpenJDK | `openjdk` | 1 |
| Opera | `opera` | 1 |
| Pale Moon | `palemoon` | 1 |
| SeaMonkey | `seamonkey` | 1 |
| Thunderbird | `thunderbird` | 1 |
| Vivaldi | `vivaldi` | 1 |
| Flash Player PepperAPI Plugin | `flashplayer-ppapi` | 1 |
| Baidu Cloud Client | `bcloud` | 2 |
| BiliDan | `bilidan` | 2 |
| ClamAV | `clamav` | 2 |
| Hardware ID Data | `hwdata` | 2 |
| Linux Kernels | `linux-kernel`, `linux-kernel-lts` | 2 |
| NVIDIA Proprietary Unix Drivers | `nvidia`, `nvidia+340`, `nvidia+390` | 2 |
| PyTZ | `pytz` | 2 |
| Telegram Desktop | `telegram-desktop` | 2 |
| Time Zone Data | `tzdata` | 2 |
| AOSC U-Boot Utilities | `u-boot-aosc-utils` | 2 |
| You-Get | `you-get` | 2 |
| Youtube-DL | `youtube-dl` | 2 |
| Gost | `gost` | 3 |
| I2pd | `i2pd` | 3 |
| KCPTUN | `kcptun` | 3 |
| OBFS Proxy | `obfsproxy` | 3 |
| OBFS4 Proxy | `obfs4proxy` | 3 |
| OpenSwan | `openswan` | 3 |
| OpenVPN | `openvpn` | 3 |
| PCAP-DNSProxy | `pcap-dnsproxy` | 3 |
| Shadowsocks | `shadowsocks` | 3 |
| Shadowsocks-LibEv | `shadowsocks-libev` | 3 |
| Shadowsocks-Qt5 | `shadowsocks-qt5` | 3 |
| Simple-OBFS | `simple-obfs` | 3 |
| StrongSwan | `strongswan` | 3 |
| Tor | `tor` | 3 |
| Trojan | `trojan` | 3 |
| V2Ray | `v2ray` | 3 |
| WireGuard | `wireguard` | 3 |
| CA Certificates | `ca-certs` | 3 |
| ACBS | `acbs` | 4 |
| Ciel | `ciel` | 4 |

# Extra Notes

- *Ask: Should non-security updates to packages found in the exception list be committed to the bugfix branch. e.g. Firefox 56.0.2?*
	- Re.: No, only if it fits within the five categories discussed above - however, it could be a valid argument that for example, if it prevents a user from accessing a certain website or it leads to crashes - in that case, an update to Firefox should be committed to the [bugfix](https://github.com/AOSC-Dev/aosc-os-abbs/tree/bugfix) branch and made available after internal testing. So that said, if the bugfix is minor and unlikely to be triggered, the update should still be limited to the [staging](https://github.com/AOSC-Dev/aosc-os-abbs/tree/staging) branch, and made available by the end of a monthly cycle.