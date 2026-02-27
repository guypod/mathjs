# Linear System Solver

Build a module that solves systems of linear equations and computes matrix properties.

## Capabilities

### Solve linear systems and compute matrix properties

Implement the following functions:

**`solveLinearSystem(A, b)`** — Solves the linear system `Ax = b` where `A` is a square 2D array (matrix) and `b` is a 1D array (vector). Returns the solution vector `x` as a plain 1D JavaScript array.

**`matrixDeterminant(A)`** — Returns the determinant of the square matrix `A` (given as a 2D array) as a plain JavaScript `number`.

**`matrixInverse(A)`** — Returns the inverse of the square matrix `A` as a plain 2D JavaScript array. Throws or returns an error-indicating value if the matrix is singular.

- `solveLinearSystem([[2, 1], [5, 3]], [4, 7])` returns approximately `[5, -6]` [@test](./test/solve_2x2.test.js)
- `matrixDeterminant([[-1, 2], [3, 1]])` returns `-7` [@test](./test/det.test.js)
- `matrixInverse([[1, 2], [3, 4]])` returns approximately `[[-2, 1], [1.5, -0.5]]` [@test](./test/inverse.test.js)
- `solveLinearSystem([[1, 0, 0], [0, 2, 0], [0, 0, 3]], [1, 4, 9])` returns `[1, 2, 3]` [@test](./test/solve_3x3.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Solves the linear system Ax = b.
 *
 * @param {number[][]} A - Coefficient matrix (n×n)
 * @param {number[]} b - Right-hand side vector (length n)
 * @returns {number[]} Solution vector x
 */
export function solveLinearSystem(A, b) {}

/**
 * Computes the determinant of a square matrix.
 *
 * @param {number[][]} A - Square matrix
 * @returns {number} The determinant
 */
export function matrixDeterminant(A) {}

/**
 * Computes the inverse of a square matrix.
 *
 * @param {number[][]} A - Square matrix
 * @returns {number[][]} The inverse matrix as a 2D array
 */
export function matrixInverse(A) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library providing linear algebra functions including LU decomposition, linear system solving, determinant computation, and matrix inversion.

[@satisfied-by](mathjs)
