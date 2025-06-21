# RAM and CPU Cache Memory

## Memory Bandwidth

### Non-Temporal Memory Access

Sometimes, we want to read or write cold data (i.e., data that is not expected to be reused soon). However, regular temporal loads or temporal stores can be inefficient in these cases. When performing a temporal load or store, the CPU typically loads the entire cache line containing the target address into the cache, modifies it if needed, and eventually writes it back to memory (in the case of a temporal store). This process can cause cache pollution: loading the cache line may evict warm or hot data (i.e., data likely to be used again soon) from the cache, reducing overall performance.

To avoid this inefficiency, modern processors support non-temporal load and store instructions. These operations allow cold data to be read from or written directly to memory while bypassing the cache hierarchy, preserving valuable cache space for data with higher temporal locality (i.e., data that is likely to be accessed repeatedly within a short period of time).

## Latency numbers

| Operation                     | Time in ns    | Time in ms |
|------------------------------|---------------|------------|
| CPU cycle                    | 0.3 - 0.5     |            |
| L1 cache reference           | 1             |            |
| Branch misprediction         | 3             |            |
| L2 cache reference           | 4             |            |
| Mutex lock/unlock            | 17            |            |
| Main memory reference        | 100           |            |
| SSD Random Read              | 17,000        | 0.017      |
| Read 1 MB sequentially from memory | 38,000  | 0.038      |
| Read 1 MB sequentially from SSD    | 622,000 | 0.622      |

## Memory Alignment

| Type            | Natural alignment                             | Size (bytes)                                                                                       |
| --------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Primitive       | Equal to its size                             | Depends on the type (`u8` = 1, `f32`/`i32`/`u32` = 4, `f64`/`i64`/`u64` = 8)                      |
| Array | Alignment of its element type | $\text{size of an element} \cdot \text{number of elements in the array}$ |
| Pointer         | Equal to pointer size                         | Determined by the maximum addressable memory on a given architecture (i.e. 8 bytes on 64-bit systems, 4 bytes on 32-bit systems)                                              |
| Tagged union    | Maximum alignment among all fields            | $\text{size of tag} + \text{size of largest variant} + \text{trailing tagged union padding}$                     |
| Struct          | Maximum alignment among all fields            | $\text{size of fields} + \text{internal field padding} + \text{trailing struct padding}$                    |

> **Note (memory layout)**\
> In C, the order of fields of a struct is preserved exactly as written, while in Rust, the compiler _may_ optimize or reorder fields to reduce padding unless you use the `#[repr(C)]` attribute.

## Index as fields Optimization

Pointers take up more space on 64-bit systems, which can significantly increase the size of a struct. In contrast, using fixed-size indices (e.g., `u32`) ensures consistent and compact memory usage across platforms. It also doesn't cost any additional padding.

Consider the following example:

```rust
// On a 64-bit system, this struct is 32 bytes due to 64-bit pointers
// On a 32-bit system, it's 16 bytes
struct NaiveFoo {
    a: *const u32, // pointer
    b: *const u32,
    c: *const u32,
    d: *const u32,
}
```

```rust
// This struct is 16 bytes on both 64-bit and 32-bit systems
// because it uses fixed-size indices instead of pointers
struct EfficientFoo {
    a: u32, // index into an external array
    b: u32,
    c: u32,
    d: u32,
}
```

## Struct of Arrays (SoA) Optimization

