# Searching

## Problem

Given an array of elements, `array` and a target element, `target`, determine if `target` exists in `array`. If it does, return its index; otherwise, return `-1`.

## Linear Search Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(n)$

#### Space Complexity

Worst-case: $O(1)$

### Pseudocode

#### Pseudocode (Iterative)

```
func iter_linear_search(array: Array[Int], target: Int) -> Int {
    for i in 0..array.len() {
        if array[i] == target {
            return i;
        }
    }

    return -1;
}
```

#### Pseudocode (Recursive)

```
func recursive_linear_search(array: Array[Int], target: Int, i: Int) -> Int {
    if array.is_empty() {
        return -1;
    }

    if array[i] == target {
        return i;
    }

    return recursive_linear_search(array, target, i + 1);
}
```

## Binary Search Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(\log n)$

#### Space Complexity

- Worst-case (iterative): $O(1)$
- Worst-case (recursive): $O(\log n)$

### Pseudocode

#### Pseudocode (Iterative)

```
func iterative_binary_search(array: Array[Int], target: Int) -> Int {
    var low: Int = 0;
    var high: Int = array.len() - 1;

    while low <= high {
        var mid: Int = low + (high - low) / 2;
        if array[mid] == target {
            return mid;
        } else if array[mid] < target {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return -1;
}
```

#### Pseudocode (Recursive)

```
func recursive_binary_search(array: Array[Int], target: Int) -> Int {
    func helper(array: Array[Int], target: Int, low: Int, high: Int) -> Int {
        if low > high {
            return -1;
        }
        var mid: Int = low + (high - low) / 2;
        if array[mid] == target {
            return mid;
        }
        if array[mid] < target {
            return helper(array, target, mid + 1, high);
        } 
        return helper(array, target, low, mid - 1); 
    }

    return helper(array, target, 0, array.len() - 1);
}
```

