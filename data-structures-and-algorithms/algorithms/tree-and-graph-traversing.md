# Tree and Graph Traversing

## Tree Traversing Problem

Given the root node of a rooted tree, `root`, traverse the the rooted tree in a pre-order, in-order, post-order, or level-order fashion.

## Tree Depth-First Search Algorithm (Pre-order, In-order, Post-order)

### Computational Complexity

#### Time Complexity

Worst-case: $O(n)$

#### Space Complexity

Worst-case: $O(n)$

### Pseudocode

#### Pseudocode (Pre-order)

```
func recursive_preorder_dfs[K](root: BinaryTreeNode[K]) {
    if root == null {
        return;
    }

    // visit e.g. println("{}", root.key);

    recursive_preorder_dfs(root.left);
    recursive_preorder_dfs(root.right);
}
```

```
func iterative_preorder_dfs[K](root: BinaryTreeNode[K]) {
    if root == null {
        return;
    }

    var stack: ArrayStack[BinaryTreeNode[K]] = ArrayStack[BinaryTreeNode[K]]::new();
    stack.push(root);

    while !stack.is_empty() {
        var node = stack.pop();

        // visit e.g. println("{}", node.key);

        if node.right != null {
            stack.push(node.right);
        }
        if node.left != null {
            stack.push(node.left);
        }
    }
}
```

#### Pseudocode (In-order)

```
func recursive_inorder_dfs[K](root: BinaryTreeNode[K]) {
    if root == null {
        return;
    }

    recursive_inorder_dfs(root.left);

    // visit e.g. println("{}", root.key);

    recursive_inorder_dfs(root.right);
}
```

```
func iterative_inorder_dfs[K](root: BinaryTreeNode[K]) {
    if root == null {
        return;
    }

    var stack: ArrayStack[BinaryTreeNode[K]] = ArrayStack[BinaryTreeNode[K]]::new();
    var current: BinaryTreeNode[K] = root;

    while !stack.is_empty() or current != null {
        if current != null {
            stack.push(current);
            current = current.left;
        }

        current = stack.pop();

        // visit e.g. println("{}", current.key);

        current = current.right;
    }
}
```

#### Pseudocode (Post-order)

```
func recursive_postorder_dfs[K](root: BinaryTreeNode[K]) {
    if root == null {
        return;
    }

    recursive_dfs(root.left);
    recursive_dfs(root.right);

    // visit e.g. println("{}", root.key);
}
```

```
func iterative_dfs[K](root: BinaryTreeNode[K]) {
    if root == null {
        return;
    }

    var stack: ArrayStack[BinaryTreeNode[K]] = ArrayStack[BinaryTreeNode[K]]::new();
    var current: BinaryTreeNode[K] = root;
    var last_visited: BinaryTreeNode[K] = null;

    while !stack.is_empty() or current != null {
        if current != null {
            stack.push(current);
            current = current.left;
        } else {
            var peek_node: BinaryTreeNode[K] = stack.top();

            if peek_node.right != null and peek_node.right != last_visited {
                current = peek_node.right;
            } else {
                // visit e.g. println("{}", peek_node.key);
                last_visited = stack.pop();
                current = null;
            }
        }
    }
}
```

## Tree Breadth-First Search Algorithm (Level-order)

### Computational Complexity

#### Time Complexity

Worst-case: $O(n)$

#### Space Complexity

Worst-case: $O(n)$

### Pseudocode

```
func iterative_bfs[K](root: BinaryTreeNode[K]) {
    if root == null {
        return;
    }

    var queue: ArrayQueue[BinaryTreeNode[K]] = ArrayQueue[BinaryTreeNode[K]]::new();
    queue.enqueue(root);

    while !queue.is_empty() {
        var level_width = queue.len();

        for _ in 0..level_width {
            var node: BinaryTreeNode[K] = queue.dequeue();

            // visit e.g. println("{}", node.key);

            if node.left != null {
                queue.enqueue(node.left);
            }
            if node.right != null {
                queue.enqueue(node.right);
            }
        }
    }
}
```

## Graph Traversing Problem

Given a graph, `graph`, and a source vertex of `graph`, `source`, traverse `graph` beginning at `source`.

## Graph Depth-First Search Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(V + E)$

#### Space Complexity

Worst-case: $O(V)$

### Pseudocode

#### Pseudocode (Iterative)

```
func iterative_dfs[V, E](graph: AdjacencyListGraph[V, E], source: V) {
    var visited: HashSet[V] = HashSet[V]::new();
    var stack: ArrayStack[V] = ArrayStack[V]::new();

    stack.push(source);

    while !stack.is_empty() {
        var vertex: V = stack.pop();
        visited.add(vertex)
        for neighbour in graph.neighbours(vertex) {
            if !visited.contains(neighbour) {
                stack.push(neighbour);
            }
        }
    }
}
```

#### Pseudocode (Recursive)

```
func recursive_dfs[V, E](graph: AdjacencyListGraph[V, E], vertex: V, visited: HashSet[V]) {
    visited.add(vertex);
    for neighbour in graph.neighbours(vertex) {
        if !visited.contains(neighbour) {
            recursive_dfs(graph, neighbour, visited);
        }
    }
}
```

## Graph Breadth-First Search Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(V + E)$

#### Space Complexity

Worst-case: $O(V)$

### Pseudocode

```
func iterative_bfs[V, E](graph: AdjacencyListGraph[V, E], source: V) {
    var visited: HashSet[V] = HashSet[V]::new();
    var queue: ArrayQueue[V] = ArrayQueue[V]::new();

    visited.add(source);
    queue.enqueue(source);

    while !queue.is_empty() {
        var vertex: V = queue.dequeue();
        for neighbour in graph.neighbours(vertex) {
            if !visited.contains(neighbour) {
                visited.add(neighbour);
                queue.enqueue(neighbour);
            }
        }
    }
}
```
