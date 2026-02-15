# Matrix and Linear Algebra

Math.js provides comprehensive matrix and linear algebra operations supporting both dense and sparse matrices. The library includes matrix creation, manipulation, decomposition algorithms, linear system solvers, and advanced operations for scientific computing.

## Capabilities

### Matrix Creation

Create matrices from various sources including arrays, functions, and column/row vectors.

```typescript { .api }
/**
 * Create identity matrix
 * @param size - Matrix size (number for square, array for rectangular)
 * @param format - Storage format ('dense' or 'sparse')
 * @returns Identity matrix
 */
function identity(size: number | number[], format?: 'dense' | 'sparse'): Matrix;

/**
 * Create matrix of ones
 * @param size - Matrix dimensions (number or array)
 * @param format - Storage format ('dense' or 'sparse')
 * @returns Matrix filled with ones
 */
function ones(size: number | number[], format?: 'dense' | 'sparse'): Matrix;

/**
 * Create matrix of zeros
 * @param size - Matrix dimensions (number or array)
 * @param format - Storage format ('dense' or 'sparse')
 * @returns Matrix filled with zeros
 */
function zeros(size: number | number[], format?: 'dense' | 'sparse'): Matrix;

/**
 * Create diagonal matrix from vector
 * @param vector - Vector or matrix to extract diagonal from
 * @param k - Diagonal offset (0 = main diagonal, positive = above, negative = below)
 * @param format - Storage format ('dense' or 'sparse')
 * @returns Diagonal matrix
 */
function diag(vector: MathArray | Matrix, k?: number, format?: 'dense' | 'sparse'): Matrix;

/**
 * Create range matrix
 * @param start - Start value (inclusive)
 * @param end - End value (exclusive for numbers, inclusive for units and strings)
 * @param step - Step size (optional, defaults to 1)
 * @returns Range matrix
 */
function range(start: number | BigNumber | Unit | string, end: number | BigNumber | Unit | string, step?: number | BigNumber | Unit | string): Matrix;

/**
 * Create matrix from generating function
 * @param size - Matrix dimensions array
 * @param fn - Generator function (index: number[]) => value
 * @param format - Storage format ('dense' or 'sparse')
 * @returns Generated matrix
 */
function matrixFromFunction(size: number[], fn: (index: number[]) => any, format?: 'dense' | 'sparse'): Matrix;

/**
 * Create matrix from column vectors
 * @param cols - Column vectors as arrays or matrices
 * @returns Matrix with specified columns
 */
function matrixFromColumns(...cols: Array<MathArray | Matrix>): Matrix;

/**
 * Create matrix from row vectors
 * @param rows - Row vectors as arrays or matrices
 * @returns Matrix with specified rows
 */
function matrixFromRows(...rows: Array<MathArray | Matrix>): Matrix;
```

### Matrix Access and Manipulation

Extract and manipulate matrix rows, columns, and subsets.

```typescript { .api }
/**
 * Extract row from matrix
 * @param matrix - Input matrix or array
 * @param index - Zero-based row index
 * @returns Row as matrix
 */
function row(matrix: MathArray | Matrix, index: number): Matrix;

/**
 * Extract column from matrix
 * @param matrix - Input matrix or array
 * @param index - Zero-based column index
 * @returns Column as matrix
 */
function column(matrix: MathArray | Matrix, index: number): Matrix;

/**
 * Get or set matrix subset
 * @param matrix - Input matrix or array
 * @param index - Index specifying subset
 * @param replacement - Replacement value (optional, for setting)
 * @returns Subset when getting, modified matrix when setting
 */
function subset(matrix: MathArray | Matrix, index: Index, replacement?: any): MathArray | Matrix;

/**
 * Concatenate matrices
 * @param matrices - Matrices to concatenate
 * @param dim - Dimension along which to concatenate (0 = rows, 1 = columns)
 * @returns Concatenated matrix
 */
function concat(...matrices: Array<MathArray | Matrix>): Matrix;
```

