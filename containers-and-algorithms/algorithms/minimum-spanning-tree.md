# Minimum Spanning Tree

## Problem

Given an undirected and weighted graph, `graph`, find the minimum spanning tree of `graph`.

## Kruskal's Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(E \log E)$

#### Space Complexity

Worst-case: $O(V + E)$

### Pseudocode

```
import algorithms;

func kruskal_minimum_spanning_tree[V, E](graph: AdjacencyListGraph[V, E]) -> HashSet[E] {
    var minimum_spanning_tree: HashSet[E] = HashSet[E]::new();
    var disjoint_set: ForestDisjointSet[V] = ForestDisjointSet[V]::new();

    for vertex in graph.vertices() {
        disjoint_set.make_set(vertex);
    }

    var edge_list: ArrayList[E] = ArrayList[E]::new();
    for edge in graph.edges() {
        edge_list.add_last(edge);
    }

    algorithms::sort(edge_list, lambda (e1, e2) => e1.weight.compare(e2.weight));

    for edge in edge_list {
        var u: V = edge.from;
        var v: V = edge.to;
        if disjoint_set.find(u) != disjoint_set.find(v) {
            minimum_spanning_tree.add(edge);
            disjoint_set.union(u, v);
        }
    }

    return minimum_spanning_tree;
}
```

## Prim's Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(E \log V)$

#### Space Complexity

Worst-case: $O(V)$

### Pseudocode (Eager Version)

```
import math;

func prim_minimum_spanning_tree[V, E](graph: AdjacencyListGraph[V, E], source: V) -> HashSet[E] {
    var minimum_spanning_tree: HashSet[E] = HashSet[E]::new();
    var index_priority_queue: MinHeapIndexPriorityQueue[V, Int] = MinHeapIndexPriorityQueue[V, Int]::new();
    var parent: HashMap[V, V] = HashMap[V, V]::new();
    var min_weight: HashMap[V, Int] = HashMap[V, Int]::new();
    var visited: HashSet[V] = HashSet[V]::new();

    for vertex in graph.vertices() {
        min_weight.put(vertex, math::MAX_INT);
        index_priority_queue.add(vertex, math::MAX_INT);
    }
    min_weight.put(source, 0);
    index_priority_queue.set(source, 0);

    while !index_priority_queue.is_empty() {
        var u: V = index_priority_queue.remove_top();
        visited.add(u);

        if parent.contains_key(u) {
            var p: V = parent.get(u);
            var e: E = graph.get_edge(p, u);
            minimum_spanning_tree.add(e);
        }

        for neighbour in graph.neighbours(u) {
            if !visited.contains(neighbour) {
                var w: Int = graph.get_edge(u, neighbour).weight;
                if w < min_weight.get(neighbour) {
                    parent.put(neighbour, u);
                    min_weight.put(neighbour, w);
                    index_priority_queue.set_key(neighbour, w);
                }
            }
        }
    }

    return minimum_spanning_tree;
}
```
