# Algorithm Techniques

## Classic Two Pointers Technique

### Two Pointers Opposite

#### Use Case

The list input is sorted, and you are looking for a pair (or pairs) that meet a specific condition.

#### Template

```python
def two_pointers_opposite(array):
    left = 0
    right = len(array) - 1
    result = 0

    while left < right:
        # Some logic involving `array[left]` and/or `array[right]`

        if <condition>:
            left += 1
        else:
            right -= 1

    return result
```

### Two Pointers Same Speed

#### Use Case

The two list inputs are sorted, and you want to compare elements between the two.

#### Template

```python
def two_pointers_same_speed(array1, array2):
    i = 0
    j = 0
    result = 0

    while i < len(array1) and j < len(array2):
        # Some logic involving `array1[i]` and `array2[j]`
        if <condition>:
            i += 1
        else:
            j += 1

    while i < len(array1):
        # Some logic involving `array1[i]`
        i += 1

    while j < len(array2):
        # Some logic involving `array2[j]`
        j += 1

    return result
```

## Sliding Window Technique

### Fixed Sliding Window

#### Use Case

You are looking for a subarray or substring of some length `k` that satisfies a certain constraint.

#### Template

```python
def fixed_sliding_window(array, k):
    current = 0
    result = 0

    for i in range(k):
        # Some logic to build first window involving `current` and/or other vars

    for right in range(k, len(array)):
        # Add `array[i]` to window
        # Remove `arr[i - k]` from window
        # Update `result`

    return result
```

### Variable Sliding Window

#### Use Case

You are looking for a subarray or substring (usually the longest) that satisfies a certain constraint.

#### Template

```python
def variable_sliding_window(array):
    left = 0
    current = 0
    result = 0

    for right in range(len(array)):
        # Some logic to add `array[right]` to `current`

        while <broken window condition>:
            # Remove `array[left]` from `current`
            left += 1

        # Update `result`, where current window length
        # at any time is `right - left + 1`

    return result
```

> **Note**\
> The constraint must be preserved as the sliding window shrinks. If shrinking the window can break the constraint, consider using a prefix sum + hash map technique instead.

> **Note**\
> The formula for the length of a window is `right - left + 1` .

## Prefix Sum Technique

### Use Case

You need a subroutine which involves finding the sum of any subarray in *O*(1).

### Template

```python
def prefix_sum_building(nums):
    n = len(nums)
    prefix = [0] * (n + 1)

    for i in range(n):
        prefix[i + 1] = prefix[i] + nums[i]

    return prefix
```

## HashMap and HashSet Technique

### HashMap

#### Use Case

* Track elements seen so far for uniqueness (with any extra info stored as the value)
* Frequency counting
* Basic mapping
* Memoization table
* Count number of subarrays with a constraint that _doesn't_ necessarily hold as you shrink the window

```python
def count_num_subarrays_with_exact_constraint(array, k):
    freq = {0 : 1} # prefix sum frequency map
    prefix_sum = 0
    result = 0

    for num in array:
        # Some logic to change `prefix_sum` as necessary (e.g. prefix_sum += num)
        result += freq.get(prefix_sum - k, 0)
        freq[prefix_sum] += freq.get(prefix_sum, 0) + 1

    return result
```

#### Template

```python
# Create an empty map
my_map = dict()

# Create a map with initial values
my_map = {
    <key 1>: <value 1>,
    <key 2>: <value 2>
}

# Add new entry or update current entry
my_map[<immutable key>] = <value>

# Remove an entry
del my_map[<key>]

# Remove all entries
my_map.clear()

# Get number of entries
len(my_map)

# Check if my_map is empty
not my_map

# Get value
my_map[<key>]

# Get the value with a default value if key isn't found
# Note: I like thise over `defaultdict(<default value type>)` for counting since it can avoid accidentally creating phantom keys
my_map.get(<key>, <default value>)

# Note: pretty much a must for adjacency list creation
from collections import defaultdict
my_map = defaultdict(<default value's type>)

# Check if key exists
<key> in my_map

# Get all entries
my_map.items()

# Get all keys
my_map.keys()

# Get all values
my_map.values()
```

