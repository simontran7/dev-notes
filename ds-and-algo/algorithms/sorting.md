# Sorting

## Problem

Given an array `array`, sort it in increasing order.

## Bubble Sort Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(n^2)$

#### Space Complexity

Worst-case: $O(1)$

### Properties

- Stable
- In-place

### Rust Code

```rust
pub fn bubble_sort(array: &mut [i32]) {
    let n = array.len();
    for i in 0..n {
        let mut swapped = false;
        for j in 0..n - i - 1 {
            if array[j] > array[j + 1] {
                array.swap(j, j + 1);
                swapped = true;
            }
        }
        if !swapped {
            break;
        }
    }
}
```

## Selection Sort Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(n^2)$

#### Space Complexity

Worst-case: $O(1)$

### Properties

- Not stable
- In-place

### Rust Code

```rust
pub fn selection_sort(array: &mut [i32]) {
    let n = array.len();
    for i in 0..n {
        let mut min_index = i;
        for j in (i + 1)..n {
            if array[j] < array[min_index] {
                min_index = j;
            }
        }
        if min_index != i {
            array.swap(i, min_index);
        }
    }
}
```

## Insertion Sort Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(n^2)$

#### Space Complexity

Worst-case: $O(1)$

### Properties

- Stable
- In-place

### Rust Code

```rust
pub fn insertion_sort(array: &mut [i32]) {
    let n = array.len();
    for i in 1..n {
        let current = array[i];
        let mut j = i;
        while j > 0 && array[j - 1] > current {
            array[j] = array[j - 1];
            j -= 1;
        }
        array[j] = current;
    }
}
```

## Merge Sort Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(n \log n)$

#### Space Complexity

Worst-case: $O(1)$

### Properties

- Stable
- Not in-place

### Rust Code

```rust
pub fn merge_sort(array: &[i32]) -> Vec<i32> {
    if array.len() <= 1 {
        return array.to_vec();
    }

    let mid = array.len() / 2;
    let left = merge_sort(&array[..mid]);
    let right = merge_sort(&array[mid..]);
    merge(&left, &right)
}

fn merge(left: &[i32], right: &[i32]) -> Vec<i32> {
    let mut result = Vec::with_capacity(left.len() + right.len());
    let mut i = 0;
    let mut j = 0;

    while i < left.len() && j < right.len() {
        if left[i] < right[j] {
            result.push(left[i]);
            i += 1;
        } else {
            result.push(right[j]);
            j += 1;
        }
    }

    result.extend_from_slice(&left[i..]);
    result.extend_from_slice(&right[j..]);

    result
}
```

## Randomized Quick Sort Algorithm

### Computational Complexity

#### Time Complexity

- Worst-case: $O(n^2)$
- Average-case: $O(n \log n)$

#### Space Complexity

- Worst-case: $O(n^2)$
- Average-case: $O(n \log n)$

### Properties

- Not stable
- In-place

### Rust Code

#### Rust Code (With Lomuto's Partition Scheme)

```
import random;

func quick_sort(array: Array[Int], low: Int, high: Int) {
    func helper(array: Array[Int], low: Int, high: Int) {
        if low < high {
            var pi: Int = lomuto_partition(array, low, high);
            quick_sort(array, low, pi - 1);
            quick_sort(array, pi + 1, high);
        }
    }

    func lomuto_partition(array: Array[Int], low: Int, high: Int) -> Int {
        var pivot: Int = lomuto_pivot_selection(array, low, high);

        var i: Int = low - 1;

        for j in low..high {
            if array[j] <= pivot {
                i += 1;
                array.swap(i, j);
            }
        }

        array.swap(i + 1, high);

        return i + 1;
    }

    func lomuto_pivot_selection(array: Array[Int], low: Int, high: Int) -> Int {
        var i: Int = low + random::int(0, high - low + 1);
        array.swap(i, high);
        return array[high];
    }
}
```

#### Rust Code (With Hoare's Partition Scheme)

```
import random;

func quick_sort(array: Array[Int], low: Int, high: Int) {
    if low >= high {
        return;
    }
    var pi: Int = hoare_partition(array, low, high);
    quick_sort(array, low, pi);
    quick_sort(array, pi + 1, high);
}

func hoare_partition(array: Array[Int], low: Int, high: Int) -> Int {
    var pivot: Int = hoare_pivot_selection(array, low, high);
    var i: Int = low - 1;
    var j: Int = high + 1;

    while true {
        i += 1;
        while i < high and array[i] < pivot {
            i += 1;
        }

        j -= 1;
        while j > low and array[j] > pivot {
            j -= 1;
        }

        if i >= j {
            return j;
        }

        array.swap(i, j);
    }
}

func hoare_pivot_selection(array: Array[Int], low: Int, high: Int) -> Int {
    var i: Int = low + random::int(0, high - low);
    array.swap(i, low);
    return array[low];
}
```

## Heap Sort Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(n \log n)$

#### Space Complexity

Worst-case: $O(1)$

### Properties

- Not stable
- In-place

### Rust Code

```rust
use std::collections::BinaryHeap;

pub fn heap_sort(array: &mut [i32]) {
    let mut heap = BinaryHeap::from(array.to_vec());
    for i in (0..array.len()).rev() {
        array[i] = heap.pop().unwrap();
    }
}
```
