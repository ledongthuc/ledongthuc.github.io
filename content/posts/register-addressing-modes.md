---
title: "CPU Register addressing modes"
series: "computer architect"
categories: "notes"
summary: "CPU register addressing modes"
date: 2021-03-11T21:33:00+01:00
draft: false
tags:
- cpu
- register
- computer architect
---

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
