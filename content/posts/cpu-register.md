---
title: "CPU Register"
series: "computer architect"
categories: "notes"
summary: "CPU register, execute cycle"
date: 2021-03-11T21:33:00+01:00
draft: false
tags:
- cpu
- register
- computer architect
---

 - It's high-speed memory storage used by the processor.
 - It's used support for the processor to store, decode and execute computer instruction.
 - It's the fastest memory level in computer architect

Memory register types:
- Accumulator (Arithmetic Logical Unit)
  - Performing arithmetic operations and also in logical operations
  - Holds the initial data, intermediate results and final result of the instruction
- Flag Register
- Memory Data Register
  - Store data to be written into or to be read out from the addressed location.
- Program Counter
  - Is called "instruction pointer".
  - Store address of next instructions need to fetch or execute.
- Instruction Register
  - Store instruction that's fetched from memory. Unit control will use it to decode and execute
- Stack Control Register
- Memory Buffer Register
- Index register
- Memory address register
  - Store address of memory unit

Fetch execute cycle:
 - Basic operation of a computer to fetch instruction from memory and execute them.
 - Steps:
   - PC (program counter) contains the address of the memory location that has the next instruction.
   - Memory's address to copy to MAR (memory address register) through address BUS
   - Instruction content at memory's address that's stored in MAR is copied into MDR (memory data register)
   - Instruction content in MDR is copied to CIR (current instruction register) through data BUS
   - PC (Program counter) increase value to be ready for the next instructions
   - CPU decodes instruction content that's stored in CIR
   - CPU executes the decoded instruction 
   - CPU sends a signal to other components of the computer through control buses
   - Continue next instruction by repeating the cycle
