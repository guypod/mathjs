# Data Statistics Calculator

Build a module that computes descriptive statistics for a dataset represented as an array of numbers.

## Capabilities

### Compute descriptive statistics on numeric arrays

Implement a function `describeDataset(data)` that takes a non-empty array of numbers and returns an object with the following properties:

- `mean`: arithmetic mean of the values
- `median`: median value
- `std`: population standard deviation
- `min`: minimum value
- `max`: maximum value
- `sum`: sum of all values
- `variance`: population variance

All returned values must be plain JavaScript `number` values.

Additionally, implement `quantile(data, q)` that returns the `q`-th quantile (0 ≤ q ≤ 1) of the data array.

- `describeDataset([2, 4, 4, 4, 5, 5, 7, 9])` returns an object with `mean` = 5, `std` ≈ 2 [@test](./test/basic_stats.test.js)
- `describeDataset([1, 2, 3, 4, 5])` returns `{ min: 1, max: 5, sum: 15, median: 3 }` (among others) [@test](./test/simple_stats.test.js)
- `quantile([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 0.5)` returns `5.5` [@test](./test/quantile_median.test.js)
- `quantile([1, 2, 3, 4, 5], 0.25)` returns the 25th percentile value [@test](./test/quantile_q1.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Computes descriptive statistics for a numeric array.
 *
 * @param {number[]} data - Array of numbers (non-empty)
 * @returns {{ mean: number, median: number, std: number, min: number, max: number, sum: number, variance: number }}
 */
export function describeDataset(data) {}

/**
 * Returns the q-th quantile of the data.
 *
 * @param {number[]} data - Array of numbers
 * @param {number} q - Quantile fraction between 0 and 1
 * @returns {number} The q-th quantile value
 */
export function quantile(data, q) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library providing a full suite of statistical functions including mean, median, std, variance, min, max, sum, and quantileSeq.

[@satisfied-by](mathjs)
