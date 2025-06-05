# Data Parallelism

## Idea

### Basics

SIMD is a form of data parallelism in which a single instruction operates on multiple data values simultaneously. This is achieved using special registers called _vector registers_, or _vector_ for short, which hold multiple values at once. Operations performed on these registers are called _vector operations_. Each individual value within a vector register is referred to as a _lane_.

### SIMD Instruction Set Extensions

SIMD instruction set extensions provide additional support for vector operations using vector registers. The instruction set architecture defines which SIMD instruction set extensions are possible, but it is up to the specific processor implementation to decide which of those extensions to support, and which vector widths of the chosen SIMD extension it will implement. As a result, different CPUs using the same ISA may support different SIMD extensions and vector widths. The most common SIMD instruction set extensions for x86-64 are SSE (SSE 1 to 4), AVX, AVX2, and the most recent, AVX-512, while for AArch64 it is NEON, with more advanced optional extensions like SVE and SVE2.

| **ISA** | **SIMD Instruction Set Extension** | **First-Class Native Vector Widths**                       |
| ------- | ---------------------------------- | ---------------------------------------------------------- |
| x86-64  | SSE, SSE2, SSE3, SSE4.1, SSE4.2    | 128 bits (fixed)                                           |
| x86-64  | AVX                                | 256 bits (fixed, float only)                               |
| x86-64  | AVX2                               | 256 bits (fixed, int + float)                              |
| x86-64  | AVX-512                            | 512 bits (fixed)                                           |
| AArch64 | NEON                               | 64 bits (fixed), 128 bits (fixed)                          |
| AArch64 | SVE                                | 128–2048 bits (scalable in 128-bit increments)             |
| AArch64 | SVE2                               | 128–2048 bits (scalable in 128-bit increments, more types) |

> **Note**\
> Each generation of x86-64 SIMD extensions builds upon the previous one. When using narrower vector operations from older instruction sets, they operate on the lower portion of the wider registers introduced in newer extensions.

> **Note**\
> To check what SIMD instruction set extensions your CPU supports, run `lscpu`, and look at the "Flags" output.

### Vector Width and Performance

When writing vectorized code, you have two main approaches: use a non-native fixed vector width or adapt to the cpu's native vector width. If you choose a fixed non-native vector width that's smaller than the native registers by using older instruction sets, the hardware will still use its full-width registers but leave some lanes empty. This wastes the processor's parallel potential. If you choose a fixed vector width that's larger than the native registers, the system must combine multiple native registers to simulate the larger width. When too many registers are needed simultaneously, the program may _spill_ — running out of physical registers and having to store data in much slower memory instead.

## Rust API

### Portable SIMD

