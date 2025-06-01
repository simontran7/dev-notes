# Strongly Connected Components

## Problem

Given a directed graph, `digraph`, find all the strongly connected components (SCCs) of `digraph`.

## Kosajaru's Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(V + E)$

#### Space Complexity

Worst-case: $O(V)$

### Pseudocode

```
func kosaraju_scc[V, E](digraph: AdjacencyListDirectedGraph[V, E]) -> ArrayList[ArrayList[V]] {
    var visited: HashSet[V] = HashSet[V]::new();
    var stack: ArrayStack[V] = ArrayStack[V]::new();

    for vertex in digraph.vertices() {
        if !visited.contains(vertex) {
            dfs_finishing_time(digraph, vertex, visited, stack);
        }
    }

    var transpose_digraph: AdjacencyListDirectedGraph[V, E] = transpose_graph(digraph);

    visited.clear();
    var result: ArrayList[ArrayList[V]] = ArrayList[ArrayList[V]]::new();
    while !stack.is_empty() {
        var vertex: V = stack.pop();
        if !visited.contains(vertex) {
            var component: ArrayList[V] = ArrayList[V]::new();
            dfs_collect_scc(transpose_digraph, vertex, visited, component);
            result.add_last(component);
        }
    }

    return result;
}

func dfs_finishing_time[V, E](digraph: AdjacencyListDirectedGraph[V, E], vertex: V, visited: HashSet[V], stack: ArrayStack[V]) {
    visited.add(vertex);

    for neighbour in digraph.neighbours(vertex) {
        if !visited.contains(neighbour) {
            dfs_finishing_time(digraph, neighbour, visited, stack);
        }
    }

    stack.push(vertex);
}

func transpose_graph[V, E](digraph: AdjacencyListDirectedGraph[V, E]) -> AdjacencyListDirectedGraph[V, E] {
    var transpose_digraph: AdjacencyListDirectedGraph[V, E] = AdjacencyListDirectedDiGraph[V, E]::new();

    for vertex in digraph.vertices() {
        transpose_digraph.add_vertex(vertex);
    }

    for vertex in digraph.vertices() {
        for neighbour in digraph.neighbours(vertex) {
            transpose_digraph.add_edge(neighbour, vertex);
        }
    }

    return transpose_graph;
}

func dfs_collect_scc[V, E](digraph: AdjacencyListDirectedGraph[V, E], vertex: V, visited: HashSet[V], component: ArrayList[V]) {
    visited.add(vertex);
    component.add_last(vertex);

    for neighbour in digraph.neighbours(vertex) {
        if !visited.contains(neighbour) {
            dfs_collect_scc(digraph, neighbour, visited, component);
        }
    }
}
```

## Tarjan's Algorithm

### Computational Complexity

#### Time Complexity

Worst-case: $O(V + E)$

#### Space Complexity

Worst-case: $O(V)$

### Pseudocode

```
func tarjan_scc[V, E](digraph: AdjacencyListDirectedGraph[V, E]) -> ArrayList[ArrayList[V]] {
    var index: Int = 0;
    var visited: HashMap[V, Int] = HashMap[V, Int]::new();
    var lowlink: HashMap[V, Int] = HashMap[V, Int]::new();
    var on_stack: HashSet[V] = HashSet[V]::new();
    var stack: ArrayStack[V] = ArrayStack[V]::new();
    var result: ArrayList[ArrayList[V]] = ArrayList[ArrayList[V]]::new();

    for vertex in digraph.vertices() {
        if !indices.contains_key(vertex) {
            dfs_scc_detect(digraph, vertex, index, indices, lowlink, stack, on_stack, result);
        }
    }

    return result;
}

func dfs_scc_detect[V, E](digraph: AdjacencyListDirectedGraph[V, E], vertex: V, index: Int, indices: HashMap[V, Int], lowlink: HashMap[V, Int], stack: ArrayStack[V], on_stack: HashSet[V], result: ArrayList[ArrayList[V]]) {
    indices[vertex] = index;
    lowlink[vertex] = index;
    index = index + 1;

    stack.push(vertex);
    on_stack.add(vertex);

    for neighbour in digraph.neighbours(vertex) {
        if !indices.contains_key(neighbour) {
            dfs_scc_detect(neighbour, digraph, index, indices, lowlink, stack, on_stack, result);
            lowlink[vertex] = min(lowlink[vertex], lowlink[neighbour]);
        } else if on_stack.contains(neighbour) {
            lowlink[vertex] = min(lowlink[vertex], indices[neighbour]);
        }
    }

    if lowlink[vertex] == indices[vertex] {
        var component: ArrayList[V] = ArrayList[V]::new();
        while true {
            var w: V = stack.pop();
            on_stack.remove(w);
            component.add_last(w);

            if w == vertex {
                break;
            }
        }

        result.add_last(component);
    }
}
```
