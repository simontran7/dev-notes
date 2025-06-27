# Data Structure Packing

## Background

Natural alignment refers to the memory address boundary that a data structure must start at to ensure efficient CPU access. You can check a data structure's alignment using `std::mem::align_of`. In structs or tagged unions, the compiler often inserts padding — extra unused bytes — so that each field starts at its required aligned address. This ensures that the layout adheres to the alignment rules of each field. The size of a data structure, which includes both actual data and any padding, is the total number of bytes it occupies in memory. You can check the size of a data structure using `std::mem::size_of`.

The following is a table of common types and their natural alignment and size in bytes.

| Type            | Natural Alignment (bytes)                            | Size (bytes)                                                                                       |
| --------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Primitive       | Usually equal to its size                     | Depends on the type ( `bool` = 1, `u8` / `i8` = 1, `u16` / `i16` = 2, `char` = 4, `f32` / `i32` / `u32` = 4, `f64` / `i64` / `u64` = 8, `u128` / `i128` = 16, `usize` / `isize` = determined by the maximum addressable memory on a given architecture (i.e. 8 bytes on 64-bit systems, 4 bytes on 32-bit systems))                      |
| Array / Slice | Alignment of its element type | $\text{size of an element} \cdot \text{number of elements in the array}$ |
| Pointer         | Equal to pointer size                         | Determined by the maximum addressable memory on a given architecture (i.e. 8 bytes on 64-bit systems, 4 bytes on 32-bit systems)                                              |
| Tagged union    | Maximum alignment among all fields            | $\text{size of tag} + \text{size of largest variant} + \text{trailing padding}$                     |
| Struct          | Maximum alignment among all fields            | $\text{size of fields} + \text{internal field padding} + \text{trailing padding}$                    |

> **Note (Memory Layout)**\
> In C, the order of fields of a struct is preserved exactly as written, while in Rust, the compiler _may_ reorder fields to reduce padding unless you use the `#[repr(C)]` attribute.

## Optimization 1: Retract Offending Fields

### Example: Booleans

```rust
struct Task {
    func: fn() -> (),
    priority: u32,
    is_running: bool,
}
```

- The boolean at the end introduces padding to align the struct (up to 7 bytes on 64-bit platforms).
- Every iteration over tasks must check `is_running`

```rust
struct Task {
    func: fn() -> (),
    priority: u32,
}

let mut running_tasks: Vec<Task> = Vec::new();
let mut waiting_tasks: Vec<Task> = Vec::new();
```

- No boolean = no padding
- Faster iteration over `running_tasks`, no branch on status
- Struct is tightly packed and cache-friendly

## Optimization 2: Compiler Packed Structs

Add an attribute `#[repr(packed)]` on top of the struct definition in Rust, so the compiler lays out fields back-to-back with no padding at all.

```rust
#[repr(packed)]
struct Foo {
    a: u8,
    b: i32,
    c: f64,
};
```