# [GRUB iso multiboot]

+ Auto detect and create entries for .iso files in /iso/
+ Preserves its boot options!
+ Small and lean, few entries lots of images
+ new iso, same script. No need to tweak around (ymmv).

![preview with vimix](doc/vimix.png?raw=true "vimix")
![preview with starfield](doc/starfield.png?raw=true "starfield")

If your pendrive is already formated to FAT skip to `2`

## 1. Installation Guide (clean disk)

```
export LABEL=<label-of-your-choice>
```

### 1.1 Create a partition table:
```
# fdisk --wipe auto /dev/sdX
g # GPT partition table
n # new partition
1 # first
<enter> # use the whole disk
<enter> # use the whole disk
w # write
```

### 1.2 Format to FAT
```
# mkfs.fat -F32 -n $LABEL /dev/sdX1
```

## Or with `gparted`

```
gparted /dev/sdX
# Device -> Create Partition Table... -> select: msdos -> Apply
# Partition -> New -> (configure options) -> Add
	Create as:   Primary Partition
	File System: fat32
	Label:       $LABEL
# Apply all operations
# Select partition /dev/sdX1 -> Left click -> Manage Flags -> Enable boot flag
```

## 2. Installation Guide (formated disk)


### 2.1a mount
```
mkdir -p /tmp/$LABEL
sudo mount /dev/disk/by-label/$LABEL /tmp/$LABEL
```

### 2.1b mount (as a regular user)
```
pmount /dev/disk/by-label/$LABEL
# adapt destination to the created folder in /media for the following commands
# mount point will look something like this: /media/disk_by-label_$LABEL
```

### 2.2 install grub & copy release files
```
sudo grub-install --force --removable --no-floppy \
    --target=x86_64-efi \
    --boot-directory=/tmp/$LABEL/boot/ \
    --efi-directory=/mnt/$LABEL/ /dev/sdX
tar xvf grub-iso-multiboot.tar.gz -C /tmp/$LABEL
```

### 2.3a unmount
```
umount /tmp/$LABEL
```

### 2.3b unmount
```
pumount /media/disk_by-label_$LABEL
```

## 3. Adjusts

### 3.1 I want my GRUB to look like that

Find a [theme](https://www.gnome-look.org/browse/cat/109/ord/rating/) you would
like to use. The one from the screenshot above is
[vimix](https://www.gnome-look.org/p/1009236/#files-panel).

Move the theme contents to the `themes` folder of the instalation, like so:

```
mount /dev/disk/by-label/$LABEL /tmp/$LABEL
tar xvf Vimix-1080p.tar.xz -C /tmp/
mv /tmp/Vimix-1080p/Vimix /tmp/$LABEL/boot/grub/themes/vimix
```

Double check that you have the `theme.txt` file in the correct place
`/tmp/$LABEL/boot/grub/themes/vimix/theme.txt`.

As the last step, select the theme in `/tmp/$LABEL/boot/grub/grub.cfg`:

```diff
-  set theme=/boot/grub/themes/starfield/theme.txt
+  set theme=/boot/grub/themes/vimix/theme.txt
```

release the pendrive.

```
umount /tmp/$LABEL
```

## 4. Add isos to /boot/iso/

Menu entries will be populated according to the files present during boot.
To add isos via terminal, just copy them over:

```
mount /dev/disk/by-label/$LABEL /tmp/$LABEL
cp <my-iso-file>.iso /tmp/$LABEL/boot/iso/
umount /tmp/$LABEL
```

## 5. Development

A `Dockerfile` is provided to help building the project.
It will setup a compilation environment 

```
sudo qemu-system-x86_64 --enable-kvm /dev/sda -bios /usr/share/edk2-ovmf/OVMF_CODE.fd
```
