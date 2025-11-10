# Rebuild the Grub2 Bootloader

by marcuz-apl | 24 Oct 2025



## Case Study

I met a situation, that one of my Ubuntu Workstations was down due to failure of its EFI Partition, while the data partition of os, apps, projects was still okay (accessible from other OS, if sharing out).

Then I have to bring up a new SATA disk and copy the data partition over and rebuild a grub bootloader for the new disk.

I didn't makde any complex partition table for those old Ubuntu stations, pretty much 2 partitions for every workstations:

​	`/dev/sda1` - mounting point: `/boot/efi` - in `vfat` file system

​	`/dev/sda2` - mounting point: `/` - in `ext4` file system

This simple partition table scheme, without the `lvm`, saves a lot of time sometimes.



## How-to Guide

Please refer to [Rebuild the Grub2 Bootloader](./Rebuild the Grub2 Bootloader.md) for detailed guide.



## Reference

GNU Grub Manual 2.12.html: [Official website](https://www.gnu.org/software/grub/manual/grub/grub.html) or [local downloadable](./reference/GNU Grub Manual 2.12.html)

Grub 2 bootloader - Full Tutorial: [Official website](https://www.dedoimedo.com/computers/grub-2.html) or [local downloadable](./reference/Grub 2 bootloader - Full Tutorial - dedoimedo.md)

Grub2 - ArchLinux Wiki: [Official website](https://wiki.archlinux.org/title/GRUB) or [local downloadable](./reference/Grub2 - ArchLinux Wiki.md)

Grub2 - Ubuntu Community Help Wiki: [Official website](https://help.ubuntu.com/community/Grub2) or [local downloadable](./reference/Grub2 - Ubuntu Community Help Wiki.md)



## The End