### Matrix Shape Operations

Reshape, resize, and transform matrix dimensions.

```typescript { .api }
/**
 * Get matrix dimensions
 * @param matrix - Input matrix, array, or value
 * @returns Array of dimensions
 */
function size(matrix: MathType): number[];

/**
 * Reshape matrix to new dimensions
 * @param matrix - Input matrix or array
 * @param size - New dimensions array
 * @returns Reshaped matrix
 */
function reshape(matrix: MathArray | Matrix, size: number[]): Matrix;

/**
 * Resize matrix to new dimensions
 * @param matrix - Input matrix or array
 * @param size - New dimensions array
 * @param defaultValue - Value to fill new elements (defaults to 0)
 * @returns Resized matrix
 */
function resize(matrix: MathArray | Matrix, size: number[], defaultValue?: any): Matrix;

/**
 * Remove singleton dimensions
 * @param matrix - Input matrix or array
 * @returns Matrix with singleton dimensions removed
 */
function squeeze(matrix: MathArray | Matrix): Matrix;

/**
 * Flatten matrix to 1D array
 * @param matrix - Input matrix or array
 * @returns Flattened array
 */
function flatten(matrix: MathArray | Matrix): MathArray;

/**
 * Transpose matrix
 * @param matrix - Input matrix or array
 * @returns Transposed matrix
 */
function transpose(matrix: MathArray | Matrix): Matrix;

/**
 * Conjugate transpose (Hermitian transpose)
 * @param matrix - Input matrix or array
 * @returns Conjugate transposed matrix
 */
function ctranspose(matrix: MathArray | Matrix): Matrix;
```

### Matrix Iteration and Transformation

Apply functions to matrix elements.

```typescript { .api }
/**
 * Iterate over matrix elements
 * @param matrix - Input matrix or array
 * @param callback - Function called for each element (value, index, matrix) => void
 */
function forEach(matrix: MathArray | Matrix, callback: (value: any, index: number[], matrix: MathArray | Matrix) => void): void;

/**
 * Map function over matrix elements
 * @param matrix - Input matrix or array
 * @param callback - Function called for each element (value, index, matrix) => newValue
 * @returns New matrix with mapped values
 */
function map(matrix: MathArray | Matrix, callback: (value: any, index: number[], matrix: MathArray | Matrix) => any): Matrix;

/**
 * Map function over matrix slices
 * @param matrix - Input matrix or array
 * @param dim - Dimension to slice along
 * @param callback - Function called for each slice (slice) => newSlice
 * @returns New matrix with transformed slices
 */
function mapSlices(matrix: MathArray | Matrix, dim: number, callback: (slice: Matrix) => Matrix): Matrix;

/**
 * Filter matrix elements
 * @param matrix - Input matrix or array
 * @param callback - Test function (value, index, matrix) => boolean
 * @returns Array of elements that pass the test
 */
function filter(matrix: MathArray | Matrix, callback: (value: any, index: number[], matrix: MathArray | Matrix) => boolean): MathArray;

/**
 * Sort matrix elements
 * @param matrix - Input matrix or array
 * @param compare - Comparison function (a, b) => number (optional)
 * @returns Sorted matrix
 */
function sort(matrix: MathArray | Matrix, compare?: (a: any, b: any) => number): Matrix;
```

### Matrix Products

Compute various types of matrix products.

```typescript { .api }
/**
 * Dot product of vectors or matrices
 * @param a - First vector or matrix
 * @param b - Second vector or matrix
 * @returns Dot product
 */
function dot(a: MathArray | Matrix, b: MathArray | Matrix): number | Complex;

/**
 * Cross product of 3D vectors
 * @param a - First 3D vector
 * @param b - Second 3D vector
 * @returns Cross product vector
 */
function cross(a: MathArray | Matrix, b: MathArray | Matrix): Matrix;

/**
 * Kronecker product of matrices
 * @param a - First matrix
 * @param b - Second matrix
 * @returns Kronecker product
 */
function kron(a: MathArray | Matrix, b: MathArray | Matrix): Matrix;
```