[`std::simd` module](https://doc.rust-lang.org/std/simd/index.html)

> **Note**\
> Portable SIMD in Rust requires the Rust nightly compiler. To install it, run `rustup toolchain install nightly`, then temporarily set it to the default compiler via `rustup default nightly`

### Vendor SIMD Intrinsics

[`std::arch` module](https://doc.rust-lang.org/std/arch/index.html)

> **Note**\
> Consider using [Dynamic CPU Feature Detection](https://doc.rust-lang.org/std/arch/index.html#dynamic-cpu-feature-detection) instead, since this approach allows:
> 1. The same binary to work on all CPUs
> 2. Automatic use of the fastest available instructions
> 3. Users don't need to worry about compatibility issues
>
> In contrast, the static CPU feature detection approach (using `RUSTFLAGS`) bakes specific SIMD instruction set extensions into the entire binary, which will crash on CPUs that don't support those features.

## Example

### Description

Given a slice of bytes `haystack` and a byte `needle`, find the first occurrence of `needle` in `haystack`.

### Scalar Solution

```rust
fn scalar_find(haystack: &[u8], needle: u8) -> Option<uwidth> {
    haystack.iter().position(|&b| b == needle)
}
```

### SIMD Solution

#### Portable SIMD Solution

```rust
#![feature(portable_simd)]
use std::simd::cmp::SimdPartialEq;
use std::simd::u8x32;

pub fn portable_simd_find(haystack: &[u8], needle: u8) -> Option<usize> {
    const VECTOR_WIDTH: usize = 32;

    // For short strings (less than 16 bytes), use simple iteration instead of SIMD
    if haystack.len() < VECTOR_WIDTH {
        return haystack.iter().position(|&b| b == needle);
    }

    // Perform SIMD on strings of at least 32 bytes.
    //
    // For every 32-byte chunk of the haystack, we create two SIMD vectors: one for the 16-byte
    // chunk of the haystack we're working on, `haystack_vec`, and one 16-byte SIMD vector containing
    // 16-bytes of just `needle`, `needle_vec` such that we can compare each byte against the needle in parallel using `simd_eq`,
    // producing a mask.
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
    // Importantly: the first element of the mask (index 0) maps to the least significant bit (bit 0)
    // in the bitmask, mask[1] maps to bit 1, and so on. This means `bitmask.trailing_zeros()`
    // directly gives us the index of the first `true` in the mask.
    //
    // So if the bitmask is, say, 0b00001000, then the first match was at index 3.
    //
    // Using `bitmask.trailing_zeros()` gives us the index of the first `true` in the mask.
    // We then add the chunk offset to get the actual index in the full haystack.
    let needle_vec = u8x32::splat(needle);

    let mut offset = 0;
    while offset + VECTOR_WIDTH <= haystack.len() {
        let chunk = &haystack[offset..offset + VECTOR_WIDTH];
        let haystack_vec = u8x32::from_slice(chunk);
        let mask = haystack_vec.simd_eq(needle_vec);
        let bitmask = mask.to_bitmask();

        if bitmask != 0 {
            return Some(offset + bitmask.trailing_zeros() as usize);
        }
        offset += VECTOR_WIDTH;
    }

    // Handle remaining bytes (less than 32) that couldn't be processed with SIMD
    haystack[offset..]
        .iter()
        .position(|&b| b == needle)
        .map(|pos| offset + pos)
}
```

#### Intrinsics (NEON) SIMD Solution

```rust
pub fn intrinsics_simd_find(haystack: &[u8], needle: u8) -> Option<usize> {
    const VECTOR_WIDTH: usize = 16;

    // For short strings (less than 16 bytes), use simple iteration instead of SIMD
    if haystack.len() < VECTOR_WIDTH {
        return haystack.iter().position(|&b| b == needle);
    }

    // Process strings of 16+ bytes using SIMD instructions
    //
    // SIMD Strategy:
    // 1. Load 16 bytes of haystack into a SIMD vector
    // 2. Create a SIMD vector filled with 16 copies of the needle byte
    // 3. Compare all 16 positions simultaneously using vceqq_u8
    // 4. This produces a mask where matching positions become 0xFF, non-matches become 0x00
    //
    // Example mask: [0x00, 0x00, 0xFF, 0x00, 0xFF, 0x00, ...]
    //               (no match, no match, MATCH, no match, MATCH, no match, ...)
    //
    // Fast Detection:
    // - Use vmaxvq_u8 to find the maximum value in the mask
    // - If result is 0, no matches found (all bytes were 0x00)
    // - If result is non-zero, at least one match exists (at least one 0xFF present)
    //
    // Finding Exact Position:
    // When matches are found, we need to determine their exact locations:
    // 1. Reinterpret the 16-byte mask as two 64-bit integers for easier processing
    // 2. Check the lower 64 bits (bytes 0-7) and upper 64 bits (bytes 8-15) separately
    // 3. Within each 64-bit section, examine each byte by shifting and masking
    //    - Right-shift by (i * 8) bits to move byte i to the lowest position
    //    - Apply mask 0xFF to isolate just that byte
    //    - Check if the result equals 0xFF (indicating a match)
    //
    // Importantly: We use right bit shifts because of little-endian byte ordering — the byte
    // from haystack position 0 gets packed into the least significant (rightmost) bits of the 64-bit integer,
    // position 1 goes into the next 8 bits, and so on. By shifting right by `i * 8` bits, we move the byte from
    // haystack position `i` into the least significant position where we can extract it with a simple mask.
    let needle_vec = unsafe { vdupq_n_u8(needle) };

    let mut offset = 0;
    while offset + VECTOR_WIDTH <= haystack.len() {
        unsafe {
            let haystack_ptr = haystack.as_ptr().add(offset);
            let haystack_vec = vld1q_u8(haystack_ptr);

            let byte_mask = vceqq_u8(haystack_vec, needle_vec);

            if vmaxvq_u8(byte_mask) != 0 {
                let mask_as_u64x2 = vreinterpretq_u64_u8(byte_mask);
                let low_bits = vgetq_lane_u64(mask_as_u64x2, 0);
                let high_bits = vgetq_lane_u64(mask_as_u64x2, 1);

                if low_bits != 0 {
                    for i in 0..8 {
                        if ((low_bits >> (i * 8)) & 0xFF) == 0xFF {
                            return Some(offset + i);
                        }
                    }
                } else {
                    for i in 0..8 {
                        if ((high_bits >> (i * 8)) & 0xFF) == 0xFF {
                            return Some(offset + 8 + i);
                        }
                    }
                }
            }
        }
        offset += VECTOR_WIDTH;
    }

    // Handle remaining bytes (less than 16) that couldn't be processed with SIMD
    haystack[offset..]
        .iter()
        .position(|&b| b == needle)
        .map(|pos| offset + pos)
}
```
