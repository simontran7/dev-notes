# algorithm patterns

## classic two pointers pattern

### two pointers opposite variant

#### use case

the list input is sorted, and you are looking for a pair (or pairs) that meet a specific condition.

#### template

```python
def two_pointers_opposite(array):
    left = 0
    right = len(array) - 1
    result = 0

    while left < right:
        # do some logic using `array[left]` and/or `array[right]`

        if <condition>:
            left += 1
        else:
            right -= 1

    return result
```

### two pointers same speed variant

#### use case

the two list inputs are sorted, and you want to compare elements between the two.

#### template

```python
def two_pointers_same_speed(array1, array2):
    i = 0
    j = 0
    result = 0

    while i < len(array1) and j < len(array2):
        # do some logic with `array1[i]` and `array2[j]`
        if <condition>:
            i += 1
        else:
            j += 1

    while i < len(array1):
        # some logic with `array1[i]`
        i += 1

    while j < len(array2):
        # some logic with `array2[j]`
        j += 1

    return result
```

## sliding window pattern

### fixed sliding window variant

#### use case

you are looking for a subarray or substring of some length `k` that satisfies a certain constraint.

#### template

```python
def fixed_sliding_window(array, k):
    current = 0
    result = 0

    for i in range(k):
        # some logic to build first window with `current` or other vars

    for right in range(k, len(array)):
        # add `array[i]` to window
        # remove `arr[i - k]` from window
        # update `result`

    return result
```

### variable sliding window variant

#### use case

you are looking for a subarray or substring (usually the longest) that satisfies a certain constraint.

#### template

```python
def variable_sliding_window(array):
    left = 0
    current = 0
    result = 0

    for right in range(len(array)):
        # some logic here to add `array[right]` to `current`

        while <broken window condition>:
            # remove `array[left]` from `current`
            left += 1

        # update `result`, where current window length
        # at any time is `right - left + 1`

    return result
```

> **Note**\
> the constraint must be preserved as the sliding window shrinks. if shrinking the window can break the constraint, consider using a prefix sum or hash map pattern instead.

> **Note**\
> the formula for the length of a window is `right - left + 1`.

## prefix sum pattern

### use case

you need a subroutine which involves finding the sum of any subarray in *o*(1).

### template

```python
def prefix_sum_building(nums):
    n = len(nums)
    prefix = [0] * (n + 1)

    for i in range(n):
        prefix[i + 1] = prefix[i] + nums[i]

    return prefix
```

## O(*n*) string building pattern

### use case

build a string in *o*(*n*) time

### template

```python
def build_string(string):
    array = []

    for char in string:
        array.append(char)

    return "".join(array)
```

## HashMap and HashSet pattern

### HashMap

#### use case

- track elements seen so far for uniqueness (with any extra info stored as the value)
- frequency counting
- basic mapping
- memoization table
- count number of subarrays with a constraint that _doesn’t_ necessarily hold as you shrink the window

```python
from collections import defaultdict

def count_num_subarrays_with_exact_constraint(array, k):
    counts = defaultdict(int)
    counts[0] = 1
    result = 0
    current = 0

    for num in array:
        # some logic to change current as necessary
        result += counts[current - k]
        counts[current] += 1

    return result
```

#### template

```python
# create an empty map
my_map = dict()

# create a map with initial values
my_map = {
    <key 1>: <value 1>,
    <key 2>: <value 2>
}

# add new entry or update current entry
my_map[<immutable key>] = <value>

# remove an entry
del my_map[<key>]

# remove all entries
my_map.clear()

# get number of entries
len(my_map)

# check if my_map is empty
not my_map

# get value
my_map[<key>]

# get the value with a default value if key isn't found
my_map.get(<key>, <default value>)

# or use defaultdict
from collections import defaultdict
my_map = defaultdict(<type>)

# check if key exists
<key> in my_map

# get all entries
my_map.items()

# get all keys
my_map.keys()

# get all values
my_map.values()
```

### HashSet

#### use case

track elements seen so far for uniqueness

#### template

```python
# create an empty set
my_set = set()

# add an element
my_set.add(<element>)

# check if a set contains an element
<element> in my_set

# remove an element
my_set.remove(<element>)

# get the set difference
my_set.difference(other_set)

# get the set intersection
my_set.intersection(other_set)

# get the set union
my_set.union(other_set)

# remove all elements
my_set.clear()

# get the number of elements
len(my_set)

# check if the set is empty
not my_set
```

## fast and slow pointers pattern

### same start, different speed

#### use case

- detect cycles in linked list
- find middle

#### template

```python
def fast_and_slow_pointers_same_start_diff_speed(head):
    fast = head
    slow = head

    while fast and fast.next:
        # some logic

        fast = fast.next.next
        slow = slow.next
```

### different start, same speed

#### use case

find or operate on a node that is a fixed number of steps from the end of a singly linked list.

#### template

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

## reversing a linked list pattern

### use case

- the problem largely involves reversing pointers in a linked list
- you need a subroutine which involves classically reversing pointers in a linked list

### template

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
> creating a dummy node `dummy = listnode(0, head)` simplifies edge cases.

## stack pattern

### use case

elements in the input interacting with each other, with a lifo order.

### template

