# RAM and CPU Cache Memory

## Memory Bandwidth

### Non-Temporal Memory Access

Sometimes, we want to read or write cold data (i.e., data that is not expected to be reused soon). However, regular temporal loads or temporal stores can be inefficient in these cases. When performing a temporal load or store, the CPU typically loads the entire cache line containing the target address into the cache, modifies it if needed, and eventually writes it back to memory (in the case of a temporal store). This process can cause cache pollution: loading the cache line may evict warm or hot data (i.e., data likely to be used again soon) from the cache, reducing overall performance.

To avoid this inefficiency, modern processors support non-temporal load and store instructions. These operations allow cold data to be read from or written directly to memory while bypassing the cache hierarchy, preserving valuable cache space for data with higher temporal locality (i.e., data that is likely to be accessed repeatedly within a short period of time).

## Latency numbers

| Operation                     | Time (ns)    | Time (ms) |
|------------------------------|---------------|------------|
| CPU cycle                    | 0.3 - 0.5     |            |
| L1 cache reference           | 1             |            |
| Branch misprediction         | 3             |            |
| L2 cache reference           | 4             |            |
| Mutex lock or unlock            | 17            |            |
| Main memory reference        | 100           |            |
| SSD Random Read              | 17,000        | 0.017      |
| Read 1 MB sequentially from memory | 38,000  | 0.038      |
| Read 1 MB sequentially from SSD    | 622,000 | 0.622      |

Source: https://chandlerc.blog/slides/2023-cppnow-compiler/index.html#/29

## Memory Footprint

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

## Handles Optimization

Pointers occupy 8 bytes on 64-bit systems, which can significantly inflate the size of a struct. As a simple optimization, you can replace pointers with fixed-size handles (commonly u32 indices into an external array or arena). This reduces memory usage, eliminates padding, and ensures consistent layout across platforms.

You're right — the pointer-to-`Bar` example is somewhat artificial unless `Bar` is large or shared. Here’s a more realistic and compelling example:

---

## Handles Optimization

On 64-bit systems, storing raw pointers increases struct size and can introduce unnecessary padding. Replacing pointers with fixed-size handles (typically `u32` indices into a pre-allocated, contiguous data structure such as a vector or arena) reduces memory footprint and improves cache locality, while also simplifying serialization and platform portability.

Consider the following example:

```rust
// Each node references its parent and two children via raw pointers
// On 64-bit systems, this struct takes 24 bytes, while on 32-bit systems, it takes 12 bytes
struct NaiveNode {
    parent: *const NaiveNode,
    left: *const NaiveNode,
    right: *const NaiveNode,
}
```

```rust
// This version uses u32 handles (e.g. indices into a Vec<Node>)
// It takes only 12 bytes on all platforms
struct CompactNode {
    parent: u32,
    left: u32,
    right: u32,
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

## Variant-Encoding Polymorphism Optimization

The main polymorphic data-layout strategies are:
- Tagged Union (AoS of sum-type)
- Subclasses (AoS of concrete types)
- Variant-Encoding (hybrid AoS and SoA)

### Naive tagged union polymorphism

```rust
struct Shape {
    x: f32,
    y: f32,
    kind: ShapeKind,
}

enum ShapeKind {
    Circle { radius: f32 },
    Rectangle { width: f32, height: f32 },
    Triangle { base: f32, height: f32 },
}
```

### Composition polymorphism

```rust
struct Shape {
    tag: Tag,
    x: f32,
    y: f32,
}

enum Tag {
    Circle,
    Rectangle,
    Triangle,
}

struct Circle {
    base: Shape,
    radius: f32,
}

struct Rectangle {
    base: Shape,
    width: f32,
    height: f32,
}

struct Triangle {
    base: Shape,
    base_len: f32,
    height: f32,
}
```

### Variant-Encoding polymorphism

```rust
struct Shape {
    tag: Tag,
    x: f32,
    y: f32,
    extra_payload_idx: u32,
}

enum Tag {
    Circle,
    Rectangle,
    Triangle,
}

struct CirclePayload {
    radius: f32,
}
struct RectanglePayload {
    width: f32,
    height: f32,
}
struct TrianglePayload {
    base: f32,
    height: f32,
}

// Layout at runtime:
//  - One Vec<Shape>
//  - Additional Vec<CirclePayload>, Vec<RectanglePayload>, and/or Vec<TrianglePayload>
//    indexed by Shape.extra_payload_idx as required

// Example of accessing the extra data:
match shapes[i].tag {
    Tag::Circle      => circle_payloads[shapes[i].extra_payload_idx].radius,
    Tag::Rectangle   => rectangle_payloads[shapes[i].extra_payload_idx],
    Tag::Triangle    => triangle_payloads[shapes[i].extra_payload_idx],
}
```

## Struct size Optimization

### Retract offending fields

#### Example 1: Booleans

```rust
struct Task {
    func: fn() -> (),
    priority: u32,
    is_running: bool,
}
```

- The bool at the end introduces padding to align the struct (up to 7 bytes on 64-bit platforms).
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

#### Example 2: Sparse fields

Pull out any field that's present in only a small fraction of elements into its own `HashMap<entity_idx, data>`. You'll trade a hash lookup (and its cache miss) for large wins in overall memory density.

### Packed structs

Add an attribute `#[repr(packed)]` on top of the struct definition in Rust, so the compiler lays out fields back-to-back with no padding at all.

```rust
#[repr(packed)]
struct Foo {
    a: u8,
    b: i32,
    c: f64,
};
```