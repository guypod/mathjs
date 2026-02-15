# Dataset Statistics Calculator

Build a statistics calculator that computes comprehensive descriptive statistics for numerical datasets.

## Requirements

Create a function `calculateStats(data)` that takes a dataset (array of numbers or multi-dimensional array) and returns an object containing:

- `mean`: The arithmetic average of all values
- `median`: The middle value when sorted
- `mode`: The most frequently occurring value
- `stdDev`: The standard deviation
- `variance`: The statistical variance

For multi-dimensional arrays, compute statistics across all elements (flattened).

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing statistical functions and numerical operations.

## Test Cases

- `[1, 2, 3, 4, 5]` returns mean: 3, median: 3, stdDev: ≈1.414 [@test](./test-1.js)
- `[10, 20, 20, 30, 30, 30]` returns mode: 30 [@test](./test-2.js)
- `[[1, 2], [3, 4]]` treats as flattened `[1, 2, 3, 4]`, returns mean: 2.5 [@test](./test-3.js)
