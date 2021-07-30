---
title: "Processor modes"
series: "osdev"
categories: "notes"
summary: "Processor real mode, protected mode, virtual real mode"
date: 2021-07-30T20:00:00+01:00
draft: false
socialImage: "https://thuc.space/images/processor_modes/social.png"
tags:
- x86
- processor
- real-mode
- protected-mode 
---

Processor modes

 - Create an environment to manage, restrict and scope system memory that processors and tasks see and use.
 - Have three famous modes: real mode, protected mode, virtual real mode.

Processor real mode

 - Support unlimited direct software access to address memory, I/O address and peripheral hardware.
 - No support memory protection, multi-tasking or code privilege levels.
 - Limit 1 MB of RAM.
 - Addresses in real mode correspond to real locations in memory.
 - In real mode, a process can jump to other process memory to modify instruction and data.

Processor protected mode

 - Solve security issues of real mode. Processes can't access other memory spaces.
 - Support virtual memory, paging and multi-tasking.
 - Program run in protected mode only see and use virtual memory. The virtual memory is assigned to programs and maps to real locations in memory.
 - Addresses in protected mode is virtual address locations, not real locations in memory.


Virtual real mode
 - Processor runs in the protected mode that's emulated 16bit real mode machine.
