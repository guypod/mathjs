# Matrix Operations

Comprehensive linear algebra operations including matrix creation, decompositions, linear system solvers, signal processing, and matrix manipulation. Supports both dense and sparse matrices.

## Capabilities

### Matrix Creation

Create matrices with various initialization patterns.

```javascript { .api }
/**
 * Create a matrix
 * @param data - Matrix data (array or existing matrix)
 * @param format - 'dense' or 'sparse' (default: 'dense')
 * @param dataType - Data type for typed matrices
 * @returns Matrix instance
 */
function matrix(data?: MathCollection, format?: 'dense' | 'sparse', dataType?: string): Matrix

/**
 * Create a sparse matrix
 * @param data - Matrix data
 * @param dataType - Data type for typed matrices
 * @returns Sparse matrix instance
 */
function sparse(data?: MathCollection, dataType?: string): Matrix

/**
 * Create an identity matrix
 * @param n - Size (creates n×n matrix)
 * @param format - 'dense' or 'sparse'
 * @returns Identity matrix
 */
function identity(n: number, format?: 'dense' | 'sparse'): Matrix
function identity(m: number, n: number, format?: 'dense' | 'sparse'): Matrix

/**
 * Create a matrix filled with ones
 * @param size - Dimensions [m, n] or single number
 * @param format - 'dense' or 'sparse'
 * @returns Matrix of ones
 */
function ones(size: number | number[], format?: 'dense' | 'sparse'): Matrix

/**
 * Create a matrix filled with zeros
 * @param size - Dimensions [m, n] or single number
 * @param format - 'dense' or 'sparse'
 * @returns Matrix of zeros
 */
function zeros(size: number | number[], format?: 'dense' | 'sparse'): Matrix

/**
 * Create a diagonal matrix
 * @param values - Diagonal values
 * @returns Diagonal matrix
 */
function diag(values: MathCollection): Matrix
function diag(X: Matrix, k?: number, format?: 'dense' | 'sparse'): Matrix

/**
 * Create matrix from row vectors
 * @param rows - Row vectors
 * @returns Matrix constructed from rows
 */
function matrixFromRows(...rows: MathCollection[]): Matrix

/**
 * Create matrix from column vectors
 * @param cols - Column vectors
 * @returns Matrix constructed from columns
 */
function matrixFromColumns(...cols: MathCollection[]): Matrix

/**
 * Create matrix using a callback function
 * @param size - Matrix dimensions
 * @param fn - Function(indices) returning element value
 * @param format - 'dense' or 'sparse'
 * @param datatype - Data type
 * @returns Matrix with computed values
 */
function matrixFromFunction(
  size: number[],
  fn: (indices: number[]) => any,
  format?: 'dense' | 'sparse',
  datatype?: string
): Matrix
```

**Usage Examples:**

```javascript
import { matrix, sparse, identity, ones, zeros, diag } from 'mathjs'

// Create matrices
matrix([[1, 2], [3, 4]])        // Dense 2×2 matrix
sparse([[1, 0], [0, 2]])        // Sparse 2×2 matrix
identity(3)                      // 3×3 identity matrix
ones([2, 3])                     // 2×3 matrix of ones
zeros(3)                         // 3×3 matrix of zeros
diag([1, 2, 3])                  // Diagonal matrix

// From function
matrixFromFunction([3, 3], ([i, j]) => i === j ? 1 : 0) // Identity
```

### Basic Matrix Operations

Fundamental matrix operations and properties.