```python
# create an empty stack
stack = list()

# push an element onto the top
stack.append(<element>)

# pop the element at the top
stack.pop()

# get the element at the top
stack[-1]

# get the number of elements in the stack
len(stack)

# check if the stack is empty
not stack
```

> ### note
>
> we often use the stack to store the result and convert it to a string using the *o*(*n*) string building pattern

## queue pattern

### use case

processing elements in a fifo order

### template

```python
from collections import deque

# create a queue from an initial list
queue = deque(<initial list>)

# enqueue an element at the back
queue.append(<element>)

# dequeue the element at the front
queue.popleft()

# get the element at the front
queue[0]

# get the number of elements
len(queue)

# check if queue is empty
not queue
```

## monotonic stack and monotonic deque pattern

### monotonic stack

#### use case

- Looking for the "next" or “previous” “greater” or ”smaller” element
- "span until" i.e. counting a range

### template

```python
def monotonic_non_decreasing_stack(array):
    stack = []
    result = <could be a list with default values, or an integer>

    for i in range(len(array)):
        while stack and array[stack[-1]] > array[i]:
		        stack.pop()
            # utilize stack.pop() in result
            # e.g. result[stack.pop()] = <something involving i>
            # e.g. result += stack.pop()

        stack.append(i)

    return result
```

### monotonic deque

#### use case

maximum or minimum values in sliding window or ranges

#### template

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
> a monotonic increasing/decreasing stack or queue implies that the elements are always increasing/decreasing, while a monotonic non-decreasing/non-increasing one may include repeated elements.

> **Note**\
> store indices when you want to calculate distances or want to mutate the result list later.

## tree traversals pattern

### depth-first search

#### use case

most binary tree problems that don’t involve levels

#### template

```python
def recursive_preorder_dfs(node):
    if not node:
        return <base case result>

    # additional base cases

    # some logic involving the current node and the current result

    recursive_preorder_dfs(node.left)
    recursive_preorder_dfs(node.right)

    return <current result and the two recursive calls above>
```

```python
def iterative_preorder_dfs(root):
    stack = [root]

    result = <container or primitive data type>

    while stack:
        node = stack.pop()

		# additional base cases

        # some logic involving the popped node and the result

		if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)

    return result
```

> **Note (base case)**\
> a good way to think about base cases is to think about a tree with only one node.

> **Note (common depth-first search ordering)**\
> in the iterative depth-first search, the flow is usually pre-order `pop node → process node → push right → push left`, while in the recursive depth-first search, pre-order `process node → recurse left → recurse right` is the most common, followed by post-order `recurse left → recurse right → process node`, then in-order `recurse left → process node → recurse right`.

> **Note (additional context)**\
> in a recursive depth-first search, when you need to track additional context or values across recursive calls—such as parent nodes, path lengths, or accumulated data—you include that information as an additional parameter. in a iterative depth-first search, you would store them as part of a tuple `(node, <value 1>, <value 2>, <…>, <value N>)` that’ll be pushed and popped from the explicit stack you create outside the while loop.

> **Note (keeping track of the final result)**\
> in a recursive depth-first search, `result` is typically implicit since it’s usually sufficient to implicitly be returned, but an explicit `result` is sometimes a good choice, where you creating it within the scope of the provided function, then create and call an inner depth-first search function to do the actual work. in a iterative depth-first search, you typically create an explicit `result` variable outside the while loop.

### breadth-first search

#### use case

binary tree problems that involve levels

#### template

```python
from collections import deque

def bfs(root):
    queue = deque([root])
    result = 0

    while queue:
        current_length = len(queue)

        for _ in range(current_length):
            node = queue.popleft()

            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

    return result
```

## graph traversals pattern

### depth-first search

#### use case

TODO

#### template

```python
def recursive_dfs(graph):
    def helper(node):
        result = 0
        for neighbor in graph[node]:
            if neighbor not in seen:
                seen.add(neighbor)
                result += helper(neighbor)
        return result

    seen = {start_node}
    return helper(start_node)
```

```python
def iterative_dfs(graph):
    stack = [start_node]
    seen = {start_node}
    result = 0

    while stack:
        node = stack.pop()

        # some logic

        for neighbor in graph[node]:
            if neighbor not in seen:
                seen.add(neighbor)
                stack.append(neighbor)

    return result
```

> **Note**\
> for graph templates, assume nodes are numbered `0` to `n - 1`, and input is an adjacency list.

### breadth-first search

#### use case

TODO

#### template

```python
from collections import deque

def iterative_bfs(graph):
    queue = deque([start_node])
    seen = {start_node}
    result = 0

    while queue:
        node = queue.popleft()

        # some logic

        for neighbor in graph[node]:
            if neighbor not in seen:
                seen.add(neighbor)
                queue.append(neighbor)

    return result
```

## PriorityQueue pattern

### use case

TODO

### template

```python
import heapq

# create an empty priority queue
priority_queue = []
heapq.heapify([<elements>])

# add an element
heapq.heappush(priority_queue, <element>)

# remove the top element
heapq.heappop(priority_queue)

# peek at the top element
priority_queue[0]

# get the number of elements
len(priority_queue)

# check if the priority queue is empty
not priority_queue
```

## Miscellaneous (useful python language features)

[https://neetcode.io/courses/lessons/python-for-coding-interviews](https://neetcode.io/courses/lessons/python-for-coding-interviews)