### Matrix Properties

Compute matrix properties and characteristics.

```typescript { .api }
/**
 * Count non-zero elements
 * @param matrix - Input matrix or array
 * @returns Number of non-zero elements
 */
function count(matrix: MathArray | Matrix): number;

/**
 * Get matrix element data type
 * @param matrix - Input matrix
 * @returns Data type string (e.g., 'number', 'Complex', 'BigNumber')
 */
function getMatrixDataType(matrix: Matrix): string;

/**
 * Matrix trace (sum of diagonal elements)
 * @param matrix - Square matrix
 * @returns Trace value
 */
function trace(matrix: MathArray | Matrix): number | BigNumber | Complex;

/**
 * Matrix determinant
 * @param matrix - Square matrix
 * @returns Determinant value
 */
function det(matrix: MathArray | Matrix): number | BigNumber | Complex;

/**
 * Matrix rank (number of linearly independent rows/columns)
 * @param matrix - Input matrix
 * @returns Rank value
 */
function rank(matrix: MathArray | Matrix): number;
```

### Matrix Inverse and Pseudo-inverse

Compute matrix inverses for solving linear systems.

```typescript { .api }
/**
 * Matrix inverse
 * @param matrix - Square invertible matrix
 * @returns Inverse matrix
 */
function inv(matrix: MathArray | Matrix): Matrix;

/**
 * Moore-Penrose pseudo-inverse
 * @param matrix - Input matrix (can be non-square)
 * @returns Pseudo-inverse matrix
 */
function pinv(matrix: MathArray | Matrix): Matrix;
```

### Matrix Decompositions

Decompose matrices for numerical analysis and solving systems.

```typescript { .api }
/**
 * LU decomposition with partial pivoting
 * @param matrix - Input matrix
 * @returns Object with L (lower), U (upper), P (permutation) matrices and p (permutation vector)
 */
function lup(matrix: MathArray | Matrix): {
  L: Matrix;
  U: Matrix;
  P: Matrix;
  p: number[];
};

/**
 * Sparse LU decomposition
 * @param matrix - Sparse matrix
 * @param order - Column ordering (0 = natural, 1 = AMD)
 * @param threshold - Pivoting threshold (0 to 1)
 * @returns Object with L, U matrices and pinv (row permutation)
 */
function slu(matrix: Matrix, order?: number, threshold?: number): {
  L: Matrix;
  U: Matrix;
  pinv: number[];
};

/**
 * QR decomposition
 * @param matrix - Input matrix
 * @returns Object with Q (orthogonal) and R (upper triangular) matrices
 */
function qr(matrix: MathArray | Matrix): {
  Q: Matrix;
  R: Matrix;
};

/**
 * Schur decomposition
 * @param matrix - Square matrix
 * @param epsilon - Convergence tolerance (optional)
 * @returns Object with U (unitary) and T (upper triangular) matrices
 */
function schur(matrix: MathArray | Matrix, epsilon?: number): {
  U: Matrix;
  T: Matrix;
};

/**
 * Eigenvalues and eigenvectors
 * @param matrix - Square matrix
 * @param options - Options object with precision, epsilon, and maxIterations
 * @returns Object with values (eigenvalues) and optionally vectors (eigenvectors) arrays
 */
function eigs(matrix: MathArray | Matrix, options?: {
  precision?: number;
  epsilon?: number;
  maxIterations?: number;
}): {
  values: MathArray;
  vectors?: MathArray;
};
```

### Linear System Solvers

Solve linear systems of equations.

