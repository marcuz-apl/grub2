# Partition-Clone Ubuntu 24 from VM to Physical Drive

by marcuz-apl | 10 November 2025



## Table of Contents

[Case Profile](#case-profile)

[Pre-requisite - System Prep](#pre-requisite---system-prep)

[Prepare the problematic SATA disk](#Prepare-the-problematic-SATA-disk)

[Copy the OS-Data Partition](#Copy-the-OS-Data-Partition)

[Create the GRUB2 bootloader](#Create-the-GRUB2-bootloader)

[Post-migration](#post-migration)

[The End](#the-end)



## Case Profile

I have installed a Ubuntu 24.04 system into VMware VM, which I've customized quite a bit, even with a macOS flavor desktop (crazy, am I?), running very well with quite a few projects. Since I have acquired quite some experience of grub2 operations, I would like to clone such Ubuntu 24.04 from the VMware to the Physical SATA Disk which is big-sized at 1.8TB.

Generally speaking, the steps are:

1- Attach the problematic SATA disk to the working VMware VM, running Ubuntu 24.04.

2- Prepare the problematic SATA disk.

3- Copy the content of OS, apps and data from the Ubuntu VM partition to the prepared SATA partition.

3- Create the GRUB2 bootloader for the newly-prepared SATA disk.

4- Detach the Physical disk from VM and test it on a real-world machine.



## Pre-requisite - System Prep

First thing first, disable the **Secure Boot** to achieve a smoother operation. 

Better have a working Ubuntu 24.04 system handy. It's a Ubuntu 24.04 with macOS flavor for my case.

Run your VMware Workstations as Administrator; then add the Physical Disk (the whole disk) as a hard disk into the VM.

- The working Linux system is ready as **/dev/sda** (128GB)

- The problematic SATA Disk is listed as **/dev/sdb** (1.8TB)




Once booted into the Ubuntu 24.04 with macOS flavor, the desktop is a as below:

![desktop](./assets/img-01-sda-desktop.png)

Also the view of `/dev/sdb` on `GParted` app tells corruption.

![sdb-corrupted](./assets/img-02-sdb-corrupt-part.png)

It seems that the `/dev/sdb` has quite some issues. anyway we are gonna format it.



## 1- Prepare the problematic SATA disk

```shell
## Install fat32 supporting mtools
sudo apt install mtools dosfstools

## lsblk to get info of the partitions
lsblk
## Output belike:
## ......
## sda		8:0		0	  128G	0	disk
## |-sda1	8:1		0		1G	0	part /boot/efi
## |-sda2	8:2		0	  127G	0	part /
## sdb		8:16	0	  1.8T	0	disk
## sr0	   11:0		0	 1024M	0	rom
```

Here is the snapshot:

![initial sda n sdb](./assets/img-03-initial-sda-n-sdb.png)



Let's get started with `parted` tool:

```shell
## launch the tool: parted
sudo parted /dev/sdb

## print the partitions
print
```

The system complains, then type in "**OK**" to proceed.

```text
Error: The primary GPT table is corrupt, but the backup appreas OK, so that will be used.
OK/Cancel? (Typed "OK" here)
Model: ATA Vmware Virtual S (scsi)
Disk /dev/sdb: 2000GB
Sector size )logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number	Start	End		Size	File system	Name	Flags
 1		1049KB	2097KB	1049KB						bios_grub
 2		2097KB	58.9GB	85.9GB	ext4				
 
 (parted)
```

Anyway we are going to remove these 2 problematic partitions:

```shell
rm 1
rm 2
```

Then make partitions:

```shell
## make partitions in the env of (parted):
mkpart "efi" fat32 1MiB 1024MiB
set 1 esp on
mkpart "system" ext4 1025MiB 501GiB
mkpart "DATA" fat32 502GiB 100%
print
```

The output belike:

```text
...
Number	Start	End		Size	File system	Name	Flags
 1		1049KB	1074MB	1073MB				efi		boot, esp
 2		1074MB	538GB	537GB	ext4		system	
 3		539GB	2000GB	1462GB	fat32		DATA	msftdata
```

Then quit the (parted) env and format the partitions:

```shell
## Quit the (printed)
quit
## List the new partitions on /dev/sdb
lsblk
## ......
## sda		8:0		0	  128G	0	disk
## |-sda1	8:1		0		1G	0	part /boot/efi
## |-sda2	8:2		0	  127G	0	part /
## sdb		8:16	0	  1.8T	0	disk
## |-sdb1	8:17	0	 1023M	0	part
## |-sdb2	8:18	0	  500G	0	part
## |-sdb3	8:19	0	  1.3T	0	part
## sr0	   11:0		0	 1024M	0	rom
```

The `/dev/sdb` shall be in good standing now:

![sdb in good standing](./assets/img-04-sdb-view-after-partitioning-with-gparted.png)

Then format the partitions:

```shell
## Format the partitions
sudo mkfs.fat -F32 /dev/sdb1
sudo mkfs.ext4 /dev/sdb2
sudo mkfs.fat -F32 /dev/sdb3    ## This takes a while as it's a 1.3TB partition.
```

The operations above can be viewed in a snapshot as below:

![ops summary](./assets/img-05-sdb-ops-summary.png)



## 2- Copy the `root` Partition

> [!IMPORTANT]
>
> 1- To move a root partition to another disk using `dd`, you must boot from a live environment, identify the source and destination partitions, and then use `dd` to copy the data. 
>
> 2- After the copy, update the new partition's `/etc/fstab` file with the new UUIDs and reinstall the bootloader to make the new drive bootable. 
>
> 3- While `dd` can create an exact block-by-block copy, it can be dangerous and is best for partitions of the same size; for different sizes or a safer method, using `rsync` after creating a new filesystem is recommended. 

**Boot the system with a Live DVD/ISO of Ubuntu Linux 24.04**.

Assuming the OS and Data are all installed at the root partition (`/`) at `/dev/sdb2`. If not on one partition, so the same for other partitions.

#### 2a) Hardcopy the root partitions using `dd` command (Not Recommended)

Use this method only if the destination partition is at least the same size as the source partition.

```shell 
## Ensure the disks and partitions
lsblk
## Copy OS+App+Data partition from the working disk /dev/sda2 to /dev/sdb2
sudo dd if=/dev/sda2 of=/dev/sdb2 bs=4M conv=noerror,sync status=progress
## Check the UUID
sudo blkid
```

`conv=noerror,sync` tells `dd` to continue if it encounters errors and to pad with zeros if it reads fewer bytes than requested

This method is great only if -

​	(1) the source and destination are in same size and 

​	(2) the source and destination are going to used in the same hardware env.

**Resize the new root partition:**

If the destination partition is larger, you'll need to resize the filesystem to use the extra space (with `resize2fs` for ext4, or using GParted).

**Update the UUID of new root partition:** 

As we all know, after `dd` operations, `blkid` reports that `/dev/sda2` and `/dev/sdb2` have same filesystem UUID. That's what DD does, no surprise! We need to generate a new UUID for the new `root` partition:

```shell
## For ext-family filesystem
sudo tune2fs -U random /dev/sdb2

## For btrfs filesystem
sudo btrfs -U $(uuidgen) /dev/sdb2
sudo btrfstune -U 0de6bd81-7013-49a8-bdc5-d832ed209d2c /dev/sdb2
## For NTFS filesystem, use ntfslabel thanks to ntfs-3g
sudo ntfslabel --new-serial=1122334455667788 /dev/sdb3
```



#### 2b) Synchronize the root partitions using `rsync` command (Recommended)

This file-level copy method is safer and handles different partition sizes better.

```shell
## Mount the source and destination partitions
sudo mkdir /mnt/{src,dst}
sudo mount /dev/sda2 /mnt/src
sudo mount /dev/sdb2 /mnt/dst
## Copy files while preserving permissions, ownership, and timestamps, and excluding virtual folders
sudo rsync -aXS --info=progress2 --exclude={/dev/*,/proc/*,/sys/*,/run/*,/tmp/*,/lost+found/*,/mnt/*, /media/*} /mnt/src/ /mnt/dst/
## Umount and remove all
sudo umount -R /mnt/src
sudo umount -R /mnt/dst
sudo rmdir /mnt/{src,dst}
```



## 4- Update `/etc/fstab` on the root partition

Now we are gonna create a GRUB bootloader on `/dev/sdb` using the configuration from `/dev/sda`.

- Identify the root partition (that's `/`) on `/dev/sda`: Determine which partition on `/dev/sda` contains your Linux root filesystem (e.g., `/dev/sda1`, `/dev/sda2`).

  ```shell
  lsblk
  ```

  Grab the UUID of the new disk (`/dev/sdb`):

  ```shell
  sudo blkid /dev/sdb
  ```
  
  The output belike:
  
  ```shell
  /dev/sdb1: UUID="8E55-11E4" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="efi" PARTUUID="2d9d0d58-......"
  /dev/sdb2: UUID="2404556e-e7de-48fb-a07f-5e81b99ca105" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="system" PARTUUID="0b63ab5b-......"
  /dev/sdb3: UUID="9246-66F3" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="DATA" PARTUUID="5f29337b-......"
  ```

  

- Mount the  root partition from `/dev/sdb2` to `/mnt`, and mount `/dev/sdb1` to `/mnt/boot/efi`:

  ```shell
  ## Mounting
  sudo mount /dev/sdb2 /mnt
  sudo mount /dev/sdb1 /mnt/boot/efi
  ```
  
  

* Edit `/etc/fstab` file of `/dev/sdb2`:

  ```shell
  sudo nano /mnt/etc/fstab
  ```
  
  Update the content of `fstab` :
  
  ```shell
  # <File system> <mount point>   <type>  <options>     <dump> <pass>
  # / was on /dev/sda2 during curtin installation
  /dev/disk/by-uuid/ad197ec0-5e3d-4128-a0b2-e525c471b1b2 / ext4 defaults 0 1
  # /boot/efi was on /dev/sda1 during curtin installation
  /dev/disk/by-uuid/970D-8713 /boot/efi vfat defaults 0 1
  /swapfile	    none	swap	sw	0	0
  ```
  
  Save the change!
  



## 5- Create the GRUB2 bootloader

* Bind mount necessary system directories from the local running `/dev/sda` to `/mnt`.

  ```shell
  sudo mount --rbind /dev /mnt/dev
  sudo mount --rbind /dev/pts /mnt/dev/pts
  sudo mount --rbind /proc /mnt/proc
  sudo mount --rbind /run /mnt/run
  sudo mount --rbind /sys /mnt/sys
  ## Or a simple script as below
  # for i in dev dev/pts proc sys run; do sudo mount -R /$i /mnt/$i; done
  ```

* Chroot into the mounted system.

  ```shell
  sudo chroot /mnt
  ```

  

* **For UEFI systems**: Reinstall the GRUB EFI packages:

  ```shell
  apt-get install --reinstall grub-efi-amd64 shim-signed
  ```

  

* Install GRUB to the target disk: `/dev/sdb`. 

  ```shell
  grub-install /dev/sdb
  ## If error, specify the parameters as below:
  grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=ubuntu /dev/sdb
  ```
  
  * It shall report:
  
    ```text
    Installing for x86_64-efi platform.
    Installation finished. No error reported.
    ```
  
    

* Update GRUB configuration.

  ```shell
  ## optional: Update initRamFS
  update-initramfs -u
  
  ## Update grub.cfg
  update-grub
  
  ## One warning may occur, which doesn't affect anything, no worry.
  ## Warning: os-prober will not be added to the GRUB boot configuration.
  ```

* Exit the chroot environment.

  ```shell
  exit
  ```

  

* Unmount the filesystems.

  ```shell
  sudo umount -R /mnt/boot/efi
  sudo umount -R /mnt
  ```



* In case of error: "`cannot umount: /mnt/sys: target is busy`", indicating that a process or user is currently accessing files or directories within the `/mnt/sys` mount point, preventing it from being unmounted, please do:

  ```shell
  ## Use the `fuser` command to list the PIDs of processes accessing `/mnt/sys`:
  fuser -m /mnt/sys
  ## Alternatively, use `lsof` for a more detailed list of open files:
  lsof /mnt/sys
  ## Perform a "lazy" unmount (if necessary): 
  ## For situations where immediate unmounting is not critical and processes cannot be easily terminated
  sudo umount -l /mnt/sys
  ```



## Post-migration

After these steps, `/dev/sdb` will have a GRUB bootloader installed, configured to boot the system from `/dev/sdb`. 

Now, it's a good time to remove other disks and boot up with the new SATA disk with GPT/UEFI system.

You may need to adjust your BIOS/UEFI settings to boot from `/dev/sdb` if you intend to use it as the primary boot disk.

Once booted, the new SATA disk becomes `/dev/sda` since no other disks is connected.



#### Update UEFI Boot Entries (if necessary)

Use `efibootmgr` to create or modify UEFI boot entries to point to the GRUB bootloader on `/dev/sda`.

```
sudo efibootmgr -c -d /dev/sda -p 1 -L "My New Ubuntu OS" -l "\EFI\Ubuntu\shimx64.efi" 
```

(Adjust `-p 1` for the correct partition number, `-L` for a descriptive label, and `-l` for the correct bootloader path.)



## The End
