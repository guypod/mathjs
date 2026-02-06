# Statistics and Probability

Statistical functions for data analysis including descriptive statistics, probability distributions, and random number generation. Functions support both scalar and array inputs with optional dimension selection.

## Capabilities

### Descriptive Statistics

Statistical measures for analyzing data distributions.

```javascript { .api }
/**
 * Calculate the mean (average) value
 * @param args - Values to average
 * @returns Mean value
 */
function mean(...args: MathType[]): MathType
function mean(A: MathCollection, dimension?: number): MathCollection | number

/**
 * Calculate the median value
 * @param args - Values
 * @returns Median value
 */
function median(...args: MathType[]): MathType
function median(A: MathCollection): number

/**
 * Calculate the mode (most frequent value)
 * @param args - Values
 * @returns Mode value(s)
 */
function mode(...args: MathType[]): MathType
function mode(A: MathCollection): MathCollection

/**
 * Calculate the standard deviation
 * @param array - Input values
 * @param dim - Dimension to calculate along (for matrices)
 * @param normalization - 'unbiased' (default), 'uncorrected', or 'biased'
 * @returns Standard deviation
 */
function std(
  array: MathCollection,
  dim?: number,
  normalization?: 'unbiased' | 'uncorrected' | 'biased'
): number | MathCollection

/**
 * Calculate the variance
 * @param array - Input values
 * @param dim - Dimension to calculate along (for matrices)
 * @param normalization - 'unbiased' (default), 'uncorrected', or 'biased'
 * @returns Variance
 */
function variance(
  array: MathCollection,
  dim?: number,
  normalization?: 'unbiased' | 'uncorrected' | 'biased'
): number | MathCollection

/**
 * Calculate median absolute deviation
 * @param array - Input values
 * @returns Median absolute deviation
 */
function mad(array: MathCollection): number

/**
 * Calculate quantiles
 * @param A - Input data (must be sorted or use sorted=false)
 * @param prob - Probability or array of probabilities [0, 1]
 * @param sorted - Whether input is already sorted (default: false)
 * @returns Quantile value(s)
 */
function quantileSeq(
  A: MathCollection,
  prob: number | MathCollection,
  sorted?: boolean
): number | MathCollection
```

**Usage Examples:**

```javascript
import { mean, median, mode, std, variance, mad, quantileSeq } from 'mathjs'

const data = [1, 2, 3, 4, 5, 6, 7, 8, 9]

mean(data)                    // 5
median(data)                  // 5
mode([1, 2, 2, 3, 3, 3])     // [3]
std(data)                     // 2.738...
variance(data)                // 7.5
mad(data)                     // 2.5

// Quantiles
quantileSeq(data, 0.5)        // 5 (median)
quantileSeq(data, [0.25, 0.5, 0.75])  // [2.5, 5, 7.5]

// Matrix operations by dimension
const matrix = [[1, 2, 3], [4, 5, 6]]
mean(matrix, 0)               // [2.5, 3.5, 4.5] (column means)
mean(matrix, 1)               // [2, 5] (row means)
```

### Aggregation Functions

Sum, product, count, min, and max operations.

```javascript { .api }
/**
 * Calculate the sum of values
 * @param args - Values to sum
 * @returns Sum
 */
function sum(...args: MathType[]): MathType
function sum(A: MathCollection, dim?: number): MathCollection | MathType

/**
 * Calculate the product of values
 * @param args - Values to multiply
 * @returns Product
 */
function prod(...args: MathType[]): MathType
function prod(A: MathCollection, dim?: number): MathCollection | MathType

/**
 * Count the number of elements
 * @param x - Value or collection
 * @returns Count of elements
 */
function count(x: MathType): number

/**
 * Find the maximum value
 * @param args - Values to compare
 * @returns Maximum value
 */
function max(...args: MathType[]): MathType
function max(A: MathCollection, dim?: number): MathCollection | MathType

/**
 * Find the minimum value
 * @param args - Values to compare
 * @returns Minimum value
 */
function min(...args: MathType[]): MathType
function min(A: MathCollection, dim?: number): MathCollection | MathType
```

**Usage Examples:**

```javascript
import { sum, prod, count, max, min } from 'mathjs'

sum(1, 2, 3, 4)          // 10
sum([1, 2, 3, 4])        // 10
prod(2, 3, 4)            // 24
count([1, 2, 3])         // 3
max(1, 5, 3, 2)          // 5
min([10, 20, 5, 15])     // 5

// Matrix operations by dimension
const matrix = [[1, 2, 3], [4, 5, 6]]
sum(matrix, 0)           // [5, 7, 9] (column sums)
sum(matrix, 1)           // [6, 15] (row sums)
max(matrix, 0)           // [4, 5, 6] (column maxima)
```

### Cumulative Operations

Cumulative sum and correlation functions.

```javascript { .api }
/**
 * Calculate cumulative sum
 * @param array - Input values
 * @param dim - Dimension to calculate along (for matrices)
 * @returns Cumulative sum array
 */
function cumsum(array: MathCollection, dim?: number): MathCollection

/**
 * Calculate correlation coefficient between two arrays
 * @param x - First array
 * @param y - Second array
 * @returns Correlation coefficient [-1, 1]
 */
function corr(x: MathCollection, y: MathCollection): number
```