```typescript { .api }
/**
 * Solve Lx = b where L is lower triangular
 * @param L - Lower triangular matrix
 * @param b - Right-hand side vector or matrix
 * @returns Solution x
 */
function lsolve(L: MathArray | Matrix, b: MathArray | Matrix): Matrix;

/**
 * Find all solutions to Lx = b where L is lower triangular
 * @param L - Lower triangular matrix
 * @param b - Right-hand side vector or matrix
 * @returns Array of solution matrices
 */
function lsolveAll(L: MathArray | Matrix, b: MathArray | Matrix): Matrix[];

/**
 * Solve Ux = b where U is upper triangular
 * @param U - Upper triangular matrix
 * @param b - Right-hand side vector or matrix
 * @returns Solution x
 */
function usolve(U: MathArray | Matrix, b: MathArray | Matrix): Matrix;

/**
 * Find all solutions to Ux = b where U is upper triangular
 * @param U - Upper triangular matrix
 * @param b - Right-hand side vector or matrix
 * @returns Array of solution matrices
 */
function usolveAll(U: MathArray | Matrix, b: MathArray | Matrix): Matrix[];

/**
 * Solve linear system using LU decomposition
 * @param LU - LU decomposition result from lup()
 * @param b - Right-hand side vector or matrix
 * @param order - Column permutation vector (optional)
 * @returns Solution x
 */
function lusolve(LU: Matrix | { L: Matrix; U: Matrix; p: number[] }, b: MathArray | Matrix, order?: number[]): Matrix;
```

### Matrix Equations

Solve special matrix equations.

```typescript { .api }
/**
 * Solve Lyapunov equation AX + XA' = Q
 * @param A - Coefficient matrix
 * @param Q - Right-hand side matrix
 * @returns Solution matrix X
 */
function lyap(A: MathArray | Matrix, Q: MathArray | Matrix): Matrix;

/**
 * Solve Sylvester equation AX + XB = C
 * @param A - First coefficient matrix
 * @param B - Second coefficient matrix
 * @param C - Right-hand side matrix
 * @returns Solution matrix X
 */
function sylvester(A: MathArray | Matrix, B: MathArray | Matrix, C: MathArray | Matrix): Matrix;
```

### Matrix Functions

Apply mathematical functions to matrices.

```typescript { .api }
/**
 * Matrix exponential e^A
 * @param matrix - Square matrix
 * @returns Matrix exponential
 */
function expm(matrix: MathArray | Matrix): Matrix;

/**
 * Matrix square root
 * @param matrix - Square matrix
 * @returns Principal square root of matrix
 */
function sqrtm(matrix: MathArray | Matrix): Matrix;
```

### Signal Processing

Apply signal processing operations to arrays and matrices.

```typescript { .api }
/**
 * Fast Fourier Transform
 * @param array - Input array or matrix
 * @returns Complex array with Fourier coefficients
 */
function fft(array: MathArray | Matrix): Matrix;

/**
 * Inverse Fast Fourier Transform
 * @param array - Complex array with Fourier coefficients
 * @returns Reconstructed array
 */
function ifft(array: MathArray | Matrix): Matrix;
```

### Rotation Matrices

Create and apply rotation transformations.

```typescript { .api }
/**
 * Rotate matrix by angle around axis
 * @param matrix - Vector or matrix to rotate (2D or 3D)
 * @param angle - Rotation angle in radians
 * @param axis - Rotation axis (optional, for 3D: [x, y, z] or 'x', 'y', 'z')
 * @returns Rotated matrix
 */
function rotate(matrix: MathArray | Matrix, angle: number | BigNumber, axis?: MathArray | Matrix | string): Matrix;

/**
 * Create rotation matrix
 * @param dim - Dimension (2 or 3)
 * @param rad - Rotation angle in radians
 * @param axis - Rotation axis (for 3D: [x, y, z] or 'x', 'y', 'z')
 * @returns Rotation matrix
 */
function rotationMatrix(dim: number, rad: number | BigNumber, axis?: MathArray | Matrix | string): Matrix;
```

### Polynomial Operations

