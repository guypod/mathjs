# Combinatorics and Special Functions

Functions for combinatorial mathematics including factorials, permutations, combinations, special sequences, and special mathematical functions like gamma and zeta.

## Capabilities

### Basic Combinatorics

Fundamental combinatorial functions.

```javascript { .api }
/**
 * Calculate factorial (n!)
 * @param n - Non-negative integer
 * @returns n! = n × (n-1) × ... × 2 × 1
 */
function factorial(n: number | BigNumber | bigint): number | BigNumber | bigint
function factorial(n: MathCollection): MathCollection

/**
 * Calculate combinations (binomial coefficient)
 * C(n,k) = n! / (k! × (n-k)!)
 * @param n - Total number of items
 * @param k - Number of items to choose
 * @returns Number of combinations
 */
function combinations(n: number | BigNumber, k: number | BigNumber): number | BigNumber
function combinations(n: MathCollection, k: MathCollection): MathCollection

/**
 * Calculate permutations
 * P(n,k) = n! / (n-k)!
 * @param n - Total number of items
 * @param k - Number of items to arrange (default: n)
 * @returns Number of permutations
 */
function permutations(n: number | BigNumber, k?: number | BigNumber): number | BigNumber
function permutations(n: MathCollection, k?: MathCollection): MathCollection

/**
 * Calculate multinomial coefficient
 * n! / (k1! × k2! × ... × km!)
 * @param a - Array of partition sizes
 * @returns Multinomial coefficient
 */
function multinomial(a: number[] | BigNumber[]): number | BigNumber
```

**Usage Examples:**

```javascript
import { factorial, combinations, permutations, multinomial } from 'mathjs'

// Factorial
factorial(5)                  // 120 (5! = 5×4×3×2×1)
factorial(0)                  // 1 (by definition)
factorial(10)                 // 3628800

// Combinations (choose k from n)
combinations(5, 2)            // 10 (C(5,2) = 5!/(2!×3!))
combinations(10, 3)           // 120 (ways to choose 3 from 10)
combinations(52, 5)           // 2598960 (poker hands)

// Permutations (arrange k from n)
permutations(5, 2)            // 20 (P(5,2) = 5!/3!)
permutations(5)               // 120 (P(5,5) = 5!)
permutations(10, 3)           // 720 (ways to arrange 3 from 10)

// Multinomial coefficient
multinomial([2, 3, 4])        // 1260 (9!/(2!×3!×4!))
multinomial([1, 1, 1])        // 6 (3!/(1!×1!×1!))
```

### Special Sequences

Combinatorial number sequences.

```javascript { .api }
/**
 * Calculate Bell numbers (number of partitions of a set)
 * @param n - Input value
 * @returns nth Bell number
 */
function bellNumbers(n: number): number

/**
 * Calculate Catalan numbers
 * C(n) = (2n)! / ((n+1)! × n!)
 * @param n - Input value
 * @returns nth Catalan number
 */
function catalan(n: number | BigNumber): number | BigNumber

/**
 * Calculate Stirling numbers of the second kind
 * S(n,k) = number of ways to partition n items into k non-empty sets
 * @param n - Number of items
 * @param k - Number of sets
 * @returns Stirling number S(n,k)
 */
function stirlingS2(n: number | BigNumber, k: number | BigNumber): number | BigNumber

/**
 * Calculate Bernoulli numbers
 * @param n - Index
 * @returns nth Bernoulli number
 */
function bernoulli(n: number): number | BigNumber

/**
 * Calculate composition
 * Number of ways to write n as sum of k positive integers
 * @param n - Total value
 * @param k - Number of parts
 * @returns Number of compositions
 */
function composition(n: number | BigNumber, k: number | BigNumber): number | BigNumber
```

**Usage Examples:**

