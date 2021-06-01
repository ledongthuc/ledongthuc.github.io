---
title: "Linux kernel boot process"
series: "linux"
categories: "notes"
summary: "Quick notes about Linux's kernel boot process"
date: 2021-06-01T21:00:00+01:00
draft: false
tags:
- linux
- boot
- kernel
---

 - BIOS loads Linux Kernel to memory and split to 2 parts:
 	- Real-mode kernel: Below 640K of memory
 	- Proctected-mode kernel: Start from 1MB of memory

Real-mode kernel:

 - http://lxr.linux.no/#linux+v2.6.25.6/arch/x86/boot/header.S#L110
 	- Real-mode kernel will be start by BIOS at `_start:`
 	- It will jump directly to `start_of_setup:`

 - http://lxr.linux.no/#linux+v2.6.25.6/arch/x86/boot/header.S#L229
 	- `_start` jumps to `start_of_setup:`
 	- Reset disk controller
 	- Setup stack
 	- Zero bss (https://en.wikipedia.org/wiki/.bss)
 	- Call to a C `main()`

 - http://lxr.linux.no/#linux+v2.6.25.6/arch/x86/boot/main.c#L122
 	- `start_of_setup:` calls `main()`
 	- Prepare/copy boot parameters
 	- Initilize memory heap
 	- Detect memory layout
 	- Query MCA
 	- Set video mode
 	- Invoke method `go_to_protected_mode()`

 - http://lxr.linux.no/#linux+v2.6.25.6/arch/x86/boot/pm.c#L153
 	- `main()` calls `go_to_protected_mode()`
 	- The method purpose is used to prepare steps before switch to protected mode
 	- Switch hook
 	- Move compressed kernel to correct place
 	- Enable co-processors
 	- Setup IDT https://en.wikipedia.org/wiki/Interrupt_descriptor_table
 	- Setup GDT https://en.wikipedia.org/wiki/Global_Descriptor_Table
 	- Jump to `protected_mode_jump:`

Protected-mode:

 - http://lxr.linux.no/#linux+v2.6.25.6/arch/x86/boot/pmjump.S#L31
 	- `go_to_protected_mode()` calls `protected_mode_jump:`
 	- Prepare/copy boot parameters
 	- Enable protected mode
 	- Disable paging https://en.wikipedia.org/wiki/Memory_paging
 	- Jump to `startup_32:`

 - http://lxr.linux.no/#linux+v2.6.25.6/arch/x86/boot/compressed/head_32.S#L35
 	- `protected_mode_jump:` jumps to `startup_32:`
 	- Doing some stuffs
 	- Call to `decompress_kernel()`
 	- Clears the bss segment
 	- Setup final GDT https://en.wikipedia.org/wiki/Global_Descriptor_Table
 	- Enables paging https://en.wikipedia.org/wiki/Memory_paging
 	- Initializes a stack
 	- Setup final IDT https://en.wikipedia.org/wiki/Interrupt_descriptor_table
 	- Call to `start_kernel()`


 - http://lxr.linux.no/#linux+v2.6.25.6/arch/x86/boot/compressed/misc.c#L368
 	- `startup_32:` calls to `decompress_kernel()`
 	- Uncompressed kernel will replace at place of existed compressed kernel. Start at 1MB

 - http://lxr.linux.no/#linux+v2.6.25.6/init/main.c#L507
 	- `startup_32:` calls to `start_kernel()`
 	- Set PID = 0
 	- Initialize stuffs, such as: scheduler, memory zones, time keeping, cpu, cgroup
 	- Call to `rest_init()`

 - http://lxr.linux.no/#linux+v2.6.25.6/init/main.c#L432
 	- `start_kernel()` calls to `rest_init()`
 	- Create another thread to call `kernel_init()`
 	- Create scheduling
 	- Sleep by call `cpu_idle()`

 - lxr.linux.no/linux+v2.6.25.6/init/main.c#L808
 	- `start_kernel()` create thread and call to `kernel_init()`
 	- Set PID = 1
 	- Init the remaining CPUs
 	- Init work queue
 	- Init user mode
 	- Init driver
 	- Call to `init_post()`

 - http://lxr.linux.no/#linux+v2.6.25.6/init/main.c#L769
 	- `kernel_init()` calls to `init_post()`
 	- Execute /sbin/init, /etc/init, /bin/init, and /bin/sh in order