Work with polynomials represented as coefficient arrays.

```typescript { .api }
/**
 * Find roots of polynomial
 * @param coefficients - Polynomial coefficients (highest degree first)
 * @returns Array of complex roots
 */
function polynomialRoot(coefficients: MathArray | Matrix): MathArray;
```

### Statistical Operations on Matrices

Select and analyze matrix elements.

```typescript { .api }
/**
 * Select kth smallest element (partition selection algorithm)
 * @param matrix - Input matrix or array
 * @param k - Zero-based index of element to select
 * @param compare - Comparison function (optional)
 * @returns kth smallest element
 */
function partitionSelect(matrix: MathArray | Matrix, k: number, compare?: (a: any, b: any) => number): any;

/**
 * Differences between adjacent elements
 * @param matrix - Input matrix or array
 * @param dim - Dimension along which to compute differences (optional, defaults to first)
 * @returns Matrix of differences
 */
function diff(matrix: MathArray | Matrix, dim?: number): Matrix;
```

## Types

### Matrix Index

```typescript { .api }
/**
 * Create multi-dimensional index
 * @param dimensions - Index ranges for each dimension
 * @returns Index object for subset operations
 */
function index(...dimensions: Array<number | Range | number[]>): Index;

class Index {
  /** Get dimension range at position */
  dimension(dim: number): IRange;

  /** Get size of each dimension */
  size(): number[];

  /** Check if scalar (single element) index */
  isScalar(): boolean;
}
```

### Range

```typescript { .api }
interface IRange {
  start: number;
  end: number;
  step: number;
}
```

## Usage Examples

### Creating Matrices

```typescript
import { matrix, sparse, identity, zeros, ones, diag, range } from 'mathjs';

// Dense matrix from array
const A = matrix([[1, 2, 3], [4, 5, 6]]);

// Sparse matrix (efficient for large matrices with many zeros)
const B = sparse([[1, 0, 0], [0, 0, 2], [0, 3, 0]]);

// Special matrices
const I = identity(3);              // 3x3 identity
const Z = zeros([2, 4]);            // 2x4 zeros
const O = ones([3, 3]);             // 3x3 ones
const D = diag([1, 2, 3]);          // Diagonal matrix

// Range matrix
const R = range(0, 10, 2);          // [0, 2, 4, 6, 8]
```

### Matrix from Functions and Vectors

```typescript
import { matrixFromFunction, matrixFromColumns, matrixFromRows } from 'mathjs';

// Generate matrix using function
const M = matrixFromFunction([3, 3], (index) => {
  const [i, j] = index;
  return i + j;
});
// Result: [[0, 1, 2], [1, 2, 3], [2, 3, 4]]

// Create from columns
const A = matrixFromColumns([1, 2, 3], [4, 5, 6]);
// Result: [[1, 4], [2, 5], [3, 6]]

// Create from rows
const B = matrixFromRows([1, 2], [3, 4], [5, 6]);
// Result: [[1, 2], [3, 4], [5, 6]]
```

### Matrix Access and Subset Operations

```typescript
import { matrix, row, column, subset, index } from 'mathjs';

const A = matrix([
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]);

// Extract row and column
const r1 = row(A, 1);              // [4, 5, 6]
const c2 = column(A, 2);           // [3, 6, 9]

// Get subset
const idx = index([0, 1], [1, 2]);
const sub = subset(A, idx);        // [[2, 3], [5, 6]]

// Set subset
const B = subset(A, index(1, 1), 100);
// Result: [[1, 2, 3], [4, 100, 6], [7, 8, 9]]
```

### Matrix Shape Transformations

