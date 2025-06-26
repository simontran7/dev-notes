# Asymptotic Analysis

## Bachmann-Landau Notation

> **Definition (big O)**
>
> Let $f(n)$ and $g(n)$ be functions mapping positive integers to positive real numbers.
> We say that $f(n)$ is $O(g(n))$ if there is a real constant $c \gt 0$ and an integer constant
> $n\_0 \ge 1$ such that
>
> $$
> f(n) \leq c \cdot g(n), \text{ for } n \geq n_0
> $$

### Simplification Rules

1. Ignore non-dominant terms
2. Ignore constant factors

### Growth Rates Classifications

In $T_{case}(n) = f(n) \in O(g(n))$, $g(n)$ is one of the following:

| Growth type  | Function     |
| ------------ | ------------ |
| Constant     | $1$        |
| Logarithmic  | $\log n$   |
| Linear       | $n$        |
| Linearithmic | $n \log n$ |
| Quadratic    | $n^2$      |
| Cubic        | $n^3$      |
| Exponential  | $2^n$      |
| Factorial    | $n!$       |

## Worst Case Time Complexity

### Iterative Algorithms

#### Complexity of Primitive Operations

The following primitive operations take $O(1)$:

* Declaring a variable
* Assigning a value to a variable
* Following an object reference
* Performing an arithmetic operation
* Comparing two numbers
* Accessing a single element of an array by index
* Accessing the length of an array
* Calling a method
* Returning from a method

#### Complexity of Loops

The complexity of a loop is expressed as the sum of the complexities of each iteration of the loop. Whenever there's nested loops, begin by calculating the innermost loop and proceed outwards, which will give nested summations.

Some useful summation properties and formulas:

> **Property (big O and summation)**
>
> $$
> \sum_{i \in I} O(f(i)) = O\left(\sum_{i \in I} f(i)\right)
> $$

> **Property (summation of a constant)**
>
> $$
> \sum_{i = m}^{n} ca_i = c \sum_{i = m}^{n} a_i
> $$

> **Property (addition and subtraction of summations)**
>
> $$
> \sum_{i = m}^{n} (a_i \pm b_i) = \sum_{i = m}^{n} a_i \pm \sum_{i = m}^{n} b_i
> $$

> **Formula (summation of a $1$)**
>
> $$
> \sum_{i = m}^{n} 1 = n - m + 1
> $$

> **Formula (summation of arithmetic series)**
>
> $$
> \sum_{i = 1}^{n} i = \frac{n(n + 1)}{2}
> $$

> **Formula (summation of quadratic series)**
>
> $$
> \sum_{i = 1}^{n} i^2 = \frac{n(n + 1)(2n + 1)}{6}
> $$

> **Formula (summation of cubic series)**
>
> $$
> \sum_{i = 1}^{n} i^3 = \left(\frac{n(n + 1)}{2}\right)^2
> $$

> **Formula (summation of $i^k$ series)**
>
> $$
> \sum_{i = 1}^{n} i^k \approx \frac{1}{k + 1}n^{k + 1}
> $$

> **Formula (summation of geometric series)**
>
> $$
> \sum_{i = 1}^{n}ar^{i - 1} = a\left(\frac{r^n - 1}{r - 1}\right) = a\left(\frac{1 - r^n}{1 - r}\right), \text{ where } r > 0 \text{ and } r \neq 1
> $$

> **Formula (summation of harmonic series)**
>
> $$
> \sum_{i = 1}^{n} \frac{1}{i} \approx \ln n + \gamma, \text{ where } \gamma \approx 0.5772 \dots
> $$

> **Formula (summation of $\log\_2$ series)**
>
> $$
> \sum_{i = 1}^{n} \log_2 i = O\left(\sum_{i = 1}^{n} \log_2 n\right) = O(n\log_2 n)
> $$

#### General Steps

1. Calculate the total cost by summing the costs of statements where the statements may be primitive operations or loops
2. Apply Bachmann-Landau notation simplification rules

> **Note (States in Graph Algorithm Complexity)**\
> When analyzing the time and space complexity of graph algorithms, the standard formula is $O(V + E)$ where $V$ is vertices and $E$ is edges. However, whenever the **state** encompasses more than just the vertex position, we need to reframe our analysis:
>
> - **Standard case**: State = vertex position only
>   - Time complexity: $O(V + E)$
>   - We visit each vertex at most once
>
> - **State-space case**: State = (vertex, additional_constraints)
>   - Let $S$ = total number of possible states
>   - Time complexity: $O(S + E_{state})$ where $E_{state}$ is the number of state transitions
>   - We visit each _state_ at most _once_, but may revisit _vertices_ under _different constraints_
>
> **Example**: In a shortest path problem with fuel constraints on an $m \times n$ grid:
> - State = (grid_position, remaining_fuel)
> - $S = m \times n \times k$ where $k$ is max fuel
> - $E_{state} \approx 4 \times m \times n \times k$ (each state can transition to ~4 neighbors)
> - Time complexity: $O(S + E_{state}) = O(mnk + 4mnk) = O(mnk)$
>
> The key insight: analyze complexity based on unique states processed, not just unique vertices visited.

### Recursive Algorithms

1. Come up with a recurrence relation of the following form

$$
T(n)= aT\left(\frac{n}{b}\right)+f(n)
$$

- $T(n)$: Time complexity for input size $n$
- $a$: Number of recursive calls
- $b$: Factor by which the problem size is divided
- $f(n)$: Time complexity of the work done outside the recursive calls

2. Use backward substitution, recursion tree, or master theorem to solve the recurrence relation

## Worst Case Space Complexity

Common data structure space complexities:

- Variable or pointer: $O(1)$
- Array: $O(n)$
- Node-Pointer Data Structure: $O(n)$
- Matrices: $O(m \cdot n)$
- Adjacency List: $O(V + E)$

### Iterative Algorithms

1. Add up the space complexities of the intermediate data stuctures
2. Apply Bachmann-Landau notation simplification rules

### Recursive Algorithms

1. Add up the space complexities of the intermediate data stuctures
2. For a recursive function, total cost as follows

$$
\text{Cost} = \text{call stack depth} \cdot \text{cost per call}
$$

3. Apply Bachmann-Landau notation simplification rules

> **Note**\
> The algorithm's input annd output is typically excluded from the total space complexity cost.
