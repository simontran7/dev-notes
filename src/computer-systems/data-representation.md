# Data Representation

## Number Systems

### Decimal System

The **Decimal System** is a number system that uses base 10, characterized by two fundamental properties:
- Each digit position can hold one of 10 unique values (0 through 9), where values greater than 9 require carrying to an additional digit position to the left.
- Each digit's position determines its contribution to the overall value through positional notation. Digits are labeled from right to left as \\(d_0, d_1, d_2, \dots, d_{N - 1}\\) where each successive position represents ten times the value of the position to its right.

More formally, any \\(N\\)-digit number can be expressed using **positional notation** as:

$$
(d_{N-1} \cdot 10^{N-1}) + (d_{N-2} \cdot 10^{N-2}) + \dots + (d_2 \cdot 10^2) + (d_1 \cdot 10^1) + (d_0 \cdot 10^0)
$$

Numbers under the base 10 system are typically written without prefixes, though they may be denoted as \\(N_{10}\\) when clarification is needed.

### Binary System

**Binary** is a number system that uses base 2, where:

- Each digit position (called a **bit**) can hold one of two unique values: 0 or 1. Values greater than 1 require carrying to an additional bit position to the left.
- Each bit's position determines its contribution through powers of 2: \\(2^{N-1}, \dots, 2^1, 2^0\\).

Binary numbers are typically prefixed with `0b`.

#### Binary Groupings

Some common bit groupings:

- **Byte**: 8 bits (the smallest addressable unit of memory)
- **Nibble**: 4 bits (half a byte)
- **Word**: The natural data width of a CPU (typically 32 or 64 bits)

In any binary sequence, we identify:

- **Most Significant Bit (MSB)**: The leftmost bit
- **Least Significant Bit (LSB)**: The rightmost bit

### Hexadecimal System

**Hexadecimal** uses base 16, where:

- Each digit position can hold one of 16 unique values, such that the first ten values are 0 to 9, followed by letters A to F represent values 10 to 15 respectively.
- Each digit's position determines its contribution through powers of 16: $16^{N-1}, \dots, 16^1, 16^0$.

Hexadecimal numbers are prefixed with `0x`.

#### Hexadecimal-Binary-Decimal Conversion Table

| Hex | Binary | Decimal |
| --- | ------ | ------- |
| 0   | 0000   | 0       |
| 1   | 0001   | 1       |
| 2   | 0010   | 2       |
| 3   | 0011   | 3       |
| 4   | 0100   | 4       |
| 5   | 0101   | 5       |
| 6   | 0110   | 6       |
| 7   | 0111   | 7       |
| 8   | 1000   | 8       |
| 9   | 1001   | 9       |
| A   | 1010   | 10      |
| B   | 1011   | 11      |
| C   | 1100   | 12      |
| D   | 1101   | 13      |
| E   | 1110   | 14      |
| F   | 1111   | 15      |

## Converting Between Number Systems

### Decimal to Any Base (Division method)

1. Repeatedly divide the decimal number by the target base
2. Record the remainder at each step
3. Continue until the quotient becomes 0
4. Read the remainders from bottom to top to get the result

### Any Base to Decimal (Addition of Positional Notation)

For a number with digits \\(d\_{N-1}, d\_{N-2}, \dots, d\_1, d\_0\\) in base \\(B\\), the converted number to base \\(C\\) is calculated using the following formula:

$$
(d_{N-1} \cdot B^{N-1}) + (d_{N-2} \cdot B^{N-2}) + \dots + (d_1 \cdot B^1) + (d_0 \cdot B^0)
$$

### Binary to Hexadecimal

1. Group binary digits into sets of 4 bits from right to left
2. Pad the leftmost group with leading zeros if necessary
3. Convert each 4-bit group to its hexadecimal equivalent

### Hexadecimal to Binary

1. Convert each hexadecimal digit to its 4-bit binary equivalent
2. Concatenate all binary groups

## Computer Data Units

### Memory and Storage Capacities

| Unit            | Value                                                |
| --------------- | ---------------------------------------------------- |
| 1 KB (Kilobyte) | $2^{10}$ bytes (1,024 bytes)                         |
| 1 MB (Megabyte) | $2^{20}$ bytes (1,048,576 bytes)                     |
| 1 GB (Gigabyte) | $2^{30}$ bytes (1,073,741,824 bytes)                 |
| 1 TB (Terabyte) | $2^{40}$ bytes (1,099,511,627,776 bytes)             |
| 1 PB (Petabyte) | $2^{50}$ bytes (1,125,899,906,842,624 bytes)         |
| 1 EB (Exabyte)  | $2^{60}$ bytes (1,152,921,504,606,846,976 bytes)     |

### Data Transfer Units

| Unit           | Value                  |
| -------------- | ---------------------- |
| 1 Kilobit (Kb) | 1,000 bits             |
| 1 Megabit (Mb) | 1,000,000 bits         |
| 1 Gigabit (Gb) | 1,000,000,000 bits     |
| 1 Terabit (Tb) | 1,000,000,000,000 bits |