```javascript { .api }
/**
 * Calculate the determinant
 * @param x - Square matrix
 * @returns Determinant value
 */
function det(x: MathCollection): number | BigNumber

/**
 * Calculate the matrix inverse
 * @param x - Invertible square matrix
 * @returns Inverse matrix
 */
function inv(x: MathCollection): MathCollection

/**
 * Transpose a matrix
 * @param x - Matrix or array
 * @returns Transposed matrix
 */
function transpose(x: MathCollection): MathCollection

/**
 * Conjugate transpose (Hermitian transpose)
 * @param x - Matrix or array
 * @returns Conjugate transposed matrix
 */
function ctranspose(x: MathCollection): MathCollection

/**
 * Calculate the trace (sum of diagonal elements)
 * @param x - Square matrix
 * @returns Trace value
 */
function trace(x: MathCollection): number | BigNumber

/**
 * Concatenate matrices or arrays
 * @param args - Matrices/arrays to concatenate
 * @returns Concatenated result
 */
function concat(...args: MathCollection[]): MathCollection

/**
 * Calculate the dot product of two vectors
 * @param x - First vector
 * @param y - Second vector
 * @returns Dot product
 */
function dot(x: MathCollection, y: MathCollection): number | BigNumber | Complex

/**
 * Calculate the cross product (3D vectors only)
 * @param x - First 3D vector
 * @param y - Second 3D vector
 * @returns Cross product vector
 */
function cross(x: MathCollection, y: MathCollection): MathCollection

/**
 * Calculate the Kronecker product
 * @param x - First matrix
 * @param y - Second matrix
 * @returns Kronecker product
 */
function kron(x: MathCollection, y: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { det, inv, transpose, trace, dot, cross } from 'mathjs'

const A = [[1, 2], [3, 4]]

det(A)              // -2
inv(A)              // [[-2, 1], [1.5, -0.5]]
transpose(A)        // [[1, 3], [2, 4]]
trace(A)            // 5

dot([1, 2, 3], [4, 5, 6])     // 32
cross([1, 0, 0], [0, 1, 0])   // [0, 0, 1]
```

### Matrix Decompositions

Matrix factorization algorithms for numerical linear algebra.

```javascript { .api }
/**
 * Calculate eigenvalues and eigenvectors
 * @param x - Square matrix
 * @param opts - Options for algorithm
 * @returns Object with values and vectors properties
 */
function eigs(x: MathCollection, opts?: object): {
  values: MathCollection
  vectors: MathCollection
}

/**
 * LU decomposition with partial pivoting
 * @param A - Input matrix
 * @returns Object with L (lower), U (upper), and p (permutation)
 */
function lup(A: MathCollection): {
  L: MathCollection
  U: MathCollection
  p: number[]
}

/**
 * QR decomposition
 * @param A - Input matrix
 * @returns Object with Q (orthogonal) and R (upper triangular)
 */
function qr(A: MathCollection): {
  Q: MathCollection
  R: MathCollection
}

/**
 * Schur decomposition
 * @param A - Square matrix
 * @returns Object with U and T matrices
 */
function schur(A: MathCollection): {
  U: MathCollection
  T: MathCollection
}

/**
 * Sparse LU decomposition
 * @param A - Sparse matrix
 * @param order - Ordering strategy
 * @param threshold - Threshold for pivoting
 * @returns Object with L, U, and permutations
 */
function slu(A: Matrix, order: number, threshold: number): {
  L: Matrix
  U: Matrix
  p: number[]
  q: number[]
}
```

**Usage Examples:**

```javascript
import { eigs, lup, qr } from 'mathjs'

const A = [[2, 1], [1, 2]]

// Eigenvalues and eigenvectors
const { values, vectors } = eigs(A)

// LU decomposition
const { L, U, p } = lup(A)

// QR decomposition
const { Q, R } = qr(A)
```

### Linear System Solvers

Solve systems of linear equations.

```javascript { .api }
/**
 * Solve lower triangular system Lx = b
 * @param L - Lower triangular matrix
 * @param b - Right-hand side vector
 * @returns Solution vector x
 */
function lsolve(L: MathCollection, b: MathCollection): MathCollection

/**
 * Solve upper triangular system Ux = b
 * @param U - Upper triangular matrix
 * @param b - Right-hand side vector
 * @returns Solution vector x
 */
function usolve(U: MathCollection, b: MathCollection): MathCollection

/**
 * Solve linear system Ax = b using LU decomposition
 * @param A - Coefficient matrix
 * @param b - Right-hand side (vector or matrix)
 * @returns Solution x
 */
function lusolve(A: MathCollection, b: MathCollection): MathCollection

/**
 * Solve Lyapunov equation AX + XA' = Q
 * @param A - Coefficient matrix
 * @param Q - Right-hand side matrix
 * @returns Solution matrix X
 */
function lyap(A: MathCollection, Q: MathCollection): MathCollection

/**
 * Solve Sylvester equation AX + XB = C
 * @param A - First coefficient matrix
 * @param B - Second coefficient matrix
 * @param C - Right-hand side matrix
 * @returns Solution matrix X
 */
function sylvester(A: MathCollection, B: MathCollection, C: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { lusolve, lup, lsolve, usolve } from 'mathjs'

const A = [[2, 1], [1, 3]]
const b = [5, 7]

// Solve Ax = b
const x = lusolve(A, b)  // [1, 2]

// Manual LU solve
const { L, U, p } = lup(A)
const y = lsolve(L, b)
const solution = usolve(U, y)
```

