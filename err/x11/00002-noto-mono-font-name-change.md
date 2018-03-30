<!-- TITLE: ERR-SYS-00001: Noto Mono Font Name Changes with Recent Font Package Update -->
<!-- SUBTITLE: Noto Mono Font Name Changes and Possible Impacts -->

# Summary

With the recent `noto-font` package update, version `1:20180324` and above, the "Noto Mono" (monospace) font family has now gotten a new name "Noto Sans Mono":

- Noto Mono Blk → Noto Sans Mono Blk
- Noto Mono Bold → Noto Sans Mono Bold
- Noto Sans Mono CJK JP Bold
- Noto Sans Mono CJK JP Regular
- Noto Sans Mono CJK KR Bold
- Noto Sans Mono CJK KR Regular
- Noto Sans Mono CJK SC Bold
- Noto Sans Mono CJK SC Regular
- Noto Sans Mono CJK TC Bold
- Noto Sans Mono CJK TC Regular
- Noto Sans Mono Cond
- Noto Sans Mono Cond Blk
- Noto Sans Mono Cond ExtBd
- Noto Sans Mono Cond ExtLt
- Noto Sans Mono Cond Light
- Noto Sans Mono Cond Med
- Noto Sans Mono Cond SemBd
- Noto Sans Mono Cond Thin
- Noto Sans Mono ExtBd
- Noto Sans Mono ExtCond
- Noto Sans Mono ExtCond Blk
- Noto Sans Mono ExtCond ExtBd
- Noto Sans Mono ExtCond ExtLt
- Noto Sans Mono ExtCond Light
- Noto Sans Mono ExtCond Med
- Noto Sans Mono ExtCond SemBd
- Noto Sans Mono ExtCond Thin
- Noto Sans Mono ExtLt
- Noto Sans Mono Light
- Noto Sans Mono Med
- Noto Sans Mono Regular
- Noto Sans Mono SemBd
- Noto Sans Mono SemCond
- Noto Sans Mono SemCond Blk
- Noto Sans Mono SemCond ExtBd
- Noto Sans Mono SemCond ExtLt
- Noto Sans Mono SemCond Light
- Noto Sans Mono SemCond Med
- Noto Sans Mono SemCond SemBd
- Noto Sans Mono SemCond Thin
- Noto Sans Mono Thin

# Possible Cause

Not yet identified.

# Fixed Version

## System Releases

Tarball system releases dated 20180126 or later will no longer include the Intel DDX (`xf86-video-intel`), as we removed `xf86-video-intel` as part of the `x11-base` metapackage. If your system still has this package installed, upon system update, you should be prompted that `xf86-video-intel` is no longer needed - follow the instruction on screen to remove this package automatically.

## Workaround

A workaround has been identified - and this issue should only happen on a computer using Intel graphics (GMA, HD, UHD, etc.). To workaround this issue, you should switch to the "Modesetting" X11/XFree86 driver. To switch to this driver, simply add and apply the following configuration file with the instructions below:

## Create a Custom X11 Configuration

Using your favourite editor (assuming `nano` here), create a custom X11 configuration file:

`sudo nano /etc/X11/xorg.conf.d/20-modesettings.conf`

Input in the following content:

```
Section "Device"
    Identifier  "Intel Graphics"
    Driver      "modesetting"
    Option      "AccelMethod"    "glamor"
    Option      "DRI"            "3"
EndSection
```

And finally, `Ctrl-X` then `Y` to save the file.

## Restart Your X11 Session

Using the following command:

`sudo systemctl restart display-manager`.

And you should be switched to the "Modesetting" driver, and that windows should be displaying correctly.