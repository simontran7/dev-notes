# Algorithm Patterns

## Classic Two Pointers Pattern

### Two Pointers Opposite Variant

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

### Two Pointers Same Speed Variant

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

## Sliding Window Pattern

### Fixed Sliding Window Variant

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

### Variable Sliding Window Variant

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
> The constraint must be preserved as the sliding window shrinks. If shrinking the window can break the constraint, consider using a prefix sum or hash map pattern instead.

> **Note**\
> The formula for the length of a window is `right - left + 1`.

## Prefix Sum Pattern

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

## HashMap and HashSet Pattern

### HashMap

#### Use Case

- Track elements seen so far for uniqueness (with any extra info stored as the value)
- Frequency counting
- Basic mapping
- Memoization table
- Count number of subarrays with a constraint that _doesn't_ necessarily hold as you shrink the window

```python
from collections import defaultdict

def count_num_subarrays_with_exact_constraint(array, k):
    counts = defaultdict(int)
    counts[0] = 1
    result = 0
    current = 0

    for num in array:
        # Some logic to change current as necessary
        result += counts[current - k]
        counts[current] += 1

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
my_map.get(<key>, <default value>)

# Or use defaultdict
from collections import defaultdict
my_map = defaultdict(<type>)

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

Track elements seen so far for uniqueness.

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

## Fast and Slow Pointers Pattern

### Same Start, Different Speed

#### Use Case

- Detect cycles in linked list
- Find middle

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

### Different Start, Same Speed

#### Use Case

Find or operate on a node that is a fixed number of steps from the end of a singly linked list.

#### Template

```python
def fast_and_slow_pointers_diff_start_same_speed(head):
    fast = head
    slow = head

    for _ in range(k):
        fast = fast.next

    while fast:
        fast = fast.next
        slow = slow.next

    return slow
```

## Reversing a Linked List Pattern

### Use Case

- The problem largely involves reversing pointers in a linked list
- You need a subroutine which involves classically reversing pointers in a linked list

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

> **Note**\
> Create a dummy node `dummy = listnode(0, head)` to simplify edge cases.

## Stack and Queue Pattern

### Stack Pattern

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
> We often use the stack to store the result and convert it to a string using the $O(n)$ string building pattern.
> ```python
> def build_string(string):
>     array = []
>
>     for char in string:
>         array.append(char)
>
>     return "".join(array)
> ```

### Queue Pattern

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

## Monotonic Stack and Monotonic Deque Pattern

### Monotonic Stack

#### Use Case

- Looking for the "next" or "previous" "greater" or "smaller" element
- "Span until" i.e. counting a range

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

## Binary Tree Traversals Pattern

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
> In the iterative depth-first search, the flow is usually pre-order `pop node → process node → push right → push left`, while in the recursive depth-first search, pre-order `process node → recurse left → recurse right` is the most common, followed by post-order `recurse left → recurse right → process node`, then in-order `recurse left → process node → recurse right`.

> **Note (Additional Context)**\
> In a recursive depth-first search, when you need to track additional context or values across recursive calls—such as parent nodes, path lengths, or accumulated data—you include that information as an additional parameter. In an iterative depth-first search, you would store them as part of a tuple `(node, <value 1>, <value 2>, <…>, <value N>)` that'll be pushed and popped from the explicit stack you create outside the while loop.

> **Note (Keeping Track of the Final Result)**\
> In a recursive depth-first search, `result` is typically implicit since it's usually sufficient to implicitly be returned, but an explicit `result` is sometimes a good choice, where you create it within the scope of the provided function, then create and call an inner depth-first search function to do the actual work. In an iterative depth-first search, you typically create an explicit `result` variable outside the while loop.

> **Note (BST Patterns)**\
> BST problems typically use DFS traversal. Common patterns include:
> - Checking if the current node's value is within bounds (e.g., `low <= node.val <= high`)
> - Leveraging the BST property to prune subtrees — if `node.val < low`, skip the left subtree; if `node.val > high`, skip the right subtree (where `low` or `high` can also just be a target value)
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

## Graph Traversals Pattern

### Graph Depth-First Search

#### Use Case

TODO

#### Template

```python
def recursive_dfs(graph):
    def dfs_helper(vertex):
        visited.add(vertex)
        for neighbour in graph[vertex]:
            if neighbour not in visited:
                dfs_helper(neighbour)
        """
        ALTERNATIVE (for connected graphs, or when u only need to explore from one starting point): recursive accumulation

        Result can be accumulated within the recursive function instead of in the outer loop. In this case, the helper function returns a value that gets combined with recursive calls:

        result = some_initial_value
        for neighbour in graph[vertex]:
            if neighbour not in visited:
                result += dfs_helper(neighbour)  # Accumulate from recursive calls
        return result
        """

    result = <container or primitive data type>
    visited = set()
    for vertex in graph:
        if vertex not in visited:

            # Some logic involving the result per connected component

            dfs_helper(vertex)
    """
    ALTERNATIVE (for connected graphs, or when u only need to explore from one starting point): recursive accumulation

    If using recursive accumulation, you might call the helper directly and it returns the accumulated result:

    visited = set()
    return dfs_helper(<starting vertex>)
    """
