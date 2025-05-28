# Sorting

## Problem

Given an array `array`, sort it in increasing order.

## Bubble sort algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(n^2) \\)

#### Space complexity

Worst-case: \\( O(1) \\)

### Properties

- Stable
- In-place

### Pseudocode

```
func bubble_sort(array: Array[Int]) {
    var swapped: Bool = False;
    for i in 0..(array.len() - 1) {
        for j in 0..(array.len() - i - 1) {
            if array[j] > array[j + 1] {
                array.swap(j, j + 1);
                swapped = True;
            }
        }
        if !swapped {
            break;
        }
    }
}
```

## Selection sort algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(n^2) \\)

#### Space complexity

Worst-case: \\( O(1) \\)

### Properties

- Not stable
- In-place

### Pseudocode

```
func selection_sort(array: Array[Int]) {
    for i in 0..(array.len() - 1) {
        var min_index: Int = i;
        for j in (i + 1)..array.len() {
            if array[j] < array[min_index] {
                min_index = j;
            }
        }
        if min_index != i {
            array.swap(min_index, i);
        }
    }
}
```

## Insertion sort algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(n^2) \\)

#### Space complexity

Worst-case: \\( O(1) \\)

### Properties

- Stable
- In-place

### Pseudocode

```
func insertion_sort(array: Array[Int]) {
    for i in 1..array.len() {
        var current: Int = array[i];
        var j: Int = i - 1;
        while j >= 0 and array[j] > current {
            array[j + 1] = array[j];
            j -= 1;
        }
        array[j + 1] = current;
    }
}
```

## Merge sort algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(n \log n) \\)

#### Space complexity

Worst-case: \\( O(1) \\)

### Properties

- Stable
- Not in-place

### Pseudocode

```
func merge_sort(array: Array[Int]) -> Array[Int] {
    if array.len() <= 1 {
        return array;
    }

    var mid: Int = array.len() / 2;
    left: Array[Int] = array[0..mid];
    right: Array[Int] = array[mid..array.len()];

    left = merge_sort(left);
    right = merge_sort(right);

    return merge(left, right);
}

func merge(left: Array[Int], right: Array[Int]) -> Array[Int] {
    var left_index: Int = 0;
    var right_index: Int = 0;
    var merged: ArrayList[Int] = ArrayList[Int]::new();

    while left_index < left.len() and right_index < right.len() {
        if left[left_index] < right[right_index] {
            merged.add_last(left[left_index]);
            left_index += 1;
        } else {
            merged.add_last(right[right_index]);
            right_index += 1;
        }
    }

    while left_index < left.len() {
        merged.add_last(left[left_index]);
        left_index += 1;
    }
    while right_index < right.len() {
        merged.add_last(right[right_index]);
        right_index += 1;
    }

    return merged;
}
```

## Randomized quick sort algorithm

### Computational complexity

#### Time complexity

- Worst-case: \\( O(n^2) \\)
- Average-case: \\( O(n \log n) \\)

#### Space complexity

- Worst-case: \\( O(n^2) \\)
- Average-case: \\( O(n \log n) \\)

### Properties

- Not stable
- In-place

### Pseudocode

#### Pseudocode (randomized quick sort with Lomuto's partition scheme)

```
import random;

func quick_sort(array: Array[Int], low: Int, high: Int) {
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
```

#### Pseudocode (randomized quick sort with Hoare's partition scheme)

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

    while True {
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

## Heap sort algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(n \log n) \\)

#### Space complexity

Worst-case: \\( O(1) \\)

### Properties

- Not stable
- In-place

### Pseudocode

```
func heap_priority_queue_sort(array: Array[Int]) {
    var pq: MaxHeapPriorityQueue[Int] = MaxHeapPriorityQueue[Int]::from(array);
    for i in (0..pq.len()).rev() {
        array[i] = pq.remove_top();
    }
}
```
