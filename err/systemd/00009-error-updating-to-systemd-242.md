<!-- TITLE: ERR-SYS-00009: Error Updating To Systemd 242 -->
<!-- SUBTITLE: Obsolete systemd services in memory cause problems -->

# Error updating to `systemd` 242

## Symptoms

AOSC has identified an issue with a systemd update. While installing update packages including `systemd` 242 or later versions, you may get an error message stating:

```shell
(Reading database ... 89562 files and directories currently installed.)
Preparing to unpack .../apt_1%3a1.7.0-3_amd64.deb ...
Unpacking apt (1:1.7.0-3) over (1:1.7.0-2) ...
Setting up apt (1:1.7.0-3) ...
Failed to enable unit: Access denied
dpkg: error processing package apt (--configure):
 installed apt package post-installation script subprocess returned error exit status 1
Errors were encountered while processing:
 apt
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

## Affected platforms

- AOSC OS Core 5 and previous versions

## Cause

After updating `systemd`, some packages updated later still calls the obsolete systemd services in memory, causing unrecoverable problems.

## Workaround

To resolve this problem, follow these steps:

1. `apt full-upgrade systemd=1:241 ppp=2.4.7-3`
2. `apt full-upgrade`

## Next steps

We are working on a resolution and will provide an update in an upcoming release.
