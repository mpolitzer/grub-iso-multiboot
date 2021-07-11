# [GRUB iso multiboot]

+ Auto detect and create entries for .iso files in /iso/
+ Preserves its boot options!
+ Small and lean, few entries lots of images
+ new iso, same script. No need to tweak around (ymmv).

![preview with vimix](doc/vimix.png?raw=true "vimix")
![preview with starfield](doc/starfield.png?raw=true "starfield")

If your pendrive is already formated to FAT, you only need to change its label
to GIM and then skip step `1`. To use different name changes to the scripts are
required, more on this later (step `3`).

## 1. Installation Guide (clean disk)

### 1.1 Create a partition table:
```
# fdisk /dev/sdX
o # DOS partition table
n # new partition
p # primary
1 # first
<enter> # use the whole disk
<enter> # use the whole disk
a # toggle bootable (make sure it is now ON)
w # write
```

### 1.2 Format to FAT
```
# mkfs.fat -n GIM /dev/sdX1
```

## Or with `gparted`

```
gparted /dev/sdX
# Device -> Create Partition Table... -> select: msdos -> Apply
# Partition -> New -> (configure options) -> Add
	Create as:   Primary Partition
	File System: fat32
	Label:       GIM
# Apply all operations
# Select partition /dev/sdX1 -> Left click -> Manage Flags -> Enable boot flag
```

## 2. Installation Guide (formated disk)

```
mkdir -p /tmp/GIM
mount /dev/disk/by-label/GIM /tmp/GIM
grub-install --boot-directory=/tmp/GIM/boot /dev/sdX
tar xvf grub-iso-multiboot.tar.gz -C /tmp/GIM
umount /tmp/GIM
```

## 3. Adjusts

### 3.1 I don't want to change the label

If you didn't change your label to `GIM` you will need to change
`/tmp/GIM/boot/grub/grub.cfg` last line:

```diff
-scan_isos /boot/iso GIM
+scan_isos /boot/iso <label>
```

### 3.2 I want my GRUB to look like that

Find a [theme](https://www.gnome-look.org/browse/cat/109/ord/rating/) you would
like to use. The one from the screenshot above is
[vimix](https://www.gnome-look.org/p/1009236/#files-panel).

Move the theme contents to the `themes` folder of the instalation, like so:

```
mount /dev/disk/by-label/GIM /tmp/GIM
tar xvf Vimix-1080p.tar.xz -C /tmp/
mv /tmp/Vimix-1080p/Vimix /tmp/GIM/boot/grub/themes/vimix
```

Double check that you have the `theme.txt` file in the correct place
`/tmp/GIM/boot/grub/themes/vimix/theme.txt`.

As the last step, select the theme in `/tmp/GIM/boot/grub/grub.cfg`:

```diff
-  set theme=/boot/grub/themes/starfield/theme.txt
+  set theme=/boot/grub/themes/vimix/theme.txt
scan_isos /boot/iso GIM
```

release the pendrive.

```
umount /tmp/GIM
```

## 4. Add isos to /boot/iso/

Menu entries will be populated according to the files present during boot.
To add isos via terminal, just copy them over:

```
mount /dev/disk/by-label/GIM /tmp/GIM
cp <my-iso-file>.iso /tmp/GIM/boot/iso/
umount /tmp/GIM
```

Consider donating:

```
btc: 1Eufd2CJ2HSALLr9ELXjJ4T3prg6phaQ8o
eth: 0x1e82c8422682f860ed2002dbb027220ecc71c8ec
```
