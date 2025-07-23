# Priority Queue ADT

## Interface

## Heap Data Structure

### Computational Complexity

### Rust Code

```rust
pub struct HeapPriorityQueue<E> {
    data: Vec<E>,
}

pub enum PriorityQueueError {
    EmptyPriorityQueue,
}

impl<E: Ord> HeapPriorityQueue<E> {
    pub fn new() -> Self {
        HeapPriorityQueue {
            data: Vec::new(),
        }
    }

    pub fn heapify<T: Into<Vec<E>>>(initial: T) -> Self {
        let mut pq = HeapPriorityQueue {
            data: initial.into(),
        };

        // There is no need to call `heapify()` on an initial array or vector
        // of length 0 or 1. Calling `heapify()` on a list of either length
        // leads to a panic due to integer underflow in `parent()`.
        if pq.data.len() > 1 {
            let root = 0;
            let last_leaf = pq.data.len() - 1;
            for i in (root..=Self::parent(last_leaf)).rev() {
                pq.sift_down(i);
            }
        }

        pq
    }

    pub fn len(&self) -> usize {
        self.data.len()
    }

    pub fn is_empty(&self) -> bool {
        self.data.is_empty()
    }

    pub fn add(&mut self, element: E) {
        self.data.push(element);
        let last_leaf = self.data.len() - 1;
        self.sift_up(last_leaf);
    }

    pub fn top(&self) -> Result<&E, PriorityQueueError> {
        self.data.get(0).ok_or_else(|| PriorityQueueError::EmptyPriorityQueue)
    }

    pub fn remove_top(&mut self) -> Result<E, PriorityQueueError> {
        // Prevents `let last_leaf = self.data.len() - 1` from a integer underflow
        // and allows for a safe `let previous_top_data = self.data.pop().unwrap();`
        if self.data.is_empty() {
            return Err(PriorityQueueError::EmptyPriorityQueue);
        }
        // We can't place `self.data.len() - 1` directly as the second argument
        // to swap, since calling `.swap()` on `self.data` is a mutable borrow, and
        // at the same time, calling `.len()` on `self.data` is an immutable borrow,
        // which is forbidden in Rust.
        let root = 0;
        let last_leaf = self.data.len() - 1;
        self.data.swap(root, last_leaf);
        let previous_top_data = self.data.pop().unwrap();
        self.sift_down(root);
        return Ok(previous_top_data);
    }

    fn sift_up(&mut self, start: usize) {
        let mut i = start;
        while i > 0 && self.data[i] < self.data[Self::parent(i)] {
            self.data.swap(i, Self::parent(i));
            i = Self::parent(i);
        }
    }

    fn sift_down(&mut self, start: usize) {
        let len = self.data.len();
        let mut i = start;
        loop {
            let left = Self::left_child(i);
            let right = Self::right_child(i);
            let mut smallest = i;

            // We must check against `self.data[smallest]` over `self.data[i]`
            // so that we may compare the parent, the left child, and the right child.
            // Otherwise, we wouldn't be able to compare the left and the right child
            // in the scenario where both children are smaller than the parent.
            if left < len && self.data[left] < self.data[smallest] {
                smallest = left;
            }
            if right < len && self.data[right] < self.data[smallest] {
                smallest = right;
            }

            if smallest == i {
                break;
            }

            self.data.swap(smallest, i);
            i = smallest;
        }
    }

    fn parent(i: usize) -> usize {
        return (i - 1) / 2;
    }

    fn left_child(i: usize) -> usize {
        return 2 * i + 1;
    }

    fn right_child(i: usize) -> usize {
        return 2 * i + 2;
    }
}
```