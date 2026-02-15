# Statistics and Probability Operations

Math.js provides comprehensive statistical analysis and probability functions supporting all numeric types including numbers, BigNumber, Fraction, and matrices. These 26 functions enable data analysis, random number generation, and probabilistic computations.

## Core Imports

```javascript { .api }
import {
  // Statistics
  mean, median, mode, std, variance, mad,
  min, max, sum, prod, count, cumsum, quantileSeq, corr,
  // Probability
  random, randomInt, pickRandom, bernoulli,
  factorial, gamma, lgamma, combinations, permutations,
  combinationsWithRep, multinomial, kldivergence
} from 'mathjs';
```

## Statistical Measures

### mean

Calculates the arithmetic mean (average) of values.

```typescript { .api }
function mean(matrix: MathCollection, dim?: number): number | BigNumber | Fraction | MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values
- `dim`: Dimension to calculate mean along (optional, for matrices)

**Returns:** Mean value, or array of means if dimension specified

**Usage Examples:**

```javascript
import { mean, matrix } from 'mathjs';

// Simple mean
mean([2, 4, 6, 8]);                    // 5
mean([1.5, 2.5, 3.5]);                 // 2.5

// Matrix mean (all elements)
mean([[1, 2, 3], [4, 5, 6]]);          // 3.5

// Mean along dimension 0 (columns)
mean([[1, 2, 3], [4, 5, 6]], 0);
// [2.5, 3.5, 4.5]

// Mean along dimension 1 (rows)
mean([[1, 2, 3], [4, 5, 6]], 1);
// [2, 5]

// Works with BigNumber for precision
mean([bignumber('0.1'), bignumber('0.2')]);
// BigNumber 0.15
```

### median

Calculates the median (middle value) of values.

```typescript { .api }
function median(matrix: MathCollection): number | BigNumber | Fraction | Unit;
```

**Parameters:**
- `matrix`: Array or matrix of values

**Returns:** Median value (middle value for odd length, average of middle two for even length)

**Usage Examples:**

```javascript
import { median } from 'mathjs';

// Odd number of values
median([1, 3, 5, 7, 9]);               // 5

// Even number of values (average of middle two)
median([1, 2, 3, 4]);                  // 2.5

// Unsorted data (automatically sorted)
median([5, 1, 9, 3, 7]);               // 5

// Works with matrices (flattens to 1D)
median([[1, 2], [3, 4]]);              // 2.5

// With units
median([unit('5 cm'), unit('10 cm'), unit('15 cm')]);
// Unit 10 cm
```

### mode

Finds the mode (most frequently occurring value) of values.

```typescript { .api }
function mode(matrix: MathCollection): number | BigNumber | Fraction | Unit | MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values

**Returns:** Mode value(s) - single value if one mode, array if multiple modes

**Usage Examples:**

```javascript
import { mode } from 'mathjs';

// Single mode
mode([1, 2, 3, 3, 4]);                 // 3
mode([1, 1, 1, 2, 2, 3]);              // 1

// Multiple modes (bimodal)
mode([1, 1, 2, 2, 3]);                 // [1, 2]

// All unique (all are modes)
mode([1, 2, 3, 4]);                    // [1, 2, 3, 4]

// Works with matrices
mode([[1, 2, 2], [3, 2, 4]]);          // 2
```

### std

Calculates the standard deviation of values.

```typescript { .api }
function std(matrix: MathCollection, dim?: number, normalization?: 'unbiased' | 'uncorrected' | 'biased'): number | BigNumber | Fraction | MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values
- `dim`: Dimension to calculate along (optional, for matrices)
- `normalization`: Normalization method (default: 'unbiased')
  - `'unbiased'`: Divide by (n-1), sample standard deviation
  - `'uncorrected'`: Divide by n, population standard deviation
  - `'biased'`: Alias for 'uncorrected'

**Returns:** Standard deviation value(s)

**Usage Examples:**

```javascript
import { std } from 'mathjs';

// Sample standard deviation (default, n-1)
std([2, 4, 6, 8]);                     // 2.5819...

