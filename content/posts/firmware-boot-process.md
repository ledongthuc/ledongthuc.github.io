---
title: "Firmware boot process"
series: "linux"
categories: "notes"
summary: "Quick notes about firmware boot process"
date: 2021-02-19T21:00:00+01:00
draft: false
tags:
- linux
- firmware
- boot
---

General Steps:
 - Click the power button on
 - CPU load instruction from firmware chip on the motherboard and execute it
 - Firmware code does initialize and tests system hardware.
 - Firmware start bootloader and give the next step to the boot loader.

Two famous firmware:
 - BIOS firmware
 - UEFI firmware

BIOS firmware:
 - Frequently is stored in ROM
 - Initializes and tests system hardware
 - Bases on user predefined prioritized boot device, BIOS try to check each device in order to see if it is bootable by trying to load MBR.
 - From valid MBR, BIOS loads boot loader and give next step to it.

MBR
 - Special type of boot sector
 - The very beginning of the partitioned hard disk

UEFI firmware
 - Designed to replace BIOS firmware 
 - Initializes and tests system hardware.
 - UEFI, or user let UEFI knows which boot loader is used. Not dependent on disk and MBR.
 - Boot loader of UEFI is stored in the EFI system partition of the hard drive GPT partition type.

Readmore:
 - https://www.happyassassin.net/posts/2014/01/25/uefi-boot-how-does-that-actually-work-then/
 - https://uefi.org/sites/default/files/resources/2_4_Errata_A.pdf
 - https://wiki.osdev.org/UEFI