### Advanced Matrix Operations

Matrix exponential, square root, and pseudoinverse.

```javascript { .api }
/**
 * Calculate matrix exponential e^A
 * @param x - Square matrix
 * @returns Matrix exponential
 */
function expm(x: MathCollection): MathCollection

/**
 * Calculate matrix square root
 * @param A - Positive semidefinite matrix
 * @returns Matrix square root
 */
function sqrtm(A: MathCollection): MathCollection

/**
 * Calculate Moore-Penrose pseudoinverse
 * @param x - Input matrix
 * @returns Pseudoinverse
 */
function pinv(x: MathCollection): MathCollection

/**
 * Rotate a vector
 * @param w - Vector to rotate
 * @param theta - Rotation angle
 * @param v - Rotation axis (for 3D)
 * @returns Rotated vector
 */
function rotate(w: MathCollection, theta: number | BigNumber, v?: MathCollection): MathCollection

/**
 * Create a rotation matrix
 * @param theta - Rotation angle
 * @param axis - Rotation axis (for 3D: [x,y,z] or string 'x','y','z')
 * @param format - 'dense' or 'sparse'
 * @returns Rotation matrix
 */
function rotationMatrix(
  theta: number | BigNumber | Unit,
  axis?: MathCollection | string,
  format?: string
): Matrix
```

**Usage Examples:**

```javascript
import { expm, sqrtm, pinv, rotationMatrix } from 'mathjs'

const A = [[1, 0], [0, 2]]

expm(A)              // Matrix exponential
sqrtm(A)             // [[1, 0], [0, sqrt(2)]]
pinv([[1, 2], [2, 4]]) // Pseudoinverse for singular matrix

// 2D rotation by 90 degrees
rotationMatrix(Math.PI / 2)  // [[0, -1], [1, 0]]
```

### Matrix Manipulation

Reshape, resize, and extract parts of matrices.

```javascript { .api }
/**
 * Extract a column from a matrix
 * @param value - Matrix or array
 * @param index - Column index (0-based)
 * @returns Column vector
 */
function column(value: MathCollection, index: number): MathCollection

/**
 * Extract a row from a matrix
 * @param value - Matrix or array
 * @param index - Row index (0-based)
 * @returns Row vector
 */
function row(value: MathCollection, index: number): MathCollection

/**
 * Flatten a matrix to a 1D array
 * @param x - Matrix or array
 * @returns Flattened array
 */
function flatten(x: MathCollection): MathCollection

/**
 * Reshape a matrix to new dimensions
 * @param x - Matrix or array
 * @param sizes - New dimensions
 * @returns Reshaped matrix
 */
function reshape(x: MathCollection, sizes: number[]): MathCollection

/**
 * Resize a matrix with padding or truncation
 * @param x - Matrix or array
 * @param size - New size
 * @param defaultValue - Value for new elements (default: 0)
 * @returns Resized matrix
 */
function resize(x: MathCollection, size: MathCollection, defaultValue?: any): MathCollection

/**
 * Get the size/dimensions of a matrix
 * @param x - Matrix, array, or any value
 * @returns Array of dimensions
 */
function size(x: any): number[]

/**
 * Remove singleton dimensions from a matrix
 * @param x - Matrix or array
 * @returns Squeezed matrix
 */
function squeeze(x: MathCollection): MathCollection

/**
 * Get or set a submatrix
 * @param x - Matrix or array
 * @param index - Index range
 * @param replacement - New values (for setting)
 * @param defaultValue - Default for expansion
 * @returns Submatrix or modified matrix
 */
function subset(
  x: MathCollection,
  index: Index,
  replacement?: any,
  defaultValue?: any
): MathCollection
```