// Population standard deviation (n)
std([2, 4, 6, 8], 'uncorrected');      // 2.2360...

// Along matrix dimensions
std([[1, 2, 3], [4, 5, 6]], 0);
// [2.1213..., 2.1213..., 2.1213...]

std([[1, 2, 3], [4, 5, 6]], 1);
// [1, 1]

// With BigNumber for precision
std([bignumber('1.5'), bignumber('2.5'), bignumber('3.5')]);
// BigNumber 1
```

### variance

Calculates the variance of values.

```typescript { .api }
function variance(matrix: MathCollection, dim?: number, normalization?: 'unbiased' | 'uncorrected' | 'biased'): number | BigNumber | Fraction | MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values
- `dim`: Dimension to calculate along (optional, for matrices)
- `normalization`: Normalization method (default: 'unbiased')
  - `'unbiased'`: Divide by (n-1), sample variance
  - `'uncorrected'`: Divide by n, population variance
  - `'biased'`: Alias for 'uncorrected'

**Returns:** Variance value(s)

**Usage Examples:**

```javascript
import { variance, sqrt } from 'mathjs';

// Sample variance (default, n-1)
variance([2, 4, 6, 8]);                // 6.6666...

// Population variance (n)
variance([2, 4, 6, 8], 'uncorrected'); // 5

// Variance is square of standard deviation
const v = variance([1, 2, 3, 4, 5]);
const s = std([1, 2, 3, 4, 5]);
sqrt(v);                                // Equals s

// Along matrix dimensions
variance([[1, 2, 3], [4, 5, 6]], 0);
// [4.5, 4.5, 4.5]

variance([[1, 2, 3], [4, 5, 6]], 1);
// [1, 1]
```

### mad

Calculates the mean absolute deviation (MAD) of values.

```typescript { .api }
function mad(matrix: MathCollection): number | BigNumber | Fraction | Unit;
```

**Parameters:**
- `matrix`: Array or matrix of values

**Returns:** Mean absolute deviation from the mean

**Usage Examples:**

```javascript
import { mad, mean, abs } from 'mathjs';

// Mean absolute deviation
mad([1, 2, 3, 4, 5]);                  // 1.2

// Measures average distance from mean
const data = [10, 20, 30, 40, 50];
mad(data);                              // 12
// Mean is 30, distances are [20,10,0,10,20], average is 12

// More robust to outliers than std
mad([1, 2, 3, 4, 100]);                // 19.2
std([1, 2, 3, 4, 100]);                 // 43.4... (much larger)

// Works with matrices (flattens)
mad([[1, 2], [3, 4]]);                 // 1
```

### quantileSeq

Calculates quantiles (percentiles) of sorted or unsorted data.

```typescript { .api }
function quantileSeq(matrix: MathCollection, prob: number | BigNumber | Array, sorted?: boolean): number | BigNumber | Fraction | Unit | MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values
- `prob`: Probability value(s) between 0 and 1, or array of probabilities
- `sorted`: Whether data is already sorted (default: false)

**Returns:** Quantile value(s) corresponding to probability

**Usage Examples:**

```javascript
import { quantileSeq } from 'mathjs';

const data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Median (50th percentile, 0.5 quantile)
quantileSeq(data, 0.5);                // 5.5

// First quartile (25th percentile)
quantileSeq(data, 0.25);               // 3.25

// Third quartile (75th percentile)
quantileSeq(data, 0.75);               // 7.75

// Multiple quantiles at once
quantileSeq(data, [0.25, 0.5, 0.75]);
// [3.25, 5.5, 7.75]

// 95th percentile
quantileSeq(data, 0.95);               // 9.55

// Already sorted data (optimization)
quantileSeq([1, 2, 3, 4, 5], 0.5, true);
// 3