Struct of Arrays is an optimization where you store each field in its own contiguous array (can be as the struct's field, a global variable, or a local variable), which provides:
- Low memory usage since it eliminates per-element padding in a AoS layout, and instead, you only have padding for array fields in a SoA layout
- Good memory bandwidth utilization for batched code (i.e. if you have a loop that process several objects, but only accesses a few of the fields, then the SoA layout reduces the amount of data that needs to be loaded).

```rust
struct <name> {
    <field 1>: Vec<<field type>>,
    <field 2>: Vec<<field type 2>>,
    <...>: Vec<...>,
    <field N>: Vec<<field type N>>,
}
```

Implementation: https://brevzin.github.io/c++/2025/05/02/soa/, https://github.com/ziglang/zig/blob/master/lib/std/multi_array_list.zig

## Variant-Encoding Polymorphism Optimization

The main polymorphic data-layout strategies are:
- Tagged union (AoS of sum-type)
- Subclasses (AoS of concrete types)
- Variant-Encoding (hybrid AoS and SoA)

### Naive tagged union polymorphism

```rust
struct Monster {
    x: u32,
    y: u32,
    kind: MonsterKind,
}

enum MonsterKind {
    Bee {
        color: BeeColor,
    },
    Human {
        hat: u32,
        shoes: u32,
        shirt: u32,
        pants: u32,
        has_braces: bool,
    },
}
enum BeeColor {
    Yellow,
    Black,
    Red,
}
```

### Composition polymorphism

```rust
struct Monster {
    tag: Tag,
    x: u32,
    y: u32,
}
enum Tag {
    Bee,
    Human,
}

struct Bee {
    base: Monster,
    color: BeeColor,
}
enum BeeColor {
    Yellow,
    Black,
    Red,
}

struct Human {
    base: Monster,
    hat:    u32,
    shoes:  u32,
    shirt:  u32,
    pants:  u32,
    braces: bool,
}
```

### Variant-Encoding polymorphism

```rust
struct Monster {
    tag:         Tag,
    x:           u32,
    y:           u32,
    extra_payload_idx: u32,
}
enum Tag {
    BeeYellow, BeeBlack, BeeRed,
    HumanNaked, HumanBracedNaked,
    HumanClothed, HumanBracedClothed,
}

struct BeePayload {
    color: Color,
}
enum Color {
    Yellow,
    Black,
    Red,
}
struct HumanNakedPayload {
    braces: bool,
}
struct HumanClothedPayload {
    hat:    u32,
    shoes:  u32,
    shirt:  u32,
    pants:  u32,
}

// Usage:
// - AoS for common fields: one array `[Monster]`
// - SoA for extras: one array per variant, indexed by `extra_index`
//
// At runtime, when you want the extra data for `monsters[i]`:
//
// match monsters[i].tag {
//     Tag::BeeRed => bee_payloads[monsters[i].extra_payload_idx].color,
//     Tag::HumanClothed => human_clothed_payloads[monsters[i].extra_payload_idx],
//     // ...
// }
```

> **Note**\
> The encoding approach isn’t a pure SoA, but a hybrid: you store the shared fields in an AoS layout, and keep each variant’s extra data in separate arrays (an SoA) indexed by the main struct.

## Struct size Optimization

### Out-of-Band Booleans

```rust
struct Monster {
    animation: *Animation,
    hp: u32,
    y: u32,
    alive: bool,
}
```

7 bytes of additional padding are required just because of the bool!

Instead, have two `ArrayList`'s containing the Monster's that are alive, and those that are dead.

```rust
struct Monster {
    animation: *const Animation,
    hp: u32,
    y: u32,
    alive: bool,
}

let mut alive_monsters: Vec<Monster> = Vec::new();
let mut dead_monsters: Vec<Monster> = Vec::new();
```

An additional benefit is that iterating over monsters now is faster too, because CPU cache lines will no longer be evicted to make room for dead monsters that we don't care about had we stored all the monsters in a single array and checked whether `alive` was `true` or `false`.

### Store sparse data in a HashMap's

Pull out any field that’s present in only a small fraction of elements into its own `HashMap<entity_index, data>`. You’ll trade a hash lookup (and its cache miss) for big wins in overall memory density



### Use compiler directives to produce packed structs

Adds an attribute (`#[repr(packed)]` in Rust, `__attribute__((packed))` in C) so the compiler lays out fields back-to-back with no padding at all.


## Memory Allocators Optimization

TODO: https://www.gingerbill.org/series/memory-allocation-strategies/
