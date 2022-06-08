---
title: "Jump table (branch table)"
series: "software development"
categories: "notes"
summary: "The jump table is a method to transfer program control to another code block using a table of branch or jump instructions."
date: 2022-06-08T21:00:00+01:00
draft: false
socialImage: "https://thuc.space/images/jump_table/jump_table-jump_table.png"
tags:
- switch optimize
- jump table
- branch table
---

# Jump table

The jump table is a method to transfer program control to another code block using a table of branch or jump instructions.

![jump table](https://thuc.space/images/jump_table/jump_table-jump_table.png)


It's a table of code addresses, that can be indexed by selector value. We can use a selector to map to a code address and jump to the code block.

Approximate translation:
```
target = JumpTable[selector];
goto target;
```

The jump table can be used by the compiler to optimize switch-case in many programming languages.
Each switch case is defined as its memory address in the table jump. 

![switch jump table](https://thuc.space/images/jump_table/jump_table-switch-case.png)

The selector can be calculated by data (input of switch-case) and transformed into an offset that is used at the selector in the jump table. The transform can be multiplying or shifting.

```
selector = jump-table-address + offset
offset = option-index * memory
option-index = transform(data)
```

If a static translate table is used, this multiplying can be performed manually or by the compiler, without any run time cost.

References:
 - https://courses.cs.washington.edu/courses/cse351/17sp/lectures/CSE351-L10-asm-IV_17sp-ink.pdf
 - https://www.nesdev.org/wiki/Jump_table
 - http://www.cs.umd.edu/~waa/311-F09/jumptable.pdf
 - https://sites.google.com/site/arch1utep/jump-tables
 - https://en.wikipedia.org/wiki/Branch_table