// Works with unsorted data
quantileSeq([5, 1, 9, 3, 7], 0.5);     // 5
```

### corr

Calculates the Pearson correlation coefficient between two arrays.

```typescript { .api }
function corr(x: MathCollection, y: MathCollection): number | BigNumber;
```

**Parameters:**
- `x`: First array of values
- `y`: Second array of values (must be same length as x)

**Returns:** Correlation coefficient between -1 and 1

**Usage Examples:**

```javascript
import { corr } from 'mathjs';

// Perfect positive correlation
corr([1, 2, 3, 4], [2, 4, 6, 8]);      // 1

// Perfect negative correlation
corr([1, 2, 3, 4], [8, 6, 4, 2]);      // -1

// No correlation
corr([1, 2, 3, 4], [3, 1, 4, 2]);      // 0 (approximately)

// Strong positive correlation
corr([1, 2, 3, 4, 5], [2, 3, 4, 5, 7]);
// 0.9827... (strong positive)

// Real-world example: height vs weight
const heights = [150, 160, 170, 180, 190]; // cm
const weights = [50, 60, 70, 80, 90];      // kg
corr(heights, weights);                 // ~1 (strong correlation)
```

## Aggregate Functions

### sum

Calculates the sum of values.

```typescript { .api }
function sum(matrix: MathCollection, dim?: number): number | BigNumber | Fraction | MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values
- `dim`: Dimension to sum along (optional, for matrices)

**Returns:** Sum of all values, or array of sums if dimension specified

**Usage Examples:**

```javascript
import { sum } from 'mathjs';

// Simple sum
sum([1, 2, 3, 4]);                     // 10
sum([0.1, 0.2, 0.3]);                  // 0.6

// Matrix sum (all elements)
sum([[1, 2], [3, 4]]);                 // 10

// Sum along dimension 0 (columns)
sum([[1, 2, 3], [4, 5, 6]], 0);
// [5, 7, 9]

// Sum along dimension 1 (rows)
sum([[1, 2, 3], [4, 5, 6]], 1);
// [6, 15]

// Precise summation with BigNumber
sum([bignumber('0.1'), bignumber('0.2')]);
// BigNumber 0.3 (exact)

// Regular numbers have floating point errors
0.1 + 0.2;                              // 0.30000000000000004
```

### prod

Calculates the product of values.

```typescript { .api }
function prod(matrix: MathCollection, dim?: number): number | BigNumber | Fraction | MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values
- `dim`: Dimension to multiply along (optional, for matrices)

**Returns:** Product of all values, or array of products if dimension specified

**Usage Examples:**

```javascript
import { prod } from 'mathjs';

// Simple product
prod([2, 3, 4]);                       // 24
prod([1, 2, 3, 4, 5]);                 // 120 (factorial of 5)

// Matrix product (all elements)
prod([[1, 2], [3, 4]]);                // 24

// Product along dimension 0 (columns)
prod([[1, 2, 3], [4, 5, 6]], 0);
// [4, 10, 18]

// Product along dimension 1 (rows)
prod([[1, 2, 3], [4, 5, 6]], 1);
// [6, 120]

// With fractions
prod([fraction(1, 2), fraction(2, 3)]);
// Fraction 1/3
```

### count

Counts the number of elements in a matrix or array.

```typescript { .api }
function count(matrix: MathCollection): number;
```

**Parameters:**
- `matrix`: Array or matrix to count elements

**Returns:** Total number of elements

**Usage Examples:**

```javascript
import { count } from 'mathjs';

// Count array elements
count([1, 2, 3, 4]);                   // 4

// Count matrix elements (all dimensions)
count([[1, 2], [3, 4]]);               // 4
count([[1, 2, 3], [4, 5, 6]]);         // 6

// Counts total elements, not unique values
count([1, 1, 2, 2, 3]);                // 5

// Multi-dimensional arrays
count([[[1, 2], [3, 4]], [[5, 6], [7, 8]]]);
// 8

// Works with sparse matrices (counts non-zero)
count(sparse([[0, 1], [2, 0]]));       // 2
```

### cumsum

Calculates the cumulative sum of values.

```typescript { .api }
function cumsum(matrix: MathCollection, dim?: number): MathCollection;
```

**Parameters:**
- `matrix`: Array or matrix of values
- `dim`: Dimension to accumulate along (optional, for matrices)

**Returns:** Array or matrix of cumulative sums

**Usage Examples:**

```javascript
import { cumsum } from 'mathjs';

