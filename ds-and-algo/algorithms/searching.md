# Searching

## Problem

Given an array of elements, `array` and a target element, `target`, determine if `target` exists in `array`. If it does, return its index; otherwise, return `-1`.

## Linear Search Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(n)$

#### Space Complexity

Worst-case: $O(1)$

### Rust Code

#### Rust Code (Iterative)

```rust
fn iterative_linear_search(array: &[i32], target: i32) -> i32 {
    for (i, &element) in array.iter().enumerate() {
        if element == target {
            return i as i32;
        }
    }
    -1
}
```

#### Rust Code (Recursive)

```rust
fn recursive_linear_search(array: &[i32], target: i32) -> i32 {
    fn helper(array: &[i32], target: i32, i: usize) -> i32 {
        if i >= array.len() {
            return -1;
        }
        if array[i] == target {
            return i as i32;
        }
        recursive_linear_search(array, target, i + 1)
    }
    helper(array, target, 0)
}
```

## Binary Search Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(\log n)$

#### Space Complexity

- Worst-case (iterative): $O(1)$
- Worst-case (recursive): $O(\log n)$

### Rust Code

#### Rust Code (Iterative)

```rust
fn iterative_binary_search(array: &[i32], target: i32) -> i32 {
    let mut low = 0;
    let mut high = array.len().saturating_sub(1);

    while low <= high {
        let mid = low + (high - low) / 2;
        if array[mid] == target {
            return mid as i32;
        } else if array[mid] < target {
            low = mid + 1;
        } else {
            if mid == 0 { break; }
            high = mid - 1;
        }
    }

    -1
}
```

#### Rust Code (Recursive)

```rust
fn recursive_binary_search(array: &[i32], target: i32) -> i32 {
    fn helper(array: &[i32], target: i32, low: usize, high: usize) -> i32 {
        if low > high {
            return -1;
        }
        let mid = low + (high - low) / 2;
        if array[mid] == target {
            return mid as i32;
        } else if array[mid] < target {
            helper(array, target, mid + 1, high)
        } else {
            if mid == 0 { return -1; }
            helper(array, target, low, mid - 1)
        }
    }

    if array.is_empty() {
        -1
    } else {
        helper(array, target, 0, array.len() - 1)
    }
}
```

