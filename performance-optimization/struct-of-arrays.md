# Struct of Arrays (SoA)

## Background

Struct of Arrays is an optimization where you store each field in its own contiguous array (can be as the struct's field, a global variable, or a local variable), which provides:
- Low memory usage since it eliminates per-element padding in a AoS layout, and instead, you only have padding for array fields in a SoA layout
- Better cache locality
- Good memory bandwidth utilization for batched code (i.e. if you have a loop that process several objects, but only accesses a few of the fields, then the SoA layout reduces the amount of data that needs to be loaded).

```rust
struct MySoA {
    field1: Vec<Type1>,
    field2: Vec<Type2>,
    // ...
    fieldN: Vec<TypeN>,
}
```

## Example: Polymorphism using SoA and AoS

### AoS with Enum-based Polymorphism

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

### AoS with Subclass-based (via Composition) Polymorphism

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

### Hybrid AoS and SoA Variant-Encoding-based Polymorphism

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
