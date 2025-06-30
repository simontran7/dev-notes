# Handle-based Indirection

## Background

On 64-bit systems, using raw pointers can bloat struct sizes and introduce unnecessary padding. A more efficient strategy is to allocate arrays of items—typically wrapped in a struct—and return handles instead of raw pointers. A _handle_ is an abstract reference to a resource, often represented as a compact integer, usually 32 bits (or a multiple thereof), that encodes:
- A unique tag in the upper bits to identify or validate the owning array.
- An index in the lower bits that locates the specific slot within the array (generally consuming more bits than the tag).

```
| Unique Tag | Index into array |
| high bits  |    low bits      |
```

This indirection reduces memory usage compared to storing raw pointers (particularly when handles are smaller than native pointers), improves cache locality by relying on arrays instead of pointers, and greatly simplifies serialization and cross-platform portability, since handles are fixed-size integers rather than raw memory addresses.

## Example

```rust
/// Each node stores raw pointers
/// 24 bytes on 64-bit, 12 bytes on 32-bit
pub struct NaiveNode {
    pub parent: *const NaiveNode,
    pub left: *const NaiveNode,
    pub right: *const NaiveNode,
}
```

```rust
/// Each node references its parent and two children via handles
/// Always 12 bytes regardless of platform (3 x u32)
pub struct CompactNode {
    pub parent: Handle,
    pub left: Handle,
    pub right: Handle,
}

#[derive(Copy, Clone, Debug, PartialEq, Eq, Hash)]
struct Handle(u32);

// Number of bits for the kind (5 bits allows up to 32 kinds)
const KIND_MASK: u32 = 0b11111 << 27 // shifts all bits 27 places to the left in 00000000000000000000000000011111
// Remaining 27 bits for the index (up to ~134 million entries)
const INDEX_MASK: u32 = 07FF_FFFF // 00000111111111111111111111111111 in binary

#[repr(u8)]
#[derive(Copy, Clone, Debug, PartialEq, Eq, Hash)]
pub enum NodeKind {
    Kind1 = 0,
    Kind2,
    Kind3,
    // Add more kinds as needed, up to 32
}

impl Handle {
    /// Create a new handle from a kind and an index
    pub fn new(kind: NodeKind, index: u32) -> Self {
        debug_assert!(
            index <= INDEX_MASK,
            "Index overflow: max index is {}",
            INDEX_MASK
        );
        let encoded = (index & INDEX_MASK) | ((kind as u32) << 27);
        Self(encoded)
    }

    /// Extract the kind
    pub fn kind(self) -> Option<NodeKind> {
        match (self.0 >> 27) & 0b11111 {
            0 => Some(NodeKind::Kind1),
            1 => Some(NodeKind::Kind2),
            2 => Some(NodeKind::Kind3),
            _ => None,
        }
    }

    /// Extract the index
    pub fn index(self) -> usize {
        (self.0 & INDEX_MASK) as usize
    }
}
```