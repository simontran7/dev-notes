# Asymptotic analysis

## Bachmann-Landau notation

> **Definition (big O)**
>
> Let \\( f(n) \\) and \\( g(n) \\) be functions mapping positive integers to positive real numbers.
> We say that \\( f(n) \\) is \\( O(g(n)) \\) if there is a real constant \\( c \gt 0 \\) and an integer constant
> \\( n_0 \ge 1 \\) such that
>
> $$
> f(n) \leq c \cdot g(n), \text{ for } n \geq n_0
> $$

#### Simplification rules

1. Ignore non-dominant terms
2. Ignore constant factors

#### Growth rates classifications

In \\( T\_{case}(n) = f(n) \in O(g(n)) \\), \\( g(n) \\) is one of the following:

| Growth type  | Function         |
| ------------ | ---------------- |
| Constant     | \\( 1 \\)        |
| Logarithmic  | \\( \log n \\)   |
| Linear       | \\( n \\)        |
| Linearithmic | \\( n \log n \\) |
| Quadratic    | \\( n^2 \\)      |
| Cubic        | \\( n^3 \\)      |
| Exponential  | \\( 2^n \\)      |
| Factorial    | \\( n! \\)       |

## Worst case time complexity

### Iterative algorithms

#### Complexity of primitive operations

The following primitive operations take \\( O(1) \\):

* Declaring a variable
* Assigning a value to a variable
* Following an object reference
* Performing an arithmetic operation
* Comparing two numbers
* Accessing a single element of an array by index
* Accessing the length of an array
* Calling a method
* Returning from a method

#### Complexity of loops

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

> **Formula (summation of a \\( 1 \\))**
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

> **Formula (summation of \\( i^k \\) series)**
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

> **Formula (summation of \\( \log\_2 \\) series)**
>
> $$
> \sum_{i = 1}^{n} \log_2 i = O\left(\sum_{i = 1}^{n} \log_2 n\right) = O(n\log_2 n)
> $$

#### General steps

1. Calculate the total cost by summing the costs of statements where the statements may be primitive operations or loops
2. Apply Bachmann-Landau notation simplification rules

### Recursive algorithms

1. Come up with a recurrence relation of the following form

$$
T(n)= aT\left(\frac{n}{b}\right)+f(n)
$$

- \\( T(n) \\): Time complexity for input size \\( n \\)
- \\( a \\): Number of recursive calls
- \\( b \\): Factor by which the problem size is divided
- \\( f(n) \\): Time complexity of the work done outside the recursive calls

2. Use backward substitution, recursion tree, or master theorem to solve the recurrence relation

## Worst case space complexity

### Iterative algorithms

- \\( \\text{Variable or pointer} = O(1) \\)
- \\( \text{1D Array or ADT} = O(n) \\)
- \\( \text{2D Array} = O(m \cdot n) \\)

### Recursive algorithms

1. Calculate total cost as follows

$$
\text{Cost} = \text{call stack depth} \cdot \text{cost per call}
$$

2. Apply Bachmann-Landau notation simplification rules

> **Note**\
> The algorithm's input is typically excluded from the total space complexity cost.
