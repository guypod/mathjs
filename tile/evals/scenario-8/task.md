# Matrix Decomposition Solver

Build a linear algebra solver using matrix decompositions to solve systems of equations.

## Requirements

Create a function `solveLinearSystem(A, b)` that:

- Takes a coefficient matrix A (n×n square matrix)
- Takes a right-hand side vector b (length n)
- Uses matrix decomposition (LU decomposition with pivoting) to solve the system Ax = b
- Returns the solution vector x

The solver should:
- Decompose the matrix into factors
- Use the decomposition to efficiently solve the system
- Handle systems of at least 3×3 size

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing matrix decompositions and linear system solvers.

## Test Cases

- Solve [[2,1],[1,2]] * x = [3,3], returns [1,1] [@test](./test-1.js)
- Solve [[3,2,1],[2,3,2],[1,2,3]] * x = [6,6,6], returns [1,1,1] [@test](./test-2.js)
- Solve [[1,2],[2,4]] * x = [3,6] handles singular/near-singular matrices appropriately [@test](./test-3.js)