## Character Representation

### ASCII

The **American Standard Code for Information Interchange (ASCII)** is a character encoding standard which uses 8 bits per character, where 7 bits encode the actual character and the eighth bit serves as a parity bit for error detection, where the added bit ensures either an even number of 1 (even parity) or an odd number of 1 (odd parity) in the complete byte.

#### Types of ASCII Characters

- Non-printable characters
  - Control characters, from `0x00` to `0x1F`
  - Delete character, `0x7F`
- Printable characters, from `0x20` to `0x7E`

#### ASCII Table

[Searchable ASCII Table](https://www.rapidtables.com/code/text/ascii-table.html)

### Unicode

**Unicode** is a character set standard that assigns a unique hexadecimal identifier, called a **code point**, to every character across all writing systems. These code points follow the format `U+<....>`.

#### Code Point Categories

- **Surrogate code points**: Code points in the range `U+D800` to `U+DFFF`, which are reserved for use in UTF-16 encoding
- **Scalar values**: All other Unicode code points (excluding surrogate code points)

#### Character Encoding Standards

Unicode defines three primary character encoding standards for converting code points into bytes for storage and transmission. These standards use **code units** as their basic storage elements to represent characters.

- UTF-8
  - **Code unit size**: 8 bits (1 byte)
  - **Character representation**: 1 to 4 code units per character
  - **Encoding type**: Variable-length encoding
- UTF-16
  - **Code unit size**: 16 bits (2 bytes)
  - **Character representation**: 1 code unit, or 2 code units for surrogate code points, and we call these 2 code units a **surrogate pair**
- UTF-32
  - **Code unit size**: 32 bits (4 bytes)
  - **Character representation**: Exactly 1 code unit per character

### Grapheme clusters

A **grapheme cluster** represents what users typically think of as a "character", that is, the smallest unit of written language that has semantic meaning.

#### Example

The grapheme clusters in the Hindi word `"क्षत्रिय"` are `["क्ष", "त्रि", "य"]`, where each cluster can comprise multiple Unicode code points. While `"य"` is a single code point, `"क्ष"` is a conjunct consonant formed from three code points: the base character `"क"` (`U+0915`), the virama `"्"` (`U+094D`), and `"ष"` (`U+0937`), which combine to create one visual unit. The cluster `"त्रि"` is even more complex, consisting of four code points: `"त"` (`U+0924`), `"्"` (`U+094D`), `"र"` (`U+0930`), and the vowel sign `"ि"` (`U+093F`), where the vowel mark appears visually before the consonant cluster despite following it in the Unicode sequence.

The example above demonstrates why simply counting Unicode code points doesn't always correspond to what users perceive as individual characters, making grapheme cluster boundaries crucial for correct string processing.

## Integer Representation

### Unsigned Integer Representation

**Unsigned integers** use all available bits to represent positive values (including zero). No bit is reserved for a sign, allowing for a larger range of positive values compared to signed integers of the same bit width.

An $N$-bit unsigned integer can take up values from $0$ to $2^N - 1$.

### Signed Integer Representation

**Signed integers** reserve one bit (typically the MSB) to indicate sign, reducing the range of representable positive values but enabling representation of negative numbers.

#### Two's Complement

**Two's complement** is the most common encoding system to represent signed integers. It treats the MSB exclusively as a sign bit:

- \\(\text{MSB} = 0\\): positive number (or zero)
- \\(\text{MSB} = 1\\): negative number

An \\(N\\)-bit unsigned integer using two's complement can take up values from \\(-2^{N-1}\\) to \\(2^{N-1} - 1\\).

To represent a number using two's complement:

1. Start with the binary representation of the positive number
2. Toggle all the bits
3. Add 1 to the result

> **Note**\
> When converting from binary to decimal, if the MSB is a 1, treat it as a negative number, then proceed as discussed in the "Any Base to Decimal" section.

## Floating-Point Representation

### IEE 754 Structure

For some number \\(x\\):
$$
x = (-1)^\text{sign} \times (\text{integer}.\text{fraction})_2 \times 2^\text{actual exponent}
$$

- Single precision (32 bits):
```
| 1 bit | 8 bits         |           23 bits                   |
  sign    biased exponent            fraction
```
- Double precision (64 bits):
```
| 1 bit |     11 bits      |             52 bits                |
  sign    biased exponent               fraction
```
- Quadruple precision (128 bits):
```
| 1 bit |     15 bits      |             112 bits                |
  sign    biased exponent               fraction
```

> **Note**\
> It may not be possible to store a given $x$ exactly with such a scheme whenever the actual exponent is outside of the possible range, or if the fraction field can't fit in the allocated number of bits (i.e., say for single precision, bits 24 and bits 25, where bit 0 is the implicit integer, are ones)

### Fields

- **Signed field**: Represents the positivity, either $0$ (for positive) or $1$ (for negative)
- **Biased exponent field**: Determines the power of $2$ by which to scale the mantissa. The exponent is biased, meaning we add a constant to the actual exponent.
    - Single precision bias: \\((2^{8 - 1} - 1)_{10} = (127)_{10}\\)
    - Double precision bias: \\((2^{11 - 1} - 1)_{10} = (1023)_{10}\\)
    - Quadruple precision bias: \\((2^{15 - 1} - 1)_{10} = (16383)_{10}\\)
    - Range of the actual exponents: \\([1 - \text{bias}, \text{bias}]\\)
- **Fraction field**: Represents the digits after the decimal point

### Normal Numbers

$$
x = (-1)^\text{sign} \times (1.\text{fraction})_2 \times 2^\text{actual exponent}
$$

```
| sign = any | exponent != 00000000 or exponent != 11111111 | fraction = any

```

### Special Numbers

#### \\(+\infty\\) and \\(-\infty\\)

**Infinity (\\(\infty\\))** is the result when calculations exceed the representable range (overflow) or a non-zero value is divided by zero. It's a useful value since it allows programs to continue running with meaningful results rather than crashing on overflow.

```
| sign = any | exponent = 11111111 | fraction = 000...0 |
```

#### NaN

**Not a Number (\\(\text{NaN}\\))**, is a value that represents mathematically undefined results.

It has some key properties:
- Any operation with \\(\text{NaN}\\) produces \\(\text{NaN}\\)
- \\(\text{NaN}\\) is never equal to anything, including itself

There are also two types of \\(\text{NaN}\\):
- Quiet \\(\text{NaN}\\): silently propagates \\(\text{NaN}\\) through calculations
- Signalling \\(\text{NaN}\\): triggers an exception or error when encountered in operations

**Quiet \\(\text{NaN}\\)**

```
| sign = any | exponent = 11111111 | fraction = 1<no restriction> |
```

**Signalling \\(\text{NaN}\\)**

```
| sign = any | exponent = 11111111 | fraction = 0<no restriction> |
```

#### 0

```
| sign = any | exponent = 00000000 | fraction = 000000...0 |
```

#### Denormalized (Subnormal) Numbers

$$
x = (-1)^\text{sign} \times (0.\text{fraction})_2 \times 2^\text{smallest possible actual exponent}
$$

```
| sign = any | exponent = 00000000 | fraction != 000...0 |
```

> **Note:** \\(+\infty\\) and \\(-\infty\\), \\(+0\\) and \\(-0\\) are not interchangeable!

### Converting Decimal to IEEE 754

To convert \\(12.375_{10}\\) to single precision IEEE 754:

**Step 1: Convert to binary**

$$
12.375_{10} = 1100.011_2
$$

**Step 2: Determine the sign**

Since it's a positive number, the sign bit is 0.

**Step 3: Normalize the fraction**

$$
1100.011_2 = 1.100011_2 \times 2^3
$$

The normalized fraction, padded to 23 bits, is: \\(10001100000000000000000_2\\)

**Step 4: Calculate the biased exponent**

$$
Biased exponent = actual exponent + bias =  3 + 127 = 130 = 10000010_2
$$

**Step 5: Combine all fields**

```
| Sign | Exponent | Mantissa                |
|  0   | 10000010 | 10001100000000000000000 |
```

Final result: \\(01000001010001100000000000000000_2\\)

## Integer Overflow and Integer Underflow

\\(N\\)-bit signed and unsigned integers have a certain range of values they may represent:

| Representation               | Range                             |
| ---------------------------- | --------------------------------- |
| Unsigned \\(N\\)-bit integer | \\([0, 2^n − 1]\\)                |
| Signed \\(N\\)-bit integer   | \\([−2^{n - 1}, 2^{n - 1} − 1]\\) |


**Integer overflow** occurs when an arithmetic operation produces a result larger than the maximum value representable with the given number of bits. **Integer underflow** occurs when an arithmetic operation produces a result smaller than the minimum value representable with the given number of bits.

In **Rust**, integer overflow and underflow produces panics, while release builds produce the following behaviour:
- Unsigned integer overflows or underflows: the result will just be modded by $2^n$, that is, `(a op b) mod 2^n` (this also applies in general to any arithmetic on unsigned integers)
- Signed integers: the result will just be modded by $2^n$, but then, the value is interpreted according to two's complement representation.

## Integer Byte Order

**Byte order**, also known as **endianness** of a system defines how multi-byte values are assign to memory addresses.
- **Big endian Byte Order**: The most significant byte is stored at the lowest memory address.
- **Little endian Byte Order**: The least significant byte is stored at the lowest memory address.

Assuming memory addresses increases from left to right:
- From a human-readable perspective, we conventionally interpret and write multi-byte values in big-endian order
- When examining the actual memory layout, the order of bytes depends on the system's endianness, where most modern computer architectures (such as x86 and ARM) use little-endian byte order.
