# Handle-based Indirection

## Background

On 64-bit systems, storing raw pointers increases struct size and can introduce unnecessary padding. Replacing pointers with fixed-size handles (typically `u32` indices, or multiple of 32 bits, into a pre-allocated, contiguous data structure such as a vector or arena) reduces memory footprint and improves cache locality, while also simplifying serialization and platform portability.

## Example

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
    parent: Handle,
    left: Handle,
    right: Handle,
}

// For type safety
struct Handle(u32);
```