### HashSet

#### Use Case

* Track elements seen so far for uniqueness
* Store a chunk (or all) of the input for fast lookups

#### Template

```python
# Create an empty set
my_set = set()

# Add an element
my_set.add(<element>)

# Check if a set contains an element
<element> in my_set

# Remove an element
my_set.remove(<element>)

# Get the set difference
my_set.difference(other_set)

# Get the set intersection
my_set.intersection(other_set)

# Get the set union
my_set.union(other_set)

# Remove all elements
my_set.clear()

# Get the number of elements
len(my_set)

# Check if the set is empty
not my_set
```

## Fast and Slow Pointers Technique

### Floyd's Cycle Detection (Same Start, Different Speed)

#### Use Case

* Detect cycles in linked list
* Find middle

#### Template

```python
def fast_and_slow_pointers_same_start_diff_speed(head):
    fast = head
    slow = head

    while fast and fast.next:
        # Some logic

        fast = fast.next.next
        slow = slow.next
```

### Fixed Gap (Different Start, Same Speed)

#### Use Case

Find or operate on a node that is a fixed number of steps from the end of a singly linked list.

#### Template

```python
def find_kth_from_end(head, k):
    fast = head
    for _ in range(k):
        if not fast: return None
        fast = fast.next

    slow = head
    while fast:
        fast = fast.next
        slow = slow.next

    return slow
```

## Reversing a Linked List Technique

### Use Case

* The problem largely involves reversing pointers in a linked list
* You need a subroutine which involves classically reversing pointers in a linked list

### Template

```python
def reverse_linked_list(head):
    prev_node = None
    curr_node = head

    while curr_node:
        next_node = curr_node.next
        curr_node.next = prev_node
        prev_node = curr_node
        curr_node = next_node
```

> **Note (Reduce Edge Cases)**\
> Create a dummy head node `dummy = listnode(0, head)` to reduce edge cases.

## Stack and Queue Technique

### Stack Technique

#### Use Case

Elements in the input interacting with each other, with a LIFO order.

#### Template

```python
# Create an empty stack
stack = list()

# Push an element onto the top
stack.append(<element>)

# Pop the element at the top
stack.pop()

# Get the element at the top
stack[-1]

# Get the number of elements in the stack
len(stack)

# Check if the stack is empty
not stack
```

> **Note**\
> We often use the stack to store the result and convert it to a string using the $O(n)$ string builder Technique.
>

```python
> def build_string(string):
>     array = []
>
>     for char in string:
>         array.append(char)
>
>     return "".join(array)
> ```

### Queue Technique

#### Use Case

Processing elements in a FIFO order.

#### Template

```python
from collections import deque

# Create a queue from an initial list
queue = deque(<initial list>)

# Enqueue an element at the back
queue.append(<element>)

# Dequeue the element at the front
queue.popleft()

# Get the element at the front
queue[0]

# Get the number of elements
len(queue)

# Check if queue is empty
not queue
```

## Monotonic Stack and Monotonic Deque Technique

### Monotonic Stack

#### Use Case

* Looking for the "next" or "previous" "greater" or "smaller" element
* "Span until" i.e. counting a range

#### Template

```python
def monotonic_non_decreasing_stack(array):
    stack = []
    result = <initial value>

    for i in range(len(array)):
        while stack and array[stack[-1]] > array[i]:
		        stack.pop()
            # Utilize stack.pop() in result
            # e.g. result[stack.pop()] = <something involving i>
            # e.g. result += stack.pop()

        stack.append(i)

    return result
```

### Monotonic Deque

#### Use Case

Maximum or minimum values in sliding window or ranges.

#### Template

```python
from collections import deque

def monotonic_non_increasing_deque(array, k):
    result = []
    dq = deque()

    for i in range(len(array)):
        while dq and array[dq[-1]] < array[i]:
            dq.pop()

        dq.append(i)

        if dq[0] <= i - k:
            dq.popleft()

        if i >= k - 1:
            result.append(array[dq[0]])

    return result
```