```javascript
import { bellNumbers, catalan, stirlingS2, bernoulli, composition } from 'mathjs'

// Bell numbers (partitions of a set)
bellNumbers(0)                // 1
bellNumbers(1)                // 1
bellNumbers(2)                // 2 ({{1,2}, {1},{2}})
bellNumbers(3)                // 5
bellNumbers(5)                // 52

// Catalan numbers
catalan(0)                    // 1
catalan(3)                    // 5
catalan(4)                    // 14
catalan(5)                    // 42
// Applications: balanced parentheses, binary trees, etc.

// Stirling numbers of the second kind
stirlingS2(5, 2)              // 15 (ways to partition 5 items into 2 sets)
stirlingS2(6, 3)              // 90
stirlingS2(4, 4)              // 1

// Bernoulli numbers
bernoulli(0)                  // 1
bernoulli(1)                  // -0.5
bernoulli(2)                  // 0.166... (1/6)

// Compositions
composition(5, 3)             // 6 (ways to write 5 as sum of 3 positive integers)
composition(10, 4)            // 84
```

### Gamma and Related Functions

Gamma function and extensions.

```javascript { .api }
/**
 * Calculate gamma function Γ(x)
 * Extends factorial to real/complex numbers: Γ(n) = (n-1)!
 * @param x - Input value
 * @returns Γ(x)
 */
function gamma(x: number): number
function gamma(x: BigNumber): BigNumber
function gamma(x: Complex): Complex
function gamma(x: MathCollection): MathCollection

/**
 * Calculate natural logarithm of gamma function
 * More numerically stable for large values
 * @param x - Input value
 * @returns ln(Γ(x))
 */
function lgamma(x: number): number
function lgamma(x: BigNumber): BigNumber
```

**Usage Examples:**

```javascript
import { gamma, lgamma, factorial } from 'mathjs'

// Gamma function
gamma(5)                      // 24 (Γ(5) = 4!)
gamma(0.5)                    // 1.772... (√π)
gamma(1)                      // 1 (Γ(1) = 0!)
gamma(3.5)                    // 3.323... (extends factorial to reals)

// Relation to factorial
gamma(6)                      // 120 (same as factorial(5))
factorial(5)                  // 120

// Log-gamma (more stable for large values)
lgamma(100)                   // 359.134... (ln(99!))
lgamma(1000)                  // 5905.22...

// Half-integer values
gamma(1.5)                    // 0.886... (√π/2)
gamma(2.5)                    // 1.329... (3√π/4)
```

### Error Function

Error function for statistics and probability.

```javascript { .api }
/**
 * Calculate error function
 * erf(x) = (2/√π) ∫₀ˣ e^(-t²) dt
 * @param x - Input value
 * @returns erf(x)
 */
function erf(x: number): number
function erf(x: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { erf } from 'mathjs'

// Error function
erf(0)                        // 0
erf(1)                        // 0.8427... (68% of normal distribution within ±1σ)
erf(2)                        // 0.9953... (95% within ±2σ)
erf(Infinity)                 // 1
erf(-1)                       // -0.8427... (odd function)

// Calculate standard normal cumulative distribution
function normalCDF(x) {
  return 0.5 * (1 + erf(x / Math.sqrt(2)))
}
normalCDF(0)                  // 0.5 (median)
normalCDF(1)                  // 0.8413... (84.13% below 1σ)
```

### Riemann Zeta Function

Zeta function for number theory.

```javascript { .api }
/**
 * Calculate Riemann zeta function
 * ζ(s) = Σ(1/n^s) for n=1 to ∞
 * @param s - Input value (s > 1 for real numbers)
 * @returns ζ(s)
 */
function zeta(s: number): number
function zeta(s: BigNumber): BigNumber
function zeta(s: Complex): Complex
```

**Usage Examples:**

```javascript
import { zeta, evaluate } from 'mathjs'

// Zeta function
zeta(2)                       // 1.6449... (π²/6, Basel problem)
zeta(3)                       // 1.2020... (Apéry's constant)
zeta(4)                       // 1.0823... (π⁴/90)
zeta(10)                      // 1.0009...

// Relation to prime numbers (Euler product)
// ζ(s) = Π(1/(1-p^(-s))) for all primes p
```