// Cumulative sum of array
cumsum([1, 2, 3, 4]);
// [1, 3, 6, 10]

cumsum([1, 1, 1, 1]);
// [1, 2, 3, 4]

// Running total
cumsum([10, 20, 30, 40]);
// [10, 30, 60, 100]

// Along matrix dimension 0 (down columns)
cumsum([[1, 2, 3], [4, 5, 6]], 0);
// [[1, 2, 3], [5, 7, 9]]

// Along matrix dimension 1 (across rows)
cumsum([[1, 2, 3], [4, 5, 6]], 1);
// [[1, 3, 6], [4, 9, 15]]

// Useful for calculating running totals in time series
const sales = [100, 150, 120, 180];
cumsum(sales);
// [100, 250, 370, 550] (cumulative sales)
```

### min

Finds the minimum value.

```typescript { .api }
function min(...values: Array<MathType>): MathType;
```

**Parameters:**
- `...values`: Two or more values, or arrays/matrices

**Returns:** Minimum value among all inputs

**Usage Examples:**

```javascript
import { min } from 'mathjs';

// Minimum of numbers
min(5, 2, 9, 1);                       // 1
min(-5, -2, -9);                       // -9

// Minimum of array
min([5, 2, 9, 1]);                     // 1

// Multiple arrays
min([1, 2], [3, 0], [5, 1]);           // 0

// Matrix minimum (all elements)
min([[5, 2], [9, 1]]);                 // 1

// Works with all numeric types
min(bignumber('1e100'), bignumber('2e100'));
// BigNumber 1e100

// With units (must be compatible)
min(unit('5 cm'), unit('2 cm'));       // Unit 2 cm
```

### max

Finds the maximum value.

```typescript { .api }
function max(...values: Array<MathType>): MathType;
```

**Parameters:**
- `...values`: Two or more values, or arrays/matrices

**Returns:** Maximum value among all inputs

**Usage Examples:**

```javascript
import { max } from 'mathjs';

// Maximum of numbers
max(5, 2, 9, 1);                       // 9
max(-5, -2, -9);                       // -2

// Maximum of array
max([5, 2, 9, 1]);                     // 9

// Multiple arrays
max([1, 2], [3, 0], [5, 1]);           // 5

// Matrix maximum (all elements)
max([[5, 2], [9, 1]]);                 // 9

// Works with all numeric types
max(bignumber('1e100'), bignumber('2e100'));
// BigNumber 2e100

// With units (must be compatible)
max(unit('5 cm'), unit('2 cm'));       // Unit 5 cm
```

## Probability and Random Numbers

### random

Generates random number(s) in a specified range.

```typescript { .api }
function random(size?: MathCollection, min?: number, max?: number): number | MathCollection;
function random(max?: number): number;
```

**Parameters:**
- `size`: Array dimensions for matrix of random numbers (optional)
- `min`: Minimum value (default: 0)
- `max`: Maximum value (default: 1)

**Returns:** Random number or matrix of random numbers in range [min, max)

**Usage Examples:**

```javascript
import { random } from 'mathjs';

// Single random number in [0, 1)
random();                              // 0.547...

// Random number in custom range [0, max)
random(100);                           // 73.2... (in [0, 100))

// Random number in [min, max)
random(-5, 5);                         // 2.1... (in [-5, 5))

// Array of random numbers
random([5]);                           // [0.234..., 0.891..., ...]

// Matrix of random numbers
random([2, 3]);
// [[0.12..., 0.45..., 0.78...],
//  [0.23..., 0.56..., 0.89...]]

