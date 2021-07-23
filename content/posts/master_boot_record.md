---
title: "Master Boot Record"
series: "osdev"
categories: "notes"
summary: "MBR, MBR partition, MBR sequence, compare with VBR"
date: 2021-07-23T19:00:00+01:00
draft: false
socialImage: "https://thuc.space/images/master_boot_record/master_boot_record-format.png"
tags:
- x86
- mbr
- boot
- kernel
---

Master boot record
 - First sector of a physical hard disk.
 - This sector is not a part of any partition (OS or data).
 - Contains two parts:
   - MBR bootstrap program to locates the boot sector of other partitions.
   - Partition table contains a master list of all available partitions on all available disks.
 - Master boot record is detected by boot signature (magic number).

MBR Partition table
 - Apply to a hard disk drive, use to manage partitions in disk
 - 16 bytes, total's 64 bytes. Limited to 4 entries. To create more, use an extended partition.
 - Maximum size of a single MBR partition is 2TB.
 - Elements:
    - Boot indicator: inactive - active/bootable partition
    - Bits to define Starting head, sector and Cylinder
    - System ID: define filesystem of the partition (ext2, fat32, NTFS, etc.)
    - Relative sector
    - Total sectors in the partition

![Master Boot Record format](/images/master_boot_record/master_boot_record-format.png)

Extended partitions
 - A solution to add more than 4 partitions to the partition table.
 - Partition can have one and only one entry with SystemID = 0x5 or 0xF to define as an extended partition.
 - Extended partition is a physical partition that's subpartitioned into "logical" partitions.
 - partition table entry describing it has a StartLBA and NumSectors that describe the space inside which any number of logical partitions may sit.
 - At the start of an extended partition is an extended partition table. Format of extended partition table is same with MBR partition table.

Master Boot Record sequence
 - After BIOS finish their job, they load the first sector (512 bytes) of devices (Hardisk, disc, USB, etc.) to check boot signature.
 - The boot signature is called a magic number. If a device has a boot signature, it's can bootable.
     - Boot signature is in a boot sector number 0.
     - Contains byte sequence: 0x55, 0xAA at byte offets 510 (0x1FE) and 511 (0x1FF).
     - 0x1FE and 0x1FF is 2 last bytes of 512 bytes of the first sector.
 - When BIOS recognize a boot device, master boot record is loaded into memory at 0x0000:0x7c00 (sector 0, address 0x7c00) from this device. It contains executable code.
 - Execution is transferred to the loaded boot record bootstrap program.

![Master Boot Record format](/images/master_boot_record/master_boot_record-magic_numbers.png)

Master boot record bootstrap program: 
 - Currently, the DL register is set to "drive number" that MBR was loaded. 
 - Master boot record process relocates itself away from 0x7c00: memory copy and far jump.
 - Determine which partition/device to boot OS.
   If the user selects inactive partition, then set the partition chosen entry to active and clear active state from others.
 - Rewrite MBR if the partition table entries were modified.
 - Load Volume Boot Record (boot sector of bootloader) from selected partition.
 - Set DS:SI pointing to the selected partition table entry.
 - Jump to 0x7c00 (with CS set to 0, and DL set to the "drive number").

Volume boot record
 - First sector of the first partition containing the OS.
 - Contains boot loader code and some file system info.

Bootloader
 - A program is in the boot sector (master boot record or Volume boot record).
 - Use to loads OS kernel.
