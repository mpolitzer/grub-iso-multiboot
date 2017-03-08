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

NonWorking
----------

+ systemrescuecd-x86-4.9.3.iso
```
error: kernel without a label. # grub failed to parse isolinux.cfg
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

