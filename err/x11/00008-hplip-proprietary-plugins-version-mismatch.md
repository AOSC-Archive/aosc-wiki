<!-- TITLE: ERR-SYS-00008: CUPS "Filter Failed" with Printers Using the HPLIP Driver -->
<!-- SUBTITLE: HPLIP Proprietary Plugin Version Mismatch and Its Consequence -->

# Summary

Some Hewlett-Packard (HP) printers may require using the drivers provided with the HP Linux Image and Printing (HPLIP, `hplip`) software package - and some even requires proprietary plugins to function (say, with the HP LaserJet P1102w where this issues was first discovered). Printers that require the proprietary plugins may fail to function when the `hplip` package is updated, with the CUPS daemon returning a "filter failed" error.

# Possible Cause



# Workaround

Unfortunately, this issue is impossible to address via future package updates - as system updates will not alter user-defined configurations. To resolve this issue, you will need to use appropriate configuration utilities to specify the "correct" font name - "Noto Sans Mono" counterparts of the former "Noto Mono" family.