**Usage Examples:**

```javascript
import { column, row, flatten, reshape, resize, size, squeeze } from 'mathjs'

const A = [[1, 2, 3], [4, 5, 6]]

column(A, 1)         // [2, 5]
row(A, 0)            // [1, 2, 3]
flatten(A)           // [1, 2, 3, 4, 5, 6]
reshape(A, [3, 2])   // [[1, 2], [3, 4], [5, 6]]
size(A)              // [2, 3]

// Resize with padding
resize([1, 2, 3], [5], 0)  // [1, 2, 3, 0, 0]

// Squeeze singleton dimensions
squeeze([[[1, 2, 3]]])  // [1, 2, 3]
```

### Signal Processing

FFT and frequency analysis functions.

```javascript { .api }
/**
 * Fast Fourier Transform
 * @param x - Input signal (array or matrix)
 * @returns Frequency domain representation
 */
function fft(x: MathCollection): MathCollection

/**
 * Inverse Fast Fourier Transform
 * @param x - Frequency domain data
 * @returns Time domain signal
 */
function ifft(x: MathCollection): MathCollection

/**
 * Numerical differentiation (finite differences)
 * @param x - Input signal
 * @param dim - Dimension along which to differentiate
 * @returns Differences along specified dimension
 */
function diff(x: MathCollection, dim?: number): MathCollection

/**
 * Frequency response of a digital filter
 * @param b - Numerator coefficients
 * @param a - Denominator coefficients
 * @param w - Frequencies at which to evaluate (optional)
 * @returns Frequency response
 */
function freqz(
  b: MathCollection,
  a: MathCollection,
  w?: number | MathCollection
): { w: MathCollection, h: MathCollection }

/**
 * Convert zero-pole-gain to transfer function
 * @param z - Zeros
 * @param p - Poles
 * @param k - Gain
 * @returns Transfer function coefficients
 */
function zpk2tf(
  z: MathCollection,
  p: MathCollection,
  k: number
): { b: MathCollection, a: MathCollection }
```

**Usage Examples:**

```javascript
import { fft, ifft, diff } from 'mathjs'

// FFT of a signal
const signal = [1, 2, 3, 4]
const freq = fft(signal)

// Inverse FFT
const recovered = ifft(freq)

// Numerical differentiation
diff([1, 4, 9, 16])  // [3, 5, 7] (differences)
```

### Array Iteration and Filtering

Higher-order functions for working with matrices and arrays.

```javascript { .api }
/**
 * Filter elements that satisfy a test
 * @param x - Matrix or array
 * @param test - Test function or RegExp
 * @returns Filtered elements
 */
function filter(
  x: MathCollection,
  test: ((value: any, index: any, matrix: MathCollection) => boolean) | RegExp
): MathCollection

/**
 * Iterate over each element
 * @param x - Matrix or array
 * @param callback - Function to call for each element
 */
function forEach(
  x: MathCollection,
  callback: (value: any, index: any, matrix: MathCollection) => void
): void

/**
 * Map a function over each element
 * @param x - Matrix or array
 * @param callback - Function to apply to each element
 * @returns New matrix with mapped values
 */
function map(
  x: MathCollection,
  callback: (value: any, index: any, matrix: MathCollection) => any
): MathCollection

/**
 * Sort elements
 * @param x - Matrix or array
 * @param compare - Comparison function
 * @returns Sorted matrix
 */
function sort(
  x: MathCollection,
  compare?: (a: any, b: any) => number
): MathCollection

/**
 * Partition select (find kth smallest element)
 * @param x - Array
 * @param k - Index of element to find
 * @param compare - Comparison function
 * @returns kth smallest element
 */
function partitionSelect(
  x: MathCollection,
  k: number,
  compare?: (a: any, b: any) => number
): any
```

**Usage Examples:**

```javascript
import { filter, map, forEach, sort } from 'mathjs'

const arr = [1, 2, 3, 4, 5]

// Filter even numbers
filter(arr, x => x % 2 === 0)  // [2, 4]

// Map to squares
map(arr, x => x * x)  // [1, 4, 9, 16, 25]

// Sort in descending order
sort(arr, (a, b) => b - a)  // [5, 4, 3, 2, 1]
```
