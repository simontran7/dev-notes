# Topological Sorting

## Problem

Given a directed acyclic graph, `dag`, return the topological sort of `dag`.

## Kahn's Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(V + E)$

#### Space Complexity

Worst-case: $O(V)$

### Pseudocode

```
func khan_topological_sort[V, E](dag: AdjacencyListDirectedGraph[V, E]) -> SinglyLinkedList[V] {
    var in_degree: HashMap[V, Int] = HashMap[V, Int]::new();
    var topological_sort: SinglyLinkedList[V] = SinglyLinkedList[V]::new();
    var queue: ArrayQueue[V] = ArrayQueue[V]::new();

    for vertex in dag.vertices() {
        in_degree[vertex] = 0;
    }

    for vertex in dag.vertices() {
        for neighbour in dag.neighbours(vertex) {
            in_degree[neighbour] += 1;
        }
    }

    for vertex, degree in in_degree.entries() {
        if degree == 0 {
            queue.enqueue(vertex);
        }
    }

    while !queue.is_empty() {
        var vertex = queue.dequeue();
        topological_sort.add_last(vertex);

        for neighbour in dag.neighbours(vertex) {
            in_degree[neighbour] = in_degree[neighbour] - 1;
            if in_degree[neighbour] == 0 {
                queue.enqueue(neighbour);
            }
        }
    }

    if topological_sort.len() != graph.vertices().len() {
        return null;
    }

    return topological_sort;
}
```

## Depth-First Search Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(V + E)$

#### Space Complexity

Worst-case: $O(V)$

### Pseudocode

```
func dfs_topological_sort[V, E](dag: AdjacencyListDirectedGraph[V, E]) -> SinglyLinkedList[V] {
    var topological_sort: SinglyLinkedList[V] = SinglyLinkedList[V]::new();
    var visited: HashSet[V] = HashSet[V]::new();

    for vertex in dag.vertices() {
        if !visited.contains(vertex) {
            dfs(dag, vertex, visited, topological_sort);
        }
    }

    return topological_sort;
}

func dfs[V, E](dag: AdjacencyListDirectedGraph[V, E], vertex: V, visited: HashSet[V], topological_sort: SinglyLinkedList[V]) {
    visited.add(vertex);
    for neighbour in dag.neighbours(vertex) {
        if !visited.contains(neighbour) {
            dfs_process(dag, neighbour, visited, topological_sort);
        }
    }
    topological_sort.add_first(vertex);
}
```