```typescript
import { reshape, resize, transpose, flatten, squeeze } from 'mathjs';

const A = matrix([[1, 2, 3], [4, 5, 6]]);

// Reshape (preserves elements)
const R = reshape(A, [3, 2]);      // [[1, 2], [3, 4], [5, 6]]

// Resize (can add/remove elements)
const S = resize(A, [3, 4], 0);    // [[1, 2, 3, 0], [4, 5, 6, 0], [0, 0, 0, 0]]

// Transpose
const T = transpose(A);            // [[1, 4], [2, 5], [3, 6]]

// Flatten to 1D array
const F = flatten(A);              // [1, 2, 3, 4, 5, 6]

// Remove singleton dimensions
const single = matrix([[[1]], [[2]]]);
const squeezed = squeeze(single);  // [1, 2]
```

### Matrix Iteration and Mapping

```typescript
import { matrix, forEach, map, filter } from 'mathjs';

const A = matrix([[1, 2], [3, 4]]);

// Iterate over elements
forEach(A, (value, index) => {
  console.log(`A[${index}] = ${value}`);
});

// Map function over elements
const B = map(A, (value) => value * 2);
// Result: [[2, 4], [6, 8]]

// Filter elements
const positive = filter(A, (value) => value > 2);
// Result: [3, 4]
```

### Matrix Products

```typescript
import { dot, cross, kron, multiply } from 'mathjs';

// Dot product of vectors
const a = [1, 2, 3];
const b = [4, 5, 6];
const dotProd = dot(a, b);         // 32

// Cross product (3D vectors only)
const u = [1, 0, 0];
const v = [0, 1, 0];
const crossProd = cross(u, v);     // [0, 0, 1]

// Kronecker product
const A = [[1, 2], [3, 4]];
const B = [[0, 5], [6, 7]];
const K = kron(A, B);
// Result: [[0, 5, 0, 10], [6, 7, 12, 14], [0, 15, 0, 20], [18, 21, 24, 28]]

// Standard matrix multiplication
const C = multiply(A, B);          // [[12, 19], [24, 43]]
```

### Matrix Decompositions

```typescript
import { lup, qr, eigs } from 'mathjs';

const A = [[4, 3], [6, 3]];

// LU decomposition with partial pivoting
const { L, U, P, p } = lup(A);
console.log('L:', L);              // Lower triangular
console.log('U:', U);              // Upper triangular
console.log('P:', P);              // Permutation matrix
console.log('p:', p);              // Permutation vector

// QR decomposition
const { Q, R } = qr(A);
console.log('Q:', Q);              // Orthogonal matrix
console.log('R:', R);              // Upper triangular

// Eigenvalues and eigenvectors
const result = eigs(A);
console.log('Eigenvalues:', result.values);
console.log('Eigenvectors:', result.vectors);
```

### Solving Linear Systems

```typescript
import { lup, lusolve, inv, multiply } from 'mathjs';

// Solve Ax = b using LU decomposition
const A = [[2, 1], [5, 7]];
const b = [11, 13];

const decomp = lup(A);
const x = lusolve(decomp, b);      // [7.111..., -3.222...]

// Verify solution
const check = multiply(A, x);      // Should equal b

// Alternative: using matrix inverse
const Ainv = inv(A);
const x2 = multiply(Ainv, b);      // Same result
```

### Triangular System Solvers

```typescript
import { lsolve, usolve } from 'mathjs';

// Lower triangular system
const L = [[1, 0, 0], [2, 1, 0], [3, 4, 1]];
const b = [1, 2, 3];
const x = lsolve(L, b);

// Upper triangular system
const U = [[1, 2, 3], [0, 1, 4], [0, 0, 1]];
const y = usolve(U, b);
```

### Matrix Equations

```typescript
import { lyap, sylvester } from 'mathjs';

// Solve Lyapunov equation: AX + XA' = Q
const A = [[1, 2], [3, 4]];
const Q = [[3, 1], [1, 3]];
const X = lyap(A, Q);

// Solve Sylvester equation: AX + XB = C
const B = [[1, 1], [0, 1]];
const C = [[1, 2], [3, 4]];
const Y = sylvester(A, B, C);
```

### Matrix Functions

