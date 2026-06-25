---
title: "Rust - without STD"
series: "rust"
categories: "notes"
summary: "What if we start a rust without stdlib."
date: 2026-06-25T20:00:00+01:00
draft: false
tags:
- rust
---

## #![no_std]
It is an attribute to ask Rust compiler don’t link the standard library into output result.The standard library is OS dependent library, so it’s not fit during kernel developing

## #[panic_handler]
This is an attribute to define the function that the compiler uses to handle logic when panic occurs
Abort on panic
We can define the profile panic is abort to disables the generation of unwinding symbol

## #![no_main]
This is an attribute to define no main function as entrypoint

## #[unsafe(no_mangle)]
This is an attribute to disable name mangling from rust compiler. Normally, the function name in rust will be converted to generated unique name.

## VGA text buffer
This is special memory address located in physical memory 0xB8000. It allows software to control VGA to     render text on the screen using characters from code page 437. It’s presented as two-dimensions array x columns and y rows.
Each element in the 2-dimensions array is 2 bytes. The first byte stores the character and second byte stores the color information. In the second byte, 4 first bits are foreground color (3 color bits and 1 bright bit), next 3 bits are background color, and last bit for blinking.

## Spin Lock
Spin lock is simple synchronization mechanism. The process continuously checks a lock until it’s available. This approach is poor performance because of busy-waiting, but it can be useful in bare system or low-level environments where blocking is not feasible and context switching is costly
