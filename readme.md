[GRUB iso multiboot]
====================

![preview](preview.png?raw=true "pick your poison")

Auto detect and create entries for .iso files in /iso/

Guide
-----

1. (as root) install grub into a usb stick
```
sudo mount /dev/sdX /mnt/multiboot
sudo grub-install --boot-directory=/mnt/multiboot/boot/ /dev/sdX # where X is your device
```

2. substitute the boot/ folder created by grub for this one
```
rsync -a grub-iso-multiboot/boot/ multiboot/boot/
```

Theme
-----

This was adapted from https://github.com/Dacha204/grub2-themes-Ettery

Working
-------

+ install-amd64-minimal-20170302.iso
+ void-live-x86_64-musl-20170220.iso
+ elementaryos-0.4-stable-amd64.20160921.iso
+ FreeBSD-11.0-RELEASE-amd64-bootonly.iso
+ bodhi-4.1.0-64.iso
+ Core-current.iso

NonWorking
----------

+ systemrescuecd-x86-4.9.3.iso
```
# grub failed to parse isolinux.cfg with:
error: kernel without a label.
```

+ TrueOS-Server-2017-02-22-x64-DVD.iso
```
takes a while to load the iso,
but then seems to hang (where vanilla BSD goes on), not sure
```
+ manjaro-xfce-17.0-stable-x86_64.iso
```
# grub failed to parse isolinux.cfg with:
error: syntax error.
error: Incorrect command.
error: syntax error.
```
+ Solus-2017.01.01.0-Budgie.iso
```
# loads isolinux.cfg ok
seems to hang after splash
```
+ GoboLinux-016-x86_64.iso
```
# Boots the kernel but fails to find the root inside the iso from kernel command line.
# memdisk would probably work here...
# You can still manually mount /Mount/CD-ROM if your iso is in a compatible disk
# but fat/vfat is not one of them. (so 1/2 working)
```

How
---

A modified autoiso.cfg (/boot/grub/scripts/autoiso.cfg) scans for .iso and .ISO
files in /iso/ and creates an entry if it knows how to boot it by:

+ via loopback.cfg if present (grub loopback)
+ isolinux.cfg, the majority of linux ISOs use this
+ custom detection for FreeBSD

Notes
-----

+ boot/ was made with a patched version of grub to add the iso as a parameter.
  check the patch @ linux-extra-variable.diff (The one liner patch is public domain)
+ the "patch" was made against grub-2.02_rc1. got from: https://www.gnu.org/software/grub/
+ memdisk is a file from syslinux project. got from: http://www.syslinux.org/

