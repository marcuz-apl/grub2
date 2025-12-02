# Everything About Grub2 and Linux Kernel

by marcuz-apl | 24 Oct 2025



## Intro

I met a situation, that one of my Ubuntu Workstations was down due to failure of its EFI Partition, while the `root` partition of OS, apps, projects was still okay, accessible from other OS, if shared.

Then I have to bring up a new SATA disk and copy the data partition over and rebuild a grub bootloader for the new disk.

I didn't make any complex partition table for those old Ubuntu stations, pretty much 2 partitions for every workstations:

​	`/dev/sda1` - EFI partition: `/boot/efi` - in `vfat` file system

​	`/dev/sda2` - root partition: `/` - in `ext4` file system

This simple partition table scheme, without the `lvm`, saves a lot of time sometimes.



## Case Studies with Step-by-Step Guide

Case 1: [Migrate a Legacy BIOS Linux System to UEFI Scheme in VM](./Migrate-a-Legacy-BIOS-Linux-System-to-UEFI-Scheme-in-VM.md)

Case 2: [Partition-Clone Ubuntu 24 from VM to Physical Drive](./Partition-Clone-Ubuntu-24-from-VM-to-Physical-Drive.md)

Case 2: [Partition-Clone Debian 12 from VM to Physical Drive](./Partition-Clone-Debian-12-from-VM-to-Physical-Drive.md)



## GRUB2 Command Shell

1- [Fix Ubuntu Not Booting Issue with Boot Repair tool](./command-shell/Fix-Ubuntu-Not-Booting-with-Boot-Repair.md)

2- [Operations on Grub](./command-shell/Operations-on-Grub.md)

3- [Rescue a non-booting GRUB2 on Ubuntu 14.04](./command-shell/Rescue-a-non-booting-GRUB2-on-Ubuntu-14.md)

4- [Rescue or Reinstall GRUB2 - the Manual Way](./command-shell/Rescue-or-Reinstall-GRUB2-the-Manual-Way.md)

5- [Use GRUB2 Command Line to Boot up](./command-shell/Use-GRUB2-Command-Line-to-Boot-up.md)



## Kernel is much Essential

1- [Kernel Versions - The Official](./kernel/Kernel-Versions-The-Official.md)

2- [Kernel Version and Operations of Ubuntu-Debian](./kernel/Kernel-Versions-and-Operations-of-Ubuntu-Debian.md)

3- [Update Kernel for Ubuntu 22.04 and Debian 12](./kernel/Update-Kernel-for-Ubuntu-22.04-Debian-12.md)

## Reference

GNU Grub Manual 2.12 (in html format): [Official website](https://www.gnu.org/software/grub/manual/grub/grub.html) or [local downloadable](./reference/GNU-Grub-Manual-2.12.html)

Grub 2 bootloader - Full Tutorial: [Official website](https://www.dedoimedo.com/computers/grub-2.html) or [local downloadable](./reference/Grub2-bootloader-Full-Tutorial-dedoimedo.md)

Grub2 - ArchLinux Wiki: [Official website](https://wiki.archlinux.org/title/GRUB) or [local downloadable](./reference/Grub2-ArchLinux-Wiki.md)

Grub2 - Ubuntu Community Help Wiki: [Official website](https://help.ubuntu.com/community/Grub2) or [local downloadable](./reference/Grub2-Ubuntu-Community-Help-Wiki.md)



## The End
