---
title: "Rust - Vector capacity increment"
series: "rust"
categories: "rust-internal"
summary: "We talk about how Vector in Rust reallocate, and how much it will be reallocate"
date: 2022-10-23T20:00:00+01:00
draft: false
tags:
- rust
---

Vector is a common data structure to store list of data with growable size.

Internally, Vector store 3 basic fields:
 - a raw pointer pointer to point to first element of the Vector store.
 - `size` is the number of elements in Vector contains.
 - `capacity` is the maximum of elements that Vector can add without reallocating.

```rust
pub struct Vec<T, A: Allocator = Global> {
    buf: RawVec<T, A>,
    len: usize,
}

struct RawVec<T> {
    ptr: NonNull<T>,
    cap: usize,
    ...
}
```

Same with other languages, Rust use `size`, `capacity` and pointer to heap space to support growable size feature.
 - If we push an element to Vector that their `size` is less than `capacity`, new element will be follow last element, `size` will be increase as counter and no allocating occurs.
 - If we push an element to Vector that their `size` is equal to `capacity`, the Vector doesn't have more space to add. It reallocates elements in bigger capacity before add new element.

```
                                    ┌─────┐                                                       
┌────────────┐                      │     │                                                       
│    Ptr     │─────────────────────▶│     │                                                       
├────────────┤                      ├─────┤                                                       
│  Size: 4   │                      │     │                                                       
├────────────┤                      │     │                                                       
│Capacity: 5 │                      ├─────┤                                                       
└────────────┘                      │     │                                                       
                                    │     │                                                       
                                    ├─────┤                                                       
                                    │     │                                                       
                                    │     │                                                       
                                    ├─────┤                                                       
                                                                                                  
                                    │     │                                                       
                                                                                                  
                                    └ ─ ─ ┘                                                       
                                                                                                  
                                                                                                  
                                                                                                  
                                                                                       Deallocate 
                                    ┌─────┐         ┌────────────┐                      ┌ ─ ─ ┐   
┌────────────┐                      │     │         │    Ptr     │───┐  Allocate                  
│    Ptr     │─────────────────────▶│     │         ├────────────┤   │  new space       │     │   
├────────────┤                      ├─────┤         │  Size: 4   │   │   ┌─────┐                  
│  Size: 5   │                      │     │         ├────────────┤   │   │     │        │     │   
├────────────┤                      │     │         │Capacity: 10│   └──▶│     │                  
│Capacity: 5 │                      ├─────┤         └────────────┘       ├─────┤        │     │   
└────────────┘                      │     │                              │     │                  
                                    │     │                              │     │        │     │   
                                    ├─────┤                              ├─────┤                  
                                    │     │                              │     │        │     │   
                                    │     │                              │     │                  
                                    ├─────┤                              ├─────┤        │     │   
                                    │     │                              │     │                  
                                    │     │                              │     │        │     │   
                                    └─────┘                              ├─────┤         ─ ─ ─    
                                                                         │     │                  
                                                                         │     │                  
                                                                         ├─────┤                  
                                                                                                  
                                                                         │     │                  
                                                                                                  
                                                                         │     │                  
                                                                                                  
                                                                         │     │                  
                                                                                                  
                                                                         │     │                  
                                                                                                  
                                                                         │     │                  
                                                                                                  
                                                                         │     │                  
                                                                          ─ ─ ─                   
```

```rust
pub fn push(&mut self, value: T) {
    // Check and reallocate if Vector doesn't have enough capacity for new element
    if self.len == self.buf.capacity() {
        self.buf.reserve_for_push(self.len);
    }

    // Add new element to Vector
    ...
    ...
}
```

During pushing to a Vector that its `size` reached to limit of `capacity`, new space with new capacity is allocated = 2x old capacity = 2x current size.

Minimum of capacity is defined as following table:

| Element's size | Minimum capacity |
|----------------|------------------|
| 1kb            | 8                |
| <= 1024kb      | 4                |
| > 1024kb       | 1                |

```rust
// addition = len in case push()
fn grow_amortized(&mut self, len: usize, additional: usize) -> Result<(), TryReserveError> {
    ...
    let required_cap = len.checked_add(additional).ok_or(CapacityOverflow)?;

    let cap = cmp::max(self.cap * 2, required_cap);
    let cap = cmp::max(Self::MIN_NON_ZERO_CAP, cap);

    let new_layout = Layout::array::<T>(cap);

    // Allocate new layout, update pointer to new heap space and update capacity
    ...
}
```

The code `grow_amortized()` is used for both cases while pushing to capacity Vector, and `reserve(more_capacity)` method. 

The `reserve()` actively expands the capacity of Vector with input argument. But the Vector will decide how much will be grown to avoid re-allocate frequently.
 - If we have current `capacity` is 10, size is `9`, we reserve more 5 elements => new `capacity` = 2x old `capacity` = 20.
 - If we have current `capacity` is 10, size is `9`, we reserve more 20 elements => new `capacity` = current `size` + 20 reserved elements = 29.

Rust also provides a method `reserve_exact()` to force fit to what value they want to reserve, if current `capacity` is less than current `len` + `additonal`
 - If we have current `capacity` is 10, size is `9`, we reserve_exact more 5 elements => new `capacity` = current `size` + 9 reserved elements = 14.
 - If we have current `capacity` is 10, size is `9`, we reserve_exact more 20 elements => new `capacity` = current `size` + 20 reserved elements = 29.

```rust
fn grow_exact(&mut self, len: usize, additional: usize) -> Result<(), TryReserveError> {
    ...

    let cap = len.checked_add(additional).ok_or(CapacityOverflow)?;
    let new_layout = Layout::array::<T>(cap);

    // Allocate new layout, update pointer to new heap space and update capacity
    ...
}
```