## Combinatorial Applications

### Probability Calculations

```javascript
import { combinations, permutations, factorial } from 'mathjs'

// Probability of poker hands
const deckSize = 52
const handSize = 5
const totalHands = combinations(deckSize, handSize)  // 2,598,960

// Royal flush (4 ways, one per suit)
const royalFlush = 4
const pRoyalFlush = royalFlush / totalHands  // 0.000154%

// Four of a kind
const fourOfKind = 13 * combinations(48, 1)  // 624
const pFourOfKind = fourOfKind / totalHands  // 0.024%

// Lottery odds (choose 6 from 49)
const lotteryOdds = combinations(49, 6)      // 13,983,816
const pWinning = 1 / lotteryOdds             // 0.00000715%
```

### Counting Problems

```javascript
import { permutations, combinations, factorial, multinomial } from 'mathjs'

// Seating arrangements
const people = 8
const arrangements = factorial(people)       // 40,320 ways

// Circular arrangements (account for rotation)
const circularArrangements = factorial(people - 1)  // 5,040 ways

// Team selection (choose 5 from 12)
const teamWays = combinations(12, 5)         // 792 ways

// Assigning roles (order matters)
const roleAssignments = permutations(12, 5)  // 95,040 ways

// Distributing items into groups
const distribute = multinomial([3, 4, 2])    // 1,260 ways
```

### Recursive Sequences

```javascript
import { catalan, bellNumbers, stirlingS2 } from 'mathjs'

// Binary tree structures with n internal nodes
const treeStructures = catalan(5)            // 42

// Balanced parentheses with n pairs
const validParentheses = catalan(4)          // 14
// e.g., ((())), (()()), (())(), ()(()), ()()()

// Set partitions
const partitions = bellNumbers(4)            // 15
// All ways to partition {1,2,3,4}

// Partitions into exactly k subsets
const kPartitions = stirlingS2(5, 3)         // 25
// Ways to partition 5 items into 3 non-empty subsets
```

## Working with Large Numbers

Use BigNumber for large combinatorial calculations:

```javascript
import { factorial, combinations, bignumber } from 'mathjs'

// Large factorials
factorial(bignumber(100))     // Very large number
factorial(bignumber(1000))    // Even larger

// Large combinations
combinations(bignumber(100), bignumber(50))
// 100891344545564193334812497256 (exact)

// Overflow with regular numbers
factorial(170)                // Infinity (exceeds number range)
factorial(bignumber(170))     // Exact BigNumber result
```

## Special Function Applications

### Normal Distribution

```javascript
import { erf, sqrt, exp, pi } from 'mathjs'

// Standard normal probability density function
function normalPDF(x) {
  return exp(-x * x / 2) / sqrt(2 * pi)
}

// Standard normal cumulative distribution function
function normalCDF(x) {
  return 0.5 * (1 + erf(x / sqrt(2)))
}

// Calculate probabilities
normalCDF(1.96)               // 0.975 (95th percentile)
normalCDF(-1.96)              // 0.025 (5th percentile)
```

### Stirling's Approximation

```javascript
import { factorial, gamma, ln, exp, pi, sqrt } from 'mathjs'

// Stirling's approximation: n! ≈ √(2πn) × (n/e)^n
function stirlingApprox(n) {
  return sqrt(2 * pi * n) * exp(n * (ln(n) - 1))
}

// Compare with exact factorial
const exact = factorial(10)               // 3628800
const approx = stirlingApprox(10)         // 3598695.6...
const error = abs((exact - approx) / exact)  // 0.83% error

// Better for large n
stirlingApprox(100)           // Very close to factorial(100)
```

## Performance Considerations

- `factorial` grows very quickly; use BigNumber for n > 20
- `combinations` and `permutations` can overflow for large inputs
- `gamma` is computationally expensive for complex arguments
- Use `lgamma` instead of `ln(gamma(x))` for numerical stability
- Special sequences like `catalan` and `bellNumbers` use memoization for efficiency