> **Note**\
> A monotonic increasing/decreasing stack or queue implies that the elements are always increasing/decreasing, while a monotonic non-decreasing/non-increasing one may include repeated elements.

> **Note**\
> Store indices when you want to calculate distances or want to mutate the result list later.

## Binary Tree Traversals Technique

### Binary Tree Depth-First Search

#### Use Case

Most binary tree problems that don't involve processing nodes by their levels.

#### Template

```python
def recursive_preorder_dfs(root):
    if not root:
        return <base case result>

    # Additional base cases

    # Some logic involving the current node and the current result

    recursive_preorder_dfs(root.left)
    recursive_preorder_dfs(root.right)

    return <current result and the two recursive calls above>
```

```python
def iterative_preorder_dfs(root):
    stack = [root]

    result = <initial value>

    while stack:
        node = stack.pop()

	    # Additional base cases

        # Some logic involving the popped node and the result

	    if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)

    return result
```

> **Note (Edge Cases)**
> - Empty tree (i.e. `None` root)
> - Single node tree
> - Skewed tree (i.e. unbalanced trees where all nodes are on one side)
> - Duplicate values in a binary search tree

> **Note (Common Depth-First Search Ordering)**\
> In the iterative depth-first search, the flow is usually pre-order `pop node → process node → push right → push left` , while in the recursive depth-first search, pre-order `process node → recurse left → recurse right` is the most common, followed by post-order `recurse left → recurse right → process node` , then in-order `recurse left → process node → recurse right` .

> **Note (Additional Context)**\
> In a recursive depth-first search, when you need to track additional context or values across recursive calls—such as parent nodes, path lengths, or accumulated data—you include that information as an additional parameter. In an iterative depth-first search, you would store them as part of a tuple `(node, <value 1>, <value 2>, <…>, <value N>)` that'll be pushed and popped from the explicit stack you create outside the while loop.

> **Note (Keeping Track of the Final Result)**\
> In a recursive depth-first search, `result` is typically implicit since it's usually sufficient to implicitly be returned, but an explicit `result` is sometimes a good choice, where you create it within the scope of the provided function, then create and call an inner depth-first search function to do the actual work. In an iterative depth-first search, you typically create an explicit `result` variable outside the while loop.

> **Note (BST Techniques)**\
> BST problems typically use DFS traversal. Common Techniques include:
> - Checking if the current node's value is within bounds (e.g., `low <= node.val <= high` )
> - Leveraging the BST property to prune subtrees — if `node.val < low` , skip the left subtree; if `node.val > high` , skip the right subtree (where `low` or `high` can also just be a target value)
> - Using inorder traversal to collect values in sorted order for problems requiring sorted data without explicit sorting.

### Binary Tree Breadth-First Search

#### Use Case

Binary tree problems that involve processing nodes by their levels.

#### Template

```python
from collections import deque

def bfs(root):
    if not root:
        return

    queue = deque([root])
    result = 0

    while queue:
        level_width = len(queue)

        # Some logic involving the current level

        for _ in range(level_width):
            node = queue.popleft()

            # Some logic involving the current node

            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

    return result
```

## Graph Traversals Technique

### `AdjacencyListGraph` Depth-First Search

#### Use Case

Most graph problems.

#### Template

```python
def adjacency_list_recursive_dfs(graph):
    def dfs(vertex):
        visited.add(vertex)
        result = <initial value>
        for neighbour in graph[vertex]:
            if neighbour not in visited:
                result += dfs(neighbour)
        return result

    result = <initial value>
    visited = set()
    for vertex in graph:
        if vertex not in visited:
            dfs(vertex)

            # Some logic involving the result per connected component
```

```python
def adjacency_list_iterative_dfs(graph):
    def dfs(vertex):
	    result = <initial value>
        stack = [vertex]
        while stack:
            vertex = stack.pop()
            visited.add(vertex)
            for neighbour in graph[vertex]:
                if neighbour not in visited:
                    result += <calculation>
                    stack.append(neighbour)
		return result

    result = <initial value>
    visited = set()
    for vertex in graph.keys():
        if vertex not in visited:
            dfs(vertex)

            # Some logic involving the result per connected component
```