```

```python
def iterative_dfs(graph):
    def dfs_helper(vertex):
        stack = []
        while stack:
            vertex = stack.pop()
            visited.add(vertex)
            for neighbour in graph[vertex]:
                if neighbour not in visited:
                    stack.append(neighbour)

    result = <container or primitive data type>
    visited = set()
    for vertex in graph.keys():
        if vertex not in visited:

            # Some logic involving the result per connected component

            dfs_helper(vertex)
    """
    ALTERNATIVE (for connected graphs, or when u only need to explore from one starting point): Accumulation within the iterative DFS traversal

    In this case, we don't use the outer loop pattern and instead
    modify the result directly within the while loop:

    result = <initial value>
    stack = [<start vertex>]
    visited = set()
    while stack:
        vertex = stack.pop()
        visited.add(vertex)
        for neighbour in graph[vertex]:
            if neighbour not in visited:
                # Logic to modify result happens here
                within the traversal

                result += <some calculation>

                stack.append(neighbour)
    return result
    """
```

### Graph Breadth-First Search

#### Use Case

TODO

#### Template

```python
from collections import deque

def bfs(graph):
    queue = deque([start_node])
    visited = set()
    result = 0

    while queue:
        vertex = queue.popleft()
        visited.add(vertex)

        # Some logic

        for neighbour in graph[vertex]:
            if neighbour not in visited:
                queue.append(neighbor)

    return result
```

> **Note (Various Types of Graph Inputs)**\
> Unlike linked lists and binary trees, which we are given `head` or `root` respectively, there are various graph inputs:
>
> 1. Edge list: A list of edges `edges`. It's useful to turn it into an adjacency list.
> ```python
> from collections import defaultdict
>
> def build_adjacency_list_graph(edges):
>    graph = defaultdict(list)
>    for u, v in edges:
>        graph[x].append(y)
>        # graph[y].append(x) uncomment this line if the input is an undirected graph
>
>    return graph
> ```
>
> 2. Integer Adjacency List: A 2D list of integers `graph`, where `n` nodes are numbered from `0` to `n - 1`, and `graph[i]` represents the neighbours of node `i`.
>
> 3. Integer Adjacency Matrix: A 2D list of integers, where `n` nodes are numbered from `0` to `n - 1`, thereby forming an `n x n` square matrix, and where when `graph[i][j] == 1`, there exist an edge between node `i` and node `j`, and when `graph[i][j] == 0`, there is no edge between node `i` and node `j`. It's also useful to pre-process it into an adjacency list.
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
>                 # graph[j].append(i) uncomment this line if the input is an undirected graph
>
>    return graph
> ```
> 4. Matrix: A 2D list, where each element will represent a vertex, but are _not_ numbered `0` to `n`, its neighbours are the adjacent squares, and the edges are determined by the problem description.

> **Note (`visited` Container)**\
> `visited` is typically a HashSet, but you might achieve better runtime performance by using a boolean array when the node range is predetermined (which is typical since graph problems usually number nodes from `0` to `n - 1`)

> **Note (`directions` List)**\
> It's good practice to define a list `directions = [(1, 0), (-1, 0), (0, -1), (0, 1)]` where a tuple is `(row change, column change)` when determining the neighbours in a matrix.

> **Note (Inverse Thinking)**\
> For graph problems, it's useful to rephrase the problem in terms of its inverse. For instance, take [LeetCode #1557](https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes/). The original problem description asks us to find the smallest set of vertices from which all nodes in the graph are reachable. Instead, we can rephrase the problem description in terms of its inverse — find the smallest set of nodes that _cannot_ be reached from other nodes, since if a node can be reached from another node, then we would rather just include the pointer rather than the pointee in our set.

## PriorityQueue Pattern

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

## Miscellaneous (Useful Python Language Features)

https://neetcode.io/courses/lessons/python-for-coding-interviews
