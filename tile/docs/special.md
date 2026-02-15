# Special Functions

Math.js provides special mathematical functions including error functions, zeta functions, and combinatorics functions. These functions are used in probability theory, number theory, and advanced mathematics.

## Core Imports

```javascript { .api }
import {
  erf, zeta,
  bellNumbers, catalan, composition, stirlingS2
} from 'mathjs';
```

## Error and Zeta Functions

### erf

Calculates the error function (erf), used in probability and statistics.

```typescript { .api }
function erf(x: number | Array | Matrix): number | Array | Matrix;
```

**Parameters:**
- `x`: Value to calculate error function for

**Returns:** Error function erf(x)

**Description:**
The error function is defined as:
erf(x) = (2/√π) ∫₀ˣ e^(-t²) dt

It represents the probability that a random variable from a standard normal distribution falls within [-x, x].

**Usage Examples:**

```javascript
import { erf } from 'mathjs';

erf(0);                           // 0
erf(1);                           // 0.8427007929497149
erf(-1);                          // -0.8427007929497149
erf(Infinity);                    // 1
erf(-Infinity);                   // -1

// Common values in statistics
erf(1 / Math.sqrt(2));            // ≈ 0.6827 (68.27% within 1σ)
erf(2 / Math.sqrt(2));            // ≈ 0.9545 (95.45% within 2σ)

// Element-wise for arrays
erf([0, 0.5, 1, 1.5]);
// [0, 0.5205, 0.8427, 0.9661]

// Used in probability calculations
// P(-x < Z < x) = erf(x / sqrt(2)) for standard normal Z
```

### zeta

Calculates the Riemann zeta function.

```typescript { .api }
function zeta(s: number): number;
```

**Parameters:**
- `s`: Complex variable (currently only real numbers supported)

**Returns:** Riemann zeta function ζ(s)

**Description:**
The Riemann zeta function is defined as:
ζ(s) = Σ(n=1 to ∞) 1/n^s for Re(s) > 1

It has important applications in number theory and is central to the Riemann Hypothesis.

**Usage Examples:**

```javascript
import { zeta } from 'mathjs';

// Famous values
zeta(2);                          // π²/6 ≈ 1.6449340668482264
zeta(4);                          // π⁴/90 ≈ 1.0823232337111381
zeta(0);                          // -0.5

// Large values approach 1
zeta(10);                         // ≈ 1.0009945751278181
zeta(100);                        // ≈ 1.0

// Used in number theory
// Related to prime number distribution
```

## Combinatorial Special Functions

### bellNumbers

Calculates the nth Bell number (number of partitions of a set).

```typescript { .api }
function bellNumbers(n: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `n`: Non-negative integer (set size)

**Returns:** Bell number B(n)

**Description:**
Bell numbers count the number of ways to partition a set of n elements into non-empty subsets. B(0) = 1.

**Usage Examples:**

```javascript
import { bellNumbers, bignumber } from 'mathjs';

bellNumbers(0);                   // 1 (one partition of empty set)
bellNumbers(1);                   // 1 (one partition: {1})
bellNumbers(2);                   // 2 (partitions: {1,2} or {1},{2})
bellNumbers(3);                   // 5
bellNumbers(4);                   // 15
bellNumbers(5);                   // 52

// Sequence: 1, 1, 2, 5, 15, 52, 203, 877, ...

// Large values with BigNumber
bellNumbers(bignumber(10));       // BigNumber 115975

// Grows very rapidly
bellNumbers(20);                  // 51724158235372
```

### catalan

Calculates the nth Catalan number.

```typescript { .api }
function catalan(n: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `n`: Non-negative integer

**Returns:** Catalan number C(n)

**Description:**
Catalan numbers have many combinatorial interpretations:
- Number of valid parentheses expressions with n pairs
- Number of binary trees with n internal nodes
- Number of paths from (0,0) to (n,n) that don't cross diagonal

Formula: C(n) = (2n)! / ((n+1)! × n!)

**Usage Examples:**

```javascript
import { catalan, bignumber } from 'mathjs';

catalan(0);                       // 1
catalan(1);                       // 1
catalan(2);                       // 2
catalan(3);                       // 5
catalan(4);                       // 14
catalan(5);                       // 42

// Sequence: 1, 1, 2, 5, 14, 42, 132, 429, ...

// Examples:
// C(3) = 5 valid parentheses:
// ()()(), ()(()), (())(), (()()), ((()))

// Large values
catalan(10);                      // 16796
catalan(bignumber(20));           // BigNumber 6564120420
```

### composition

Calculates the number of compositions of n into k parts.

```typescript { .api }
function composition(n: number | BigNumber, k: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `n`: Positive integer to be partitioned
- `k`: Number of parts (positive integer)

**Returns:** Number of compositions of n into k parts

**Description:**
A composition is an ordered partition. The number of ways to write n as a sum of k positive integers where order matters.

Formula: C(n-1, k-1) where C is binomial coefficient

**Usage Examples:**

```javascript
import { composition } from 'mathjs';

// Compositions of 5 into 3 parts
composition(5, 3);                // 6
// [1,1,3], [1,2,2], [1,3,1], [2,1,2], [2,2,1], [3,1,1]

composition(4, 2);                // 3
// [1,3], [2,2], [3,1]

composition(6, 3);                // 10

// Edge cases
composition(5, 1);                // 1 (only [5])
composition(5, 5);                // 1 (only [1,1,1,1,1])

// Related to binomial coefficient
composition(10, 4);               // C(9, 3) = 84
```

### stirlingS2

Calculates Stirling numbers of the second kind.

```typescript { .api }
function stirlingS2(n: number | BigNumber, k: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `n`: Total number of elements (non-negative integer)
- `k`: Number of non-empty subsets (non-negative integer)

**Returns:** Stirling number of second kind S(n, k)

**Description:**
S(n, k) counts the number of ways to partition a set of n elements into exactly k non-empty subsets.

Properties:
- S(n, 0) = 0 for n > 0
- S(0, 0) = 1
- S(n, 1) = 1
- S(n, n) = 1
- S(n, k) = k × S(n-1, k) + S(n-1, k-1)

**Usage Examples:**

```javascript
import { stirlingS2, bignumber } from 'mathjs';

// Partition 3 elements into 2 subsets
stirlingS2(3, 2);                 // 3
// {1,2},{3} or {1,3},{2} or {2,3},{1}

stirlingS2(4, 2);                 // 7

// Edge cases
stirlingS2(5, 0);                 // 0
stirlingS2(5, 1);                 // 1 (all in one subset)
stirlingS2(5, 5);                 // 1 (each in own subset)

// Triangle of values
stirlingS2(4, 1);                 // 1
stirlingS2(4, 2);                 // 7
stirlingS2(4, 3);                 // 6
stirlingS2(4, 4);                 // 1

// Large values
stirlingS2(10, 5);                // 42525
stirlingS2(bignumber(20), bignumber(10));
// BigNumber 1978261657756160

// Related to Bell numbers: B(n) = Σ S(n,k) for k=0 to n
```

## Common Applications

These special functions are used in:

- **erf**: Normal distribution calculations, error analysis
- **zeta**: Number theory, prime distribution, physics
- **bellNumbers**: Set partitions, topology
- **catalan**: Combinatorics, tree structures, path counting
- **composition**: Integer partitions (ordered)
- **stirlingS2**: Set partitions, distribution problems

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathArray<T> = T[] | Array<MathArray<T>>;
```
