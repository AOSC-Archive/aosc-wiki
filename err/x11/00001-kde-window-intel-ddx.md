<!-- TITLE: ERR-SYS-00001: Disappearing Windows with KDE/Plasma Desktop using Intel DDX -->
<!-- SUBTITLE: Intel X11/XFree86 Driver Issue and an Workaround -->

Issue Summary
===========

As of Plasma 5.11.5 and KDE Frameworks 5.41.0, some windows - specifically, those utillising GNOME/GTK+ 3 Client Side Decoration (or VTE 2.90?) - could fail to display on the screen. A taskbar entry of the window could be found, and a window preview should also display correctly. The window is not displaying, neither does it respond to any window management event: no cursor change when moving to the edge of the offending windows. Here are two instances of the issue:

- Tilix
- GNOME Terminal