### `MatrixGraph` Depth-First Search

#### Use Case

Most graph problems where the input is a matrix.

#### Template

```python
def matrix_recursive_dfs(matrix):
    ROWS = len(matrix)
    COLUMNS = len(matrix[0])
    # NOTE: each element is of the form (change in row, change in col)
    DIRECTIONS = [(1, 0), (-1, 0), (0, -1), (0, 1)]

    def valid_cell(row, col):
        return 0 <= row < ROWS and 0 <= col < COLUMNS and <another condition for a cell to be valid>

    def dfs(row, col):
        visited.add((row, col))
        result = <initial value>
        for dr, dc in DIRECTIONS:
            neighbour_row = row + dr
            neighbour_col = col + dc
            if valid_cell(neighbour_row, neighbour_col) and (neighbour_row, neighbour_col) not in visited:
                result += dfs(neighbour_row, neighbour_col)
        return result

    visited = set()
    result = <initial value>
    for row in range(ROWS):
        for col in range(COLUMNS):
            if (row, col) not in visited:
                dfs(row, col)

                # Some logic involving the result per connected component

    return result
```

```python
def matrix_iterative_dfs(matrix):
    ROWS = len(matrix)
    COLUMNS = len(matrix[0])
    # NOTE: each element is of the form (change in row, change in col)
    DIRECTIONS = [(1, 0), (-1, 0), (0, -1), (0, 1)]

    def valid_cell(row, col):
        return 0 <= row < ROWS and 0 <= col < COLUMNS and <another condition for a cell to be valid>

    def dfs(row, col):
        stack = [(row, col)]
        result = <initial value>
        while stack:
            row, col = stack.pop()
            visited.add((row, col))
            for dr, dc in DIRECTIONS:
                neighbour_row = row + dr
                neighbour_col = col + dc
                if valid_cell(neighbour_row, neighbour_col) and (neighbour_row, neighbour_col) not in visited:
                    result += <some calculation>
                    stack.append((neighbour_row, neighbour_col))
        return result

    visited = set()
    result = <initial value>
    for row in range(ROWS):
        for col in range(COLUMNS):
            if (row, col) not in visited:
                dfs(row, col)

                # Some logic involving the result per connected component

    return result
```

### `AdjacencyListGraph` Breadth-First Search

#### Use Case

Distance in a graph.

#### Template

```python
from collections import deque

def adjacency_list_bfs(graph):
    queue = deque([(<source vertex>,<additional state>, <initial distance>)])
    visited = set([<source vertex>])

    while queue:
        vertex, <additional state>, dist = queue.popleft()

        if vertex == <destination vertex>:
            return dist

        for neighbour in graph[vertex]:
            if neighbour not in visited:
                visited.add(neighbour)
                queue.append((neighbour, <additional state>, dist + 1))

            # Some logic involving the neighbour
```

### `MatrixGraph` Breadth-First Search

#### Use Case

Distance in a graph given as a matrix.

#### Template

```python
def matrix_iterative_bfs(matrix):
    ROWS = len(matrix)
    COLUMNS = len(matrix[0])
    # NOTE: each element is of the form (change in row, change in col)
    DIRECTIONS = [(0, 1), (1, 0), (1, 1), (-1, -1), (-1, 1), (1, -1), (-1, 0), (0, -1)]

    def valid_cell(row, col):
        return 0 <= row < ROWS and 0 <= col < COLUMNS and <another condition for a cell to be valid>

    queue = deque([(<source vertex>,<additional state>, <initial distance>)])
    visited = set([(<source vertex>, <initial distance>)])

    while queue:
        row, col, dist = queue.popleft()

        if (row, col) == (<destination row>, <destination col>):
            return dist

        for dr, dc in DIRECTIONS:
            neighbour_row = row + dr
            neighbour_col = col + dc
            neighbour_dist = dist + 1
            if (neighbour_row, neighbour_col) not in visited and valid_cell(neighbour_row, neighbour_col):
                visited.add((neighbour_row, neighbour_col))
                queue.append((neighbour_row, neighbour_col, <additional state>, neighbour_dist))

            # Some logic involving the neighbour's row and col
```

