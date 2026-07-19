# ParticulaOS

ParticulaOS is a personal, immutable Fedora image built with [mkosi](https://github.com/systemd/mkosi)
and systemd's native tooling.

## Installation

1. Download the latest disk image from this repository's
   [Releases](../../releases/latest) page.
2. Decompress it and burn it to a USB drive:
   ```sh
   xz -d ParticulaOS_<version>_x86-64.raw.xz
   dd if=ParticulaOS_<version>_x86-64.raw of=/dev/<usb> bs=4M status=progress oflag=sync
   ```
3. Boot the USB drive on the target machine and select the **Live** UKI profile.
4. Once in the root shell, install to the target drive:
   ```sh
   systemd-repart --dry-run=no --empty=force --defer-partitions=swap,root,home /dev/<drive>
   ```
5. Reboot into the target drive (not the USB), using the regular boot entry, not
   Live.

## GNOME core apps via Flatpak

Only a minimal set of GNOME apps ships in the base image. Install the rest of the
usual GNOME core app suite from Flathub:

```sh
flatpak install flathub org.gnome.{baobab,Calculator,Calendar,Characters,clocks,Connections,Contacts,Decibels,Epiphany,font-viewer,Logs,Loupe,Maps,Music,NautilusPreviewer,Papers,Showtime,SimpleScan,Snapshot,TextEditor,Weather}
```

## Configuring systemd-homed after installation

After installing ParticulaOS and logging into your systemd-homed managed user, run
the following to configure systemd-homed for the best experience:

```sh
homectl update <user> --auto-resize-mode=off --disk-size=max --luks-discard=on
```

Disabling the auto resize mode avoids slow system boot and shutdown. Enabling
LUKS discard makes sure the home directory doesn't become inaccessible because
systemd-homed is unable to resize it.

## Credits

Based on [ParticleOS](https://github.com/systemd/particleos) by the systemd
project, licensed under LGPL-2.1-or-later. The license is preserved unmodified
in this repository. Most of the underlying design comes directly from upstream.