// Random numbers in range with matrix size
random([3, 3], -1, 1);
// 3x3 matrix with values in [-1, 1)
```

### randomInt

Generates random integer(s) in a specified range.

```typescript { .api }
function randomInt(min: number, max?: number, size?: MathCollection): number | MathCollection;
function randomInt(max: number): number;
```

**Parameters:**
- `min`: Minimum value (inclusive), or max if only one argument
- `max`: Maximum value (exclusive)
- `size`: Array dimensions for matrix of random integers (optional)

**Returns:** Random integer or matrix of random integers in range [min, max)

**Usage Examples:**

```javascript
import { randomInt } from 'mathjs';

// Single random integer in [0, max)
randomInt(10);                         // 7 (integer in [0, 10))

// Random integer in [min, max)
randomInt(1, 7);                       // 4 (dice roll: [1, 7))

// Array of random integers
randomInt(0, 100, [5]);
// [23, 67, 45, 89, 12]

// Matrix of random integers
randomInt(1, 7, [2, 3]);
// [[4, 2, 6],
//  [1, 5, 3]]

// Simulating dice rolls
randomInt(1, 7);                       // 6-sided die
randomInt(1, 21);                      // 20-sided die

// Random indices for array
const arr = ['a', 'b', 'c', 'd', 'e'];
const idx = randomInt(arr.length);
arr[idx];                              // Random element
```

### pickRandom

Randomly picks one or more elements from an array.

```typescript { .api }
function pickRandom(array: MathCollection, count?: number | 'auto', weights?: MathCollection): MathType | MathCollection;
```

**Parameters:**
- `array`: Array to pick from
- `count`: Number of elements to pick (default: 1), or 'auto' to return single value for count=1
- `weights`: Array of weights for weighted random selection (optional)

**Returns:** Randomly selected element(s)

**Usage Examples:**

```javascript
import { pickRandom } from 'mathjs';

// Pick single random element
pickRandom([1, 2, 3, 4, 5]);           // 3 (random)

// Pick multiple random elements (with replacement)
pickRandom([1, 2, 3, 4, 5], 3);
// [2, 5, 2] (can repeat)

// Random selection from strings
pickRandom(['red', 'green', 'blue']);  // 'blue'

// Weighted random selection
pickRandom([1, 2, 3], 1, [1, 2, 3]);
// More likely to pick 3 (weight 3) than 1 (weight 1)

// Heavily weighted example
pickRandom(['rare', 'common'], 10, [1, 9]);
// Mostly 'common', occasionally 'rare'

// Random matrix element
pickRandom([[1, 2], [3, 4]]);          // 3 (random element)

// Shuffling: pick all elements
const arr = [1, 2, 3, 4, 5];
pickRandom(arr, arr.length);
// [3, 1, 5, 2, 4] (random order, with replacement)
```

### bernoulli

Generates a random sample from a Bernoulli distribution (0 or 1).

```typescript { .api }
function bernoulli(p: number): number;
```

**Parameters:**
- `p`: Probability of returning 1 (must be between 0 and 1)

**Returns:** 0 or 1 based on probability

**Usage Examples:**

```javascript
import { bernoulli } from 'mathjs';

// Fair coin flip (50% chance of 1)
bernoulli(0.5);                        // 0 or 1 (equally likely)

// Biased coin (75% chance of 1)
bernoulli(0.75);                       // 1 (more likely)

// Rare event (5% chance)
bernoulli(0.05);                       // 0 (95% of the time)

// Simulate 10 coin flips
Array.from({length: 10}, () => bernoulli(0.5));
// [1, 0, 1, 1, 0, 0, 1, 0, 1, 1]

// Simulate success/failure
const success = bernoulli(0.8);        // 80% success rate
if (success) {
  console.log('Operation succeeded');
} else {
  console.log('Operation failed');
}

// Bernoulli trials
const trials = 1000;
const p = 0.3;
const successes = Array.from({length: trials}, () => bernoulli(p))
  .reduce((a, b) => a + b, 0);
// successes ≈ 300 (30% of 1000)
```

## Combinatorics Functions

### factorial

Calculates the factorial of a number (n!).

```typescript { .api }
function factorial(n: number | BigNumber | Array | Matrix): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `n`: Non-negative integer value