> **Note (Various Types of Graph Inputs)**\
> Unlike linked lists and binary trees, which we are given `head` or `root` respectively, there are various graph inputs:
> 1. Matrix: A 2D list, where each element will represent a vertex, but are _not_ numbered `0` to `n`, its neighbours are the adjacent squares, and the edges are determined by the problem description.
> 2. Edge list: A list of edges `edges`. It's useful to turn it into an adjacency list.
> ```python
> from collections import defaultdict
>
> def build_adjacency_list_graph(edges):
>    graph = defaultdict(list)
>    for u, v in edges:
>        graph[x].append(y)
>        graph[y].append(x) # comment out this line if the input is a directed graph
>
>    return graph
> ```
> 3. Integer Adjacency List: A 2D list of integers `graph`, where `n` nodes are numbered from `0` to `n - 1`, and `graph[i]` represents the neighbours of node `i`.
> 4. Integer Adjacency Matrix: A 2D list of integers, where `n` nodes are numbered from `0` to `n - 1`, thereby forming an `n x n` square matrix, and where when `graph[i][j] == 1`, there exist an edge between node `i` and node `j`, and when `graph[i][j] == 0`, there is no edge between node `i` and node `j`. It's also useful to pre-process it into an adjacency list.
> ```python
> from collections import defaultdict
>
> def build_adjacency_list_graph(adjacency_matrix):
>     graph = defaultdict(list)
>     n = len(adjacency_matrix)
>
>     for i in range(n):
>         for j in range(i + 1, n):
>             if adjacency_matrix[i][j]:
>                 graph[i].append(j)
>                 graph[j].append(i) # comment out this line if the input is a directed graph
>
>    return graph
> ```
> Although, even if the input is none of the above, it may still be an implicit graph problem, often where vertices aren't explicitly given, but can be generated on the fly through valid transitions or transformations. These problems typically involve:
> - A starting state and a goal/end state
> - A defined set of valid transitions or mutations
> - Optional constraints like invalid/intermediate states

> **Note ( `visited` Container)**\
> `visited` is typically a HashSet, but you might achieve better runtime performance by using a boolean array when the node range is predetermined (which is typical since graph problems usually number nodes from `0` to `n - 1` )

> **Note (Prohibited Vertices)**\
> Whenever the problem mentions prohibited vertices, then that's an indicator to add them straight away to the `visited` container.

> **Note (Distance vs Path Length for BFS)**\
> When using BFS to find shortest paths:
> - If the problem asks for distance (number of moves/steps), initialize source vertices with `distance = 0`

> - If the problem asks for path length (number of cells in the path), initialize source vertices with `path_length = 1`

> **Note (Multi-source BFS)**\
> For a multi-source BFS, create a for loop that visits all source nodes and appends them to the queue for the BFS.

> **Note (Inverse Thinking)**\
> For graph problems, it's useful to rephrase the problem in terms of its inverse. For instance, take [LeetCode #1557](https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes/). The original problem description asks us to find the smallest set of vertices from which all nodes in the graph are reachable. Instead, we can rephrase the problem description in terms of its inverse — find the smallest set of nodes that _cannot_ be reached from other nodes, since if a node can be reached from another node, then we would rather just include the pointer rather than the pointee in our set. Another example is [LeetCode #542](https://leetcode.com/problems/01-matrix/description/). The brute force solution would be to perform BFS for each cell with a 1, but instead, we can perform a multi-source BFS by performing starting from all cells with a 0 (if we have a cell `x` with value 1 and its nearest cell y has value 0, then it doesn't make a difference if we traverse from `x -> y` or `y -> x` — both give the same distance).

## PriorityQueue Technique

### Use Case

TODO

### Template

```python
import heapq

# Create an empty priority queue
priority_queue = []
heapq.heapify([<elements>])

# Add an element
heapq.heappush(priority_queue, <element>)

# Remove the top element
heapq.heappop(priority_queue)

# Peek at the top element
priority_queue[0]

# Get the number of elements
len(priority_queue)

# Check if the priority queue is empty
not priority_queue
```
