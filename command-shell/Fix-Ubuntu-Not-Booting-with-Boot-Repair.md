# Fix Ubuntu Not Booting Issue with boot-repair tool

Ubuntu boot issues might occur because of bootloader problems, Grub issues, etc.

Boot repair Ubuntu is one of the used tools to fix Ubuntu boot problem and repair common Linux not booting issues.

To use the Linux boot repair tool, the below steps are **required**:

- Create a bootable USB with GPT partitioning.
- Boot your computer from USB in **UEFI mode** (Unified Extensible Firmware Interface).
- Install Boot-Repair.
- Select the ‘’Recommended repair’’ option.

**Note**: Make sure that the Ubuntu entry is the default boot option and that your computer is configured to boot in UEFI mode.

When you restart your machine after using the boot-repair Ubuntu solution, Ubuntu should now load in UEFI mode.

Table of Contents



## 1- Prerequisites

To let this tutorial work correctly, provide the options below:

- A system or server running Ubuntu.
- A non-root user with `sudo` privileges.
- Install Boot Repair



## 2- Detailed Steps

Typically, booting issues might be caused by a faulty file in the */boot* directory or the GRUB boot menu.

Boot Repair is a great piece of software to get us started on troubleshooting, whatever the situation may be.

Let’s go through the steps of this guide to be an expert in facing Ubuntu not booting occasions.

#### Step 0. Install Ubuntu Boot Repair

To boot into a Live Ubuntu environment, you are recommended to make a **bootable Ubuntu USB disk**.

Since you do not need to reinstall Ubuntu first to fix Ubuntu boot problem, select **Try Ubuntu without installing** from the GRUB menu and continue.

> [!NOTE]
>
> The Boot Repair tool is not available in the Ubuntu official repository. So, open your terminal window and run the command below to install it.

```shell
$ sudo add-apt-repository ppa:yannubuntu/boot-repair
$ sudo apt update
$ sudo apt install -y boot-repair
```

To launch and start the application, type:

```elixir
$ boot-repair
```

Wait a few minutes to let the app scan your hard drive and finish the installation.

#### Step 1 . Using Boot Repair - the "Recommended repair"

When the Boot Repair is started, try the ”**Recommended repair**” option to let it troubleshoot some of the most common boot problems.

![Boot Repair - Recommended Repair](./assets/S1-Using-Boot-Repair-tool-Recommended-repair.png)

It may take several minutes to find and fix boot problems. 

​	- If you prefer to upload the report to `Pastebin`, click on ”**Yes**” when you are prompted.

Once you see the ”**Boot successfully repaired**” message and a text document, you’re all done with this tool.

Review your system information and the summary of the boot repair function and boot into your installed OS.

![Boot Repair Summary](./assets/S1-Using-Boot-Repair-tool-Recommended-repair-Summary.png)

Then you can reboot the system to check if the problems are fixed.

Move on with the remaining steps to check other solutions for Ubuntu repair boot.

#### Step 2. Using Boot Repair Advanced options - 'Main options' tab

If you still have problems, there are many advanced options in Boot Repair.

As you saw in the above image (fig.1), there is an ”**Advanced options**” in Boot Repair which offers other repair options.

To use them, click on the **Advanced options** to view the below menu in Boot Repair.

![how to use boot repair tool](./assets/S2-Use-Boot-Repair-tool-Advanced-options.png)

To receive a text summary of your current issue, select the ”**Create boot info summary**” option.

Also, if you think the GRUB is the reason for not booting your Ubuntu, you can reinstall GRUB or choose a new location to store the bootloader configuration.

#### Step 3. Using Boot Repair Advanced options - "GRUB location" tab

When Ubuntu does not boot, by using Boot Repair’s advanced settings, you may determine where GRUB is installed on your hard drive.

You may wish to specify which hard drives you want to repair here if your system has numerous installed hard drives with GRUB installed on it.

To do this:

- Go to the **GRUB location** tab of Boot Repair to alter the GRUB location.
- Choose the hard disk partition from the drop-down menu under “**OS to boot by default**” now.
- Choose the hard disk partition that is utilized as the EFI System Partition from the **Separate /boot/efi partition** drop-down selection if your motherboard is UEFI-based.

![Advanced options - GRUB location](./assets/S3-Use-Boot-Repair-tool-Advanced-options-GRUB-location.png)

#### Step 4. Change "GRUB options" from Boot Repair

The **GRUB options** tab of Boot Repair enables you to modify a lot of the GRUB settings.

![Change-GRUB-Options-from-Boot-Repair](./assets/S4-Change-GRUB-Options-from-Boot-Repair.png)

#### Step 5. Back-Up Partition Table using Boot Repair

With Boot Repair, you can back up your partition table.

It is crucial since you can restore the partitions and recover your data if your partition table is somehow compromised.

You risk losing all of your data if you don’t.

Simply click the **Backup partition tables, boot sectors, and logs** button as seen in the screenshot below to back up your partition tables.

![Back-Up Partition Table using Boot Repair](./assets/S5-Back-Up-Partition-Table-using-Boot-Repair.png)

Then, choose the location you consider saving the partition table data and click on ”**Save**”.

It will take several minutes. When saving the partition is saved, you will see a box. Click on ”**OK**”.

#### Step 6. Repair File Systems using Boot Repair

Your file systems may occasionally become corrupt, and Ubuntu won’t be able to automatically fix it when the system boots.

That could lead to a boot failure.

With Boot Repair, the file system can be fixed. Select the **Main options** tab, tick the box next to **Repair file systems**, and then click **Apply**.

The filesystem repair and boot issues need to take some time.

You should be able to boot into your installed operating systems once more once it’s finished.



## 3- Conclusion

Now, you know how to install and use the Boot-Repair tool on your Ubuntu live system to troubleshoot boot issues on Ubuntu. This tool helps you to fix the most common boot problems.