**Returns:** Factorial of n (n! = n × (n-1) × ... × 2 × 1)

**Usage Examples:**

```javascript
import { factorial } from 'mathjs';

// Basic factorials
factorial(0);                          // 1 (by definition)
factorial(1);                          // 1
factorial(5);                          // 120 (5×4×3×2×1)
factorial(10);                         // 3628800

// Large factorials with BigNumber
factorial(bignumber(50));
// BigNumber 3.0414093201713376e+64

// Arrays
factorial([3, 4, 5]);
// [6, 24, 120]

// Matrix
factorial([[1, 2], [3, 4]]);
// [[1, 2], [6, 24]]

// Combinatorics: number of permutations
factorial(5);                          // 120 ways to arrange 5 items
```

### gamma

Calculates the gamma function Γ(n).

```typescript { .api }
function gamma(n: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `n`: Value to calculate gamma function for

**Returns:** Gamma function result (Γ(n) = (n-1)! for positive integers)

**Usage Examples:**

```javascript
import { gamma, factorial } from 'mathjs';

// For positive integers: Γ(n) = (n-1)!
gamma(5);                              // 24 (equals factorial(4))
gamma(6);                              // 120 (equals factorial(5))

// Gamma extends factorial to non-integers
gamma(0.5);                            // 1.772... (√π)
gamma(1.5);                            // 0.886... (√π/2)

// Useful mathematical constant
gamma(0.5) * gamma(0.5);               // π (approximately)

// Works with complex numbers
gamma(complex(2, 1));                  // Complex result

// Arrays
gamma([1, 2, 3, 4]);
// [1, 1, 2, 6]

