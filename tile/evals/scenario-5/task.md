# Matrix Builder

Build a module that creates and manipulates matrices using a math library's matrix API.

## Capabilities

### Create and access matrices programmatically

Implement the following functions:

**`createIdentityMatrix(n)`** — Returns an `n×n` identity matrix as a 2D JavaScript array (array of arrays).

**`matrixFromRows(rows)`** — Takes a 2D array `rows` and returns a library Matrix object. The matrix must be stored in dense format.

**`getDiagonal(matrix)`** — Takes a 2D array or Matrix and returns a 1D array containing the diagonal elements.

**`reshapeMatrix(data, rows, cols)`** — Takes a flat 1D array `data` and reshapes it into a `rows × cols` 2D array. Throws if `data.length !== rows * cols`.

- `createIdentityMatrix(3)` returns `[[1, 0, 0], [0, 1, 0], [0, 0, 1]]` [@test](./test/identity.test.js)
- `getDiagonal([[1, 2, 3], [4, 5, 6], [7, 8, 9]])` returns `[1, 5, 9]` [@test](./test/diagonal.test.js)
- `reshapeMatrix([1, 2, 3, 4, 5, 6], 2, 3)` returns `[[1, 2, 3], [4, 5, 6]]` [@test](./test/reshape.test.js)
- `matrixFromRows([[1, 2], [3, 4]])` returns a Matrix instance whose `toArray()` equals `[[1, 2], [3, 4]]` [@test](./test/from_rows.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * @param {number} n - Size of the identity matrix
 * @returns {number[][]} n×n identity matrix as a 2D array
 */
export function createIdentityMatrix(n) {}

/**
 * @param {number[][]} rows - 2D array of values
 * @returns {Matrix} A dense Matrix instance
 */
export function matrixFromRows(rows) {}

/**
 * @param {number[][]|Matrix} matrix - A 2D array or Matrix
 * @returns {number[]} The diagonal elements
 */
export function getDiagonal(matrix) {}

/**
 * @param {number[]} data - Flat 1D array of values
 * @param {number} rows - Number of rows in the output
 * @param {number} cols - Number of columns in the output
 * @returns {number[][]} Reshaped 2D array
 */
export function reshapeMatrix(data, rows, cols) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library providing Matrix construction, utility functions for identity matrices, diagonal extraction, and reshaping.

[@satisfied-by](mathjs)