**Usage Examples:**

```javascript
import { cumsum, corr } from 'mathjs'

cumsum([1, 2, 3, 4])     // [1, 3, 6, 10]

// Correlation
const x = [1, 2, 3, 4, 5]
const y = [2, 4, 6, 8, 10]
corr(x, y)               // 1 (perfect positive correlation)

const z = [5, 4, 3, 2, 1]
corr(x, z)               // -1 (perfect negative correlation)
```

### Random Number Generation

Generate random values and make random selections.

```javascript { .api }
/**
 * Generate a random number
 * @returns Random number in [0, 1)
 */
function random(): number
/**
 * Generate a random number in a range
 * @param max - Maximum value (exclusive)
 * @returns Random number in [0, max)
 */
function random(max: number): number
/**
 * Generate a random number in a range
 * @param min - Minimum value (inclusive)
 * @param max - Maximum value (exclusive)
 * @returns Random number in [min, max)
 */
function random(min: number, max: number): number

/**
 * Generate a random matrix
 * @param size - Dimensions [m, n] or single number
 * @param min - Minimum value (default: 0)
 * @param max - Maximum value (default: 1)
 * @returns Random matrix
 */
function random(size: number[], min?: number, max?: number): Matrix

/**
 * Generate a random integer
 * @param max - Maximum value (exclusive)
 * @returns Random integer in [0, max)
 */
function randomInt(max: number): number
/**
 * Generate a random integer in a range
 * @param min - Minimum value (inclusive)
 * @param max - Maximum value (exclusive)
 * @returns Random integer in [min, max)
 */
function randomInt(min: number, max: number): number

/**
 * Generate a random integer matrix
 * @param size - Dimensions [m, n] or single number
 * @param min - Minimum value (default: 0)
 * @param max - Maximum value (exclusive, default: 1)
 * @returns Random integer matrix
 */
function randomInt(size: number[], min?: number, max?: number): Matrix

/**
 * Pick random element(s) from an array
 * @param array - Array to pick from
 * @returns Single random element
 */
function pickRandom(array: MathCollection): any
/**
 * Pick multiple random elements from an array
 * @param array - Array to pick from
 * @param number - Number of elements to pick
 * @param weights - Optional weights for weighted selection
 * @returns Array of randomly picked elements
 */
function pickRandom(
  array: MathCollection,
  number: number,
  weights?: MathCollection
): MathCollection
```

**Usage Examples:**

```javascript
import { random, randomInt, pickRandom, config } from 'mathjs'

// Random numbers
random()                 // e.g., 0.7531...
random(10)               // Random in [0, 10)
random(5, 10)            // Random in [5, 10)
random([2, 3])           // 2×3 random matrix

// Random integers
randomInt(10)            // Integer in [0, 10)
randomInt(5, 10)         // Integer in [5, 10)
randomInt([3, 3], 0, 10) // 3×3 random integer matrix

// Random selection
const arr = ['a', 'b', 'c', 'd', 'e']
pickRandom(arr)          // e.g., 'c'
pickRandom(arr, 3)       // e.g., ['a', 'd', 'b']

// Weighted random selection
pickRandom([1, 2, 3], 2, [0.1, 0.3, 0.6])  // 3 more likely

// Set random seed for reproducibility
config({ randomSeed: 'my-seed' })
```

### Statistical Testing and Analysis

Additional statistical functions for data analysis.

```javascript { .api }
/**
 * Calculate the Pearson correlation coefficient
 * This is an alias for corr()
 * @param x - First array
 * @param y - Second array
 * @returns Correlation coefficient
 */
function corr(x: MathCollection, y: MathCollection): number
```

**Usage Examples:**

```javascript
import { mean, std, variance } from 'mathjs'

// Calculate z-scores manually
function zScore(data, value) {
  const m = mean(data)
  const s = std(data)
  return (value - m) / s
}

const data = [2, 4, 4, 4, 5, 5, 7, 9]
zScore(data, 7)  // 1.0

// Coefficient of variation
function coefficientOfVariation(data) {
  return std(data) / mean(data)
}

coefficientOfVariation(data)  // 0.4
```

## Notes on Normalization

The `std` and `variance` functions support three normalization modes:

- `'unbiased'` (default): Divides by N-1 (Bessel's correction)
- `'uncorrected'`: Divides by N
- `'biased'`: Divides by N (same as uncorrected)

```javascript
import { std, variance } from 'mathjs'

const data = [1, 2, 3, 4, 5]

std(data, 'unbiased')      // Sample standard deviation (default)
std(data, 'uncorrected')   // Population standard deviation
variance(data, 'unbiased') // Sample variance
```

## Working with Multi-dimensional Data

Statistical functions can operate on specific dimensions of matrices:

```javascript
import { mean, std, sum } from 'mathjs'

const data = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]

// Overall statistics
mean(data)      // 5 (mean of all elements)

// By dimension
mean(data, 0)   // [4, 5, 6] (column means)
mean(data, 1)   // [2, 5, 8] (row means)

std(data, 0)    // Column-wise standard deviations
sum(data, 1)    // Row sums
```