// Used in probability distributions
// Beta function: B(a,b) = Γ(a)Γ(b)/Γ(a+b)
```

### lgamma

Calculates the natural logarithm of the gamma function.

```typescript { .api }
function lgamma(n: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `n`: Value to calculate log-gamma for

**Returns:** ln(Γ(n)), logarithm of gamma function

**Usage Examples:**

```javascript
import { lgamma, gamma, log, exp } from 'mathjs';

// Log-gamma is more numerically stable for large values
lgamma(100);                           // 359.1342... (much smaller than gamma(100))
exp(lgamma(100));                      // gamma(100) (very large)

// Relationship to gamma
log(gamma(5));                         // Equals lgamma(5)

// Useful for large factorials
lgamma(171);                           // 701.43... (log of 170!)
// gamma(171) would overflow

// Statistical applications
// Log-likelihood calculations in statistics
lgamma([1, 2, 3, 4, 5]);
// [0, 0, 0.693..., 1.791..., 3.178...]

// Stirling's approximation verification
// lgamma(n) ≈ n*log(n) - n for large n
```

### combinations

Calculates binomial coefficient C(n, k) - number of ways to choose k items from n.

```typescript { .api }
function combinations(n: number | BigNumber, k: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `n`: Total number of items
- `k`: Number of items to choose

**Returns:** Number of combinations (n! / (k! × (n-k)!))

**Usage Examples:**

```javascript
import { combinations } from 'mathjs';

// Choose 2 from 5
combinations(5, 2);                    // 10
// {1,2}, {1,3}, {1,4}, {1,5}, {2,3}, {2,4}, {2,5}, {3,4}, {3,5}, {4,5}

// Lottery: choose 6 numbers from 49
combinations(49, 6);                   // 13983816

// Pascal's triangle
combinations(4, 0);                    // 1
combinations(4, 1);                    // 4
combinations(4, 2);                    // 6
combinations(4, 3);                    // 4
combinations(4, 4);                    // 1

// Symmetric property: C(n,k) = C(n,n-k)
combinations(10, 3);                   // 120
combinations(10, 7);                   // 120

// Large combinations with BigNumber
combinations(bignumber(100), bignumber(50));
// BigNumber 1.00891344545564e+29

// Poker: 5 cards from 52
combinations(52, 5);                   // 2598960 possible hands
```

### combinationsWithRep

Calculates combinations with replacement (multiset coefficient).

```typescript { .api }
function combinationsWithRep(n: number | BigNumber, k: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `n`: Number of types of items
- `k`: Number of items to choose

**Returns:** Number of k-combinations with repetition from n items

**Usage Examples:**

```javascript
import { combinationsWithRep } from 'mathjs';

// Choose 2 items from 3 types (with replacement)
combinationsWithRep(3, 2);             // 6
// {1,1}, {1,2}, {1,3}, {2,2}, {2,3}, {3,3}

// Ice cream: 3 scoops from 5 flavors (can repeat)
combinationsWithRep(5, 3);             // 35

// Distribute k identical balls into n bins
combinationsWithRep(4, 3);             // 20
// (number of ways to put 3 balls in 4 bins)

// Dice: rolling 2 six-sided dice (order doesn't matter)
combinationsWithRep(6, 2);             // 21 distinct outcomes

// Stars and bars problem
// Ways to make n using k non-negative integers
combinationsWithRep(10, 3);            // 66
// (ways to write n = a + b + c where a,b,c ≥ 0)
```

### permutations

Calculates number of permutations P(n, k) - ordered arrangements.

```typescript { .api }
function permutations(n: number | BigNumber, k?: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `n`: Total number of items
- `k`: Number of items to arrange (default: n, for all items)

**Returns:** Number of permutations (n! / (n-k)!)

**Usage Examples:**

```javascript
import { permutations, factorial } from 'mathjs';

// Arrange 3 from 5
permutations(5, 3);                    // 60 (5×4×3)

// Arrange all items (n!)
permutations(5);                       // 120 (equals factorial(5))
permutations(5, 5);                    // 120 (same as above)

// Race: 3 medals from 8 runners
permutations(8, 3);                    // 336 (gold, silver, bronze)

// Password: 4 digits, no repeating
permutations(10, 4);                   // 5040

// Difference from combinations
combinations(5, 3);                    // 10 (order doesn't matter)
permutations(5, 3);                    // 60 (order matters)

// Anagrams: letters in "CAT"
permutations(3);                       // 6 (CAT, CTA, ACT, ATC, TAC, TCA)

// Large permutations with BigNumber
permutations(bignumber(100), bignumber(5));
// BigNumber 9.0345024e+9
```

### multinomial

Calculates multinomial coefficient - generalizes binomial to multiple groups.

```typescript { .api }
function multinomial(...args: Array<number | BigNumber>): number | BigNumber;
```

**Parameters:**
- `...args`: Group sizes (must sum to total n)

**Returns:** Multinomial coefficient (n! / (k₁! × k₂! × ... × kₘ!))

**Usage Examples:**

```javascript
import { multinomial } from 'mathjs';

// Divide 5 items into groups of 2, 2, 1
multinomial(2, 2, 1);                  // 30
// (5! / (2! × 2! × 1!) = 120 / 4 = 30)

// Anagrams of "MISSISSIPPI" (11 letters)
// M:1, I:4, S:4, P:2
multinomial(1, 4, 4, 2);               // 34650

// Binomial as special case
multinomial(3, 2);                     // 10
combinations(5, 3);                    // 10 (same: C(n,k) = M(k,n-k))

// Distribute 10 items into 3 groups
multinomial(3, 3, 4);                  // 4200
// (10! / (3! × 3! × 4!))

// Card dealing: 13 cards to 4 players
multinomial(13, 13, 13, 13);
// Number of ways to deal 52 cards equally

// Trinomial expansion term coefficients
multinomial(2, 1, 1);                  // 12
// Coefficient of x²yz in (x+y+z)⁴
```

### kldivergence

Calculates Kullback-Leibler divergence between two probability distributions.

```typescript { .api }
function kldivergence(p: MathCollection, q: MathCollection): number;
```

**Parameters:**
- `p`: First probability distribution (true distribution)
- `q`: Second probability distribution (approximating distribution)

**Returns:** KL divergence D_KL(P‖Q) = Σ p(i) × log(p(i) / q(i))

**Usage Examples:**

```javascript
import { kldivergence } from 'mathjs';

// Identical distributions (KL = 0)
kldivergence([0.5, 0.5], [0.5, 0.5]);  // 0

// Different distributions
kldivergence([0.7, 0.3], [0.5, 0.5]);  // 0.0853... (bits)

// Measure how different distributions are
const true_dist = [0.1, 0.2, 0.3, 0.4];
const approx_dist = [0.2, 0.2, 0.3, 0.3];
kldivergence(true_dist, approx_dist);  // 0.0513...

// Information loss when using Q instead of P
const p = [0.8, 0.1, 0.1];
const q = [0.4, 0.3, 0.3];
kldivergence(p, q);                    // 0.4179...

// Asymmetric: KL(P‖Q) ≠ KL(Q‖P)
kldivergence([0.7, 0.3], [0.3, 0.7]);  // 0.5108...
kldivergence([0.3, 0.7], [0.7, 0.3]);  // 0.5108... (same by symmetry of this example)

// Machine learning: comparing predicted vs actual distributions
const actual = [0.1, 0.6, 0.3];
const predicted = [0.2, 0.5, 0.3];
kldivergence(actual, predicted);       // Information loss
```

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathScalarType = MathNumericType | Unit;
type MathArray<T> = T[] | Array<MathArray<T>>;
type MathCollection = MathArray<any> | Matrix;
type MathType = MathScalarType | MathCollection;
```

## Statistical Analysis Example

```javascript
import {
  mean, median, mode, std, variance, min, max,
  quantileSeq, corr
} from 'mathjs';

// Sample dataset: test scores
const scores = [85, 92, 78, 90, 88, 76, 95, 89, 84, 91];

// Descriptive statistics
const stats = {
  mean: mean(scores),              // 86.8
  median: median(scores),          // 88.5
  mode: mode(scores),              // All unique
  std: std(scores),                // 5.97...
  variance: variance(scores),      // 35.73...
  min: min(scores),                // 76
  max: max(scores),                // 95
  range: max(scores) - min(scores) // 19
};

// Quartiles
const q1 = quantileSeq(scores, 0.25);  // 84.25 (25th percentile)
const q2 = quantileSeq(scores, 0.5);   // 88.5 (median)
const q3 = quantileSeq(scores, 0.75);  // 91 (75th percentile)
const iqr = q3 - q1;                    // 6.75 (interquartile range)

// Correlation between two variables
const study_hours = [2, 4, 1, 3, 3, 1, 5, 3, 2, 4];
const correlation = corr(study_hours, scores);
// 0.89... (strong positive correlation)
```

## Probability Simulation Example

```javascript
import {
  random, randomInt, pickRandom, bernoulli,
  combinations, permutations, mean, std
} from 'mathjs';

// Monte Carlo simulation: coin flips
function simulateCoinFlips(n, p, trials) {
  const results = [];
  for (let i = 0; i < trials; i++) {
    const flips = Array.from({length: n}, () => bernoulli(p));
    const heads = flips.reduce((a, b) => a + b, 0);
    results.push(heads);
  }
  return results;
}

// Simulate 10 coin flips, 1000 times
const outcomes = simulateCoinFlips(10, 0.5, 1000);
const avgHeads = mean(outcomes);      // ~5 (expected value)
const stdHeads = std(outcomes);       // ~1.58 (theoretical: sqrt(n*p*(1-p)))

// Card game probabilities
const deck = Array.from({length: 52}, (_, i) => i);
const hand = pickRandom(deck, 5);     // Random poker hand

// Calculate theoretical probability
const totalHands = combinations(52, 5);        // 2598960
const flushes = 4 * combinations(13, 5);       // 5148
const flushProbability = flushes / totalHands;  // 0.00198...

// Random walk simulation
function randomWalk(steps) {
  let position = 0;
  const path = [position];
  for (let i = 0; i < steps; i++) {
    position += randomInt(-1, 2);  // -1, 0, or 1
    path.push(position);
  }
  return path;
}

const walk = randomWalk(100);  // 100-step random walk
```
