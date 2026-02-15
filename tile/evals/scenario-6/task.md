# Matrix Manipulation Toolkit

Build a toolkit for creating and manipulating matrices with various operations.

## Requirements

Create three functions:

1. `createMatrix(rows, cols, fillValue)` - Creates a matrix of given dimensions filled with a specific value

2. `transposeAndExtract(matrix, row, col)` - Transposes a matrix and extracts the element at position [row, col]

3. `reshapeMatrix(matrix, newRows, newCols)` - Reshapes a matrix to new dimensions (must preserve total element count)

All functions should work with 2D matrices and handle indexing correctly (0-based for function parameters).

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing matrix creation, manipulation, and transformation operations.

## Test Cases

- `createMatrix(2, 3, 0)` returns a 2×3 matrix filled with zeros [@test](./test-1.js)
- `transposeAndExtract([[1,2],[3,4]], 1, 0)` returns 2 (after transpose: [[1,3],[2,4]]) [@test](./test-2.js)
- `reshapeMatrix([[1,2,3,4,5,6]], 2, 3)` returns [[1,2,3],[4,5,6]] [@test](./test-3.js)
