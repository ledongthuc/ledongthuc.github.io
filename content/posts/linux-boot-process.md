---
title: "Linux boot process"
series: "linux"
categories: "notes"
summary: "Quick notes about Linux boot process"
date: 2021-02-21T21:00:00+01:00
draft: false
tags:
- linux
- firmware
- boot
---

General steps:
 - Power on.
 - BIOS/UEFI.
 - Bootloader.
 - Kernel load.
 - Run level.

BIOS/UEFI
 - Read more at https://thuc.space/posts/firmware-boot-process/

Bootloader
 - Choose OS.
 - Choose kernel.
 - Load and execute kernel and initrd.

Kernel load
 - initrd loads a temporary root file system into memory until real file system is mounted.
 - Start load kernel and real file system.
 - Mount real root file system.
 - Execute /sbin/init as 1st program, can be systemd or SysV init.

Run level
 - Mount file system.
 - Decide what run-level will be run.
 - Run scripts from /etc/rc[0-6S].d directory bases on run-level.
  - /etc/rc[0-6S].d/S.... : is run when starting.
  - /etc/rc[0-6S].d/K.... : is run when stopping.

Read more: 
 - https://man7.org/linux/man-pages/man7/boot.7.html
