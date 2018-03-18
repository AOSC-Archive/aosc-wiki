<!-- TITLE: How To Mirror the Community Repository -->
<!-- SUBTITLE: A Quick Summary of How to Create a Mirror of AOSC's Repo -->

Thank you for your interest in mirroring AOSC's Repo.

Believe it or not, AOSC has been around for quite some time now. We do want more people to know about us, and those who we are grateful to. AOSC will never take its shape today without the support from our lovely community and our generous sponsors.

Up to now, we have many mirrors (mainly in China), and we do need more mirrors in other areas (especially in U.S.). If you are interested and able to help us, please read the following information.

# Repo Contents
What is included in the repo? Generally everything about AOSC, and most are of AOSC OS (packages, system images). Some documentations, like *Free Software Localization Guide for Chinese (China) *, are also saved here.

# Repo Size
Up to now (March, 2018), the repo is taking ~480GB of disk. You may expect that the repo will grow to around 600GB or more.

# How to Mirror
If you have enough hard disk space, please do rsync from our main repo: rsync://repo.aosc.io/anthon/ . 

If you have trouble resolving the DNS, please use the IP addresses: 163.22.17.40 (IPv4), 2001:e10:6840:17::40 (IPv6).

Please be noted that the repo is located in Taiwan, China, and the network connection could be VERY slow if you are in Mainland, China.

You may also initialize the mirror by doing rsync from any one of the mirrors providing rsync, like [USTCLUG](https://mirrors.ustc.edu.cn/) or [Tuna](https://mirrors.tuna.tsinghua.edu.cn/).

After you have finished the initialization, please tell us by sending an email to our mailing list: mirrors@lists.aosc.io, including your name (which will be listed on sponsors) and mirror URL.

When providing public HTTP service, we highly encourage you to use HTTPS.

# Sponsors
The Main Repo is kindly sponsored by [OSSPlanet](http://ossplanet.net/).

For other sponsors, please refer to https://aosc.io/mirror-status (list of mirrors) and https://aosc.io/about (all general sponsors).

# Question?
Feel free to send your question to the mailing list mirrors@lists.aosc.io .
