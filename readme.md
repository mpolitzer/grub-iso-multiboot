[GRUB iso multiboot]
====================

1. Installation Guide (clean disk)
----------------------------------

1.1 Create a partition table:
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

1.2 Format to FAT
```
# mkfs.fat -n GIM /dev/sdX1
```

Or with `gparted`

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

2. Installation Guide (formated disk)
-------------------------------------

```
mkdir -p /tmp/GIM
mount /dev/disk/by-label/GIM /tmp/GIM
grub-install --boot-directory=/tmp/GIM/boot /dev/sdX
tar xvf grub-iso-multiboot.tar.gz -C /tmp/GIM
umount /tmp/GIM
```

3. Add isos to /boot/iso/
-------------------------
