# Data parallelism

## Idea

### Basics

SIMD is a form of data parallelism in which a single instruction operates on multiple data values simultaneously. This is achieved using special registers called vector registers, which hold multiple values at once. Operations performed on these registers are called vector operations. Each individual value within a vector register is referred to as a lane.

### Instruct set extensions

SIMD instruction set extensions provide additional support for vector operations using vector registers. The ISA defines which SIMD instruction set extensions are possible, but it is up to the specific processor implementation to decide which of those extensions to support, and which vector lengths of the chosen SIMD extension it will implement. As a result, different processors using the same ISA may support different SIMD extensions and vector lengths.

### Vector length

Match your vector length to your data patterns. If you process varied input lengths, you might need a dynamic approach that selects vector length based on the current input length. This is because large vectors with small inputs leads to frequent fallbacks to scalar operations, hurting performance, while large vectors with large inputs leads ideal performance since the fallback overhead becomes negligible.

## Rust API

### Portable SIMD

[`std::simd` module](https://doc.rust-lang.org/std/simd/index.html)

> **Note**\
> Portable SIMD in Rust requires the Rust nightly compiler. To install it, run `rustup toolchain install nightly`, then temporarily set it to the default compiler via `rustup default nightly`

### Vendor SIMD intrinsics

[`std::arch` module](https://doc.rust-lang.org/std/arch/index.html)

## Example

### Description

Given a string `haystack` and a character to find within the string `needle`, find the first occurrence of `needle` in `haystack`.

### Scalar solution

```rust
fn scalar_find(haystack: &[u8], needle: u8) -> Option<usize> {
    haystack.iter().position(|&c| c == needle)
}
```

### SIMD solution

#### Portable SIMD solution

```rust
#![feature(portable_simd)]
use std::simd::cmp::SimdPartialEq;
use std::simd::u8x32;

fn portable_simd_find(haystack: &[u8], needle: u8) -> Option<usize> {
    // For short strings (less than 32 bytes), fallback to scalar solution.
    if haystack.len() < 32 {
        return haystack.iter().position(|&c| c == needle);
    }

    // Perform SIMD on strings of at least 32 bytes.
    //
    // For every 32-byte chunk of the haystack, we create a SIMD vector and compare
    // each byte against the needle in parallel using `simd_eq`, producing a mask.
    //
    // This SIMD mask is a vector of booleans, like:
    //   [false, false, false, true, false, ...]
    //
    // We convert this mask into a compact bitmask with `to_bitmask()`, which gives us
    // a u32 where each bit corresponds to an entry in the SIMD mask:
    //
    //   - A `true` in the SIMD mask becomes a `1` in the bitmask.
    //   - A `false` becomes a `0`.
    //
    // Importantly: the first element of the mask (index 0) maps to the least significant bit (LSB)
    // in the bitmask. That is, `mask[0]` affects bit 0, `mask[1]` affects bit 1, and so on.
    //
    // So if the bitmask is, say, 0b00001000, then the first match was at index 3.
    //
    // Using `bitmask.trailing_zeros()` gives us the index of the first `true` in the mask.
    // We then add the chunk offset to get the actual index in the full haystack.
    const SIMD_WIDTH: usize = 32;
    let needle_vec = u8x32::splat(needle);

    let mut offset = 0;
    while offset + SIMD_WIDTH <= haystack.len() {
        let chunk = &haystack[offset..offset + SIMD_WIDTH];
        let haystack_vec = u8x32::from_slice(chunk);
        let mask = haystack_vec.simd_eq(needle_vec);
        let bitmask = mask.to_bitmask();

        if bitmask != 0 {
            return Some(offset + bitmask.trailing_zeros() as usize);
        }
        offset += SIMD_WIDTH;
        println!("new offset {offset}")
    }

    // Fallback to the scalar solution for the remainder of the string,
    // which is usually less than 32 bytes
    haystack[offset..]
        .iter()
        .position(|&c| c == needle)
        .map(|pos| offset + pos)
}
```

#### Intrinsic-based SIMD solution

```rust
fn intrinsic_simd_find(haystack: &[u8], needle: u8) -> Option<usize> {
    todo!()
}
```

