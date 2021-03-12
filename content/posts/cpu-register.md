---
title: "CPU Register"
series: "computer architect"
categories: "notes"
summary: "CPU register, execute cycle and addressing modes"
date: 2021-03-11T21:33:00+01:00
draft: false
tags:
- cpu
- register
- computer architect
---

CPU Register:
 - It's high-speed memory storage used by the processor.
 - It's used support for the processor to store, decode and execute computer instruction.
 - It's the fastest memory level in computer architect
 - They support different types of memory register:
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

Addressing modes:
 - Specifies how to calculate the effective memory address of an operand by using information held in registers and/or constants contained within a machine instruction or elsewhere

Immediate mode
   - The operand is an immediate value is stored explicitly in the instruction
   - The "data" is embedded in the instruction itself
   - E.g. loads the immediate value of "3" (data) into register $11
   - E.g. move immediate value "200" (data) in register R0

Register mode
    - Data is stored in register. Or the register contains the value of the operand
    - E.g. register $11 contains value 5

Indirect mode
   - The effective address of the operand is the contents of a register or main memory location, location whose address appears in the instruction
   - e.g. (A) is the operand address = pointer variable
   - e,.g. Store value at location whose address is givin by content of register $1

Base pointer addressing mode
    - Same Indirect mode but support an offset to add to address before lookup

Index mode
    - Instruction contains memory address to access.
   - The address of the operand is obtained by adding to the contents of the general register (called index register) a constant value
   - E.g. Load value of register with address 2000 + (value in index register R5)

Absolute (Direct) Mode
   - The address of the operand is embedded in the instruction code.
