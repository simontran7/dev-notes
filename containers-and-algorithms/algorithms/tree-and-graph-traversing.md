# Tree and graph traversing

## Tree traversing problem

Given the root node of a rooted tree, `root`, traverse the the rooted tree in a pre-order, in-order, post-order, or level-order fashion.

## Tree depth-first search (pre-order, in-order, post-order) algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(n) \\)

#### Space complexity

Worst-case: \\( O(n) \\)

### Pseudocode

#### Pseudocode (pre-order traversal)

```
func recursive_dfs[K](root: BinaryTreeNode[K]) {
    if root == None {
        return;
    }
    // visit e.g. println("{}", root.key);
    recursive_dfs(root.left);
    recursive_dfs(root.right);
}
```

```
func iterative_dfs[K](root: BinaryTreeNode[K]) {
    if root == None {
        return;
    }

    var stack: ArrayStack[BinaryTreeNode[K]] = ArrayStack[BinaryTreeNode[K]]::new();
    var current: BinaryTreeNode[K] = root;

    while !stack.is_empty() or current != None {
        if current != None {
            // visit e.g. println("{}", current.key);
            stack.push(current.right);
            current = current.left;
        } else {
            current = stack.pop();
        }
    }
}
```

#### Pseudocode (in-order traversal)

```
func recursive_dfs[K](root: BinaryTreeNode[K]) {
    if root == None {
        return;
    }
    recursive_dfs(root.left);
    // visit e.g. println("{}", root.key);
    recursive_dfs(root.right);
}
```

```
func iterative_dfs[K](root: BinaryTreeNode[K]) {
    if root == None {
        return;
    }

    var stack: ArrayStack[BinaryTreeNode[K]] = ArrayStack[BinaryTreeNode[K]]::new();
    var current: BinaryTreeNode[K] = root;

    while !stack.is_empty() or current != None {
        if current != None {
            stack.push(current);
            current = current.left;
        } else {
            current = stack.pop();
            // visit e.g. println("{}", current.key);
            current = current.right;
        }
    }
}
```

#### Pseudocode (post-order traversal)

```
func recursive_dfs[K](root: BinaryTreeNode[K]) {
    if root == None {
        return;
    }
    recursive_dfs(root.left);
    recursive_dfs(root.right);
    // visit e.g. println("{}", root.key);
}
```

```
func iterative_dfs[K](root: BinaryTreeNode[K]) {
    if root == None {
        return;
    }

    var stack: ArrayStack[BinaryTreeNode[K]] = ArrayStack[BinaryTreeNode[K]]::new();
    var current: BinaryTreeNode[K] = root;
    var last_visited: BinaryTreeNode[K] = None;

    while !stack.is_empty() or current != None {
        if current != None {
            stack.push(current);
            current = current.left;
        } else {
            var peek_node: BinaryTreeNode[K] = stack.top();

            if peek_node.right != None and peek_node.right != last_visited {
                current = peek_node.right;
            } else {
                // visit e.g. println("{}", peek_node.key);
                last_visited = stack.pop();
                current = None;
            }
        }
    }
}
```

## Tree breadth-first search (level-order) algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(n) \\)

#### Space complexity

Worst-case: \\( O(n) \\)

### Pseudocode

```
func iterative_bfs[K](root: BinaryTreeNode[K]) {
    if root == None {
        return;
    }

    var queue: ArrayQueue[BinaryTreeNode[K]] = ArrayQueue[BinaryTreeNode[K]]::new();
    var queue.enqueue(root);

    while !queue.is_empty() {
        var current: BinaryTreeNode[K] = queue.dequeue();
        // visit e.g. println("{}", current.key);
        if current.left != None {
            queue.enqueue(current.left);
        }
        if current.right != None {
            queue.enqueue(current.right);
        }
    }
}
```

## Graph traversing problem

Given a graph, `graph`, and a source node of `graph`, `source`, traverse `graph` beginning at the `source`.

## Graph depth-first search algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(V + E) \\)

#### Space complexity

Worst-case: \\( O(V) \\)

### Pseudocode

#### Pseudocode (iterative)

```
func iterative_dfs[V, E](graph: AdjacencyListGraph[V, E], source: V) {
    var visited: HashSet[V] = HashSet[V]::new();
    var stack: ArrayStack[V] = ArrayStack[V]::new();

    stack.push(source);
    visited.add(source);

    while !stack.is_empty() {
        var vertex: V = stack.pop();
        if !visited.contains(vertex) {
            visited.add(vertex);
            for neighbour in graph.neighbours(vertex) {
                stack.push(neighbour);
            }
        }
    }
}
```

#### Pseudocode (recursive)

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

## Graph breadth-first search algorithm

### Computational complexity

#### Time complexity

Worst-case: \\( O(V + E) \\)

#### Space complexity

Worst-case: \\( O(V) \\)

### Pseudocode

```
func iterative_bfs[V, E](graph: AdjacencyListGraph[V, E], source: V) {
    var visited: HashSet[V] = HashSet[V]::new();
    var queue: ArrayQueue[V] = ArrayQueue[V]::new();

    queue.enqueue(source);
    visited.add(source);

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