```typescript
import { expm, sqrtm, det, trace, inv } from 'mathjs';

const A = [[1, 1], [0, 1]];

// Matrix exponential
const expA = expm(A);              // [[e, e], [0, e]]

// Matrix square root
const sqrtA = sqrtm(A);

// Matrix properties
const detA = det(A);               // 1
const trA = trace(A);              // 2
const invA = inv(A);               // [[1, -1], [0, 1]]
```

### Sparse Matrix Operations

```typescript
import { sparse, slu, multiply, zeros } from 'mathjs';

// Create large sparse matrix (efficient storage)
const A = sparse([
  [5, 0, 0, 0],
  [0, 8, 0, 0],
  [0, 0, 3, 0],
  [2, 0, 0, 1]
]);

console.log(A.nonZeros());         // 5

// Sparse LU decomposition
const { L, U, pinv } = slu(A, 1, 0.001);

// Operations preserve sparsity when possible
const B = sparse(zeros([1000, 1000]));
B.set([0, 0], 1);
B.set([999, 999], 1);
```

### Signal Processing

```typescript
import { fft, ifft } from 'mathjs';

// FFT of signal
const signal = [1, 2, 3, 4];
const freq = fft(signal);          // Complex frequency domain

// Inverse FFT
const reconstructed = ifft(freq);  // Back to [1, 2, 3, 4]

// FFT is useful for frequency analysis
const timeSeries = [1, 0, 1, 0, 1, 0, 1, 0];
const spectrum = fft(timeSeries);
```

### Rotation Matrices

```typescript
import { rotate, rotationMatrix, multiply, pi } from 'mathjs';

// Rotate 2D vector
const v = [1, 0];
const rotated = rotate(v, pi / 2);  // [0, 1] (90° rotation)

// Create 2D rotation matrix
const R2 = rotationMatrix(2, pi / 4);  // 45° rotation

// Create 3D rotation matrix around z-axis
const R3 = rotationMatrix(3, pi / 3, [0, 0, 1]);

// Rotate 3D vector around x-axis
const v3 = [0, 1, 0];
const rotated3 = rotate(v3, pi / 2, 'x');  // [0, 0, 1]
```

### Polynomial Roots

```typescript
import { polynomialRoot } from 'mathjs';

// Find roots of x^2 - 5x + 6 = 0
const coeffs = [1, -5, 6];         // Highest degree first
const roots = polynomialRoot(coeffs);  // [3, 2]

// Complex roots: x^2 + 1 = 0
const complexRoots = polynomialRoot([1, 0, 1]);  // [i, -i]

// Cubic: x^3 - 6x^2 + 11x - 6 = 0
const cubic = polynomialRoot([1, -6, 11, -6]);  // [1, 2, 3]
```

### Advanced Matrix Operations

```typescript
import {
  schur, eigs, rank, cond, norm,
  partitionSelect, diff
} from 'mathjs';

const A = [[4, -2], [1, 1]];

// Schur decomposition
const { U, T } = schur(A);

// Eigenanalysis
const eigen = eigs(A);
console.log('Eigenvalues:', eigen.values);

// Find median using partition select
const data = [5, 2, 8, 1, 9];
const median = partitionSelect(data, 2);  // 5 (3rd smallest)

// Differences between elements
const series = [1, 4, 9, 16, 25];
const diffs = diff(series);       // [3, 5, 7, 9]
```

### Complex and BigNumber Matrices

```typescript
import { matrix, complex, bignumber, multiply, det } from 'mathjs';

// Complex matrix
const C = matrix([
  [complex(1, 2), complex(3, 4)],
  [complex(5, 6), complex(7, 8)]
]);

const detC = det(C);               // Complex determinant

// BigNumber matrix (arbitrary precision)
const B = matrix([
  [bignumber('1.1'), bignumber('2.2')],
  [bignumber('3.3'), bignumber('4.4')]
]);

const detB = det(B);               // BigNumber result
```
