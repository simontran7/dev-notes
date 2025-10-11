# Handles

## Background

**Handles** are integers as form of indirection, similiar to [tagged pointers](https://en.wikipedia.org/wiki/Tagged_pointer). They have notable benefits over raw pointers: 
1. Stable: Handles remains valid even if the underlying resource can be moved, swapped, or reallocated, while a pointer would be invalidated in the same scenario.
2. Compact Representation: Handles can be smaller than native pointers. Notably, if you know your memory pool is less than 4 GiB, a `u32` handle is enough, thereby saving memory.
3. Can Cross Abstraction Boundaries: Pointers are tied to one process, and therefore, one address space. Handles can be shared across threads, modules, or even across processes. The receiver only needs to know the handle format, not the actual memory layout.
4. Control: On top of storing the index, handles can also encode metadata (e.g., generation counter, the underlying array within an SoA, etc.). 

## Implementation 

```rust
struct Handle(u32);

impl Handle {}
```
