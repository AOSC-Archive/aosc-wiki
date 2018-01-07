<!-- TITLE: ERR-SYS-00001: Disappearing Windows with KDE/Plasma Desktop using Intel DDX -->
<!-- SUBTITLE: Intel X11/XFree86 Driver Issue and an Workaround -->

Summary
===========

When using Plasma 5.11.5 and KDE Frameworks 5.41.0 (and earlier) with a Intel X11/XFree86 driver (`xf86-video-intel`), some windows - specifically, those utillising GNOME/GTK+ 3 Client Side Decoration (or VTE 2.90?) - could fail to display on the screen. A taskbar entry of the window could be found, and a window preview should also display correctly. The window is not displaying, neither does it respond to any window management event: no cursor change when moving to the edge of the offending windows. Here are two instances of the issue:

- Tilix
- GNOME Terminal

Possible Cause
===========

Not yet identified.

Fixed Version
===========

None. However, a workaround has been identified - and this issue should only happen on a computer using Intel graphics (GMA, HD, UHD, etc.). To workaround this issue, you should switch to the "Modesetting" X11/XFree86 driver. To switch to this driver, simply add and apply the following configuration file with the instructions below:

Create a Custom X11 Configuration
------------------------------


