# mathjs Matrix and Linear Algebra

mathjs v15.1.0 — Matrix creation, manipulation, and linear algebra functions.

---

## Core Types

```typescript { .api }
type MathCollection<T = MathNumericType> = MathArray<T> | Matrix<T>
type MathArray<T>                        = T[] | Array<MathArray<T>>
type MatrixStorageFormat                 = 'dense' | 'sparse'
type MatrixFromFunctionCallback<T>       = (index: number[]) => T
```

---

## Decomposition Result Interfaces

```typescript { .api }
interface LUDecomposition {
  L: MathCollection   // lower-triangular factor
  U: MathCollection   // upper-triangular factor
  p: number[]         // row permutation vector (A[p,:] = L * U)
}

interface SLUDecomposition extends LUDecomposition {
  q: number[]         // column permutation vector (P * A * Q = L * U)
}

interface QRDecomposition {
  Q: MathCollection   // orthogonal matrix
  R: MathCollection   // upper-triangular matrix
}

interface SchurDecomposition {
  U: MathCollection   // orthogonal matrix
  T: MathCollection   // upper quasi-triangular matrix (A = U T U')
}
```

---

## Matrix Creation

### zeros

Create a matrix filled with zeros.

```typescript { .api }
function zeros(size?: number | number[] | BigNumber | BigNumber[], format?: string): MathCollection
function zeros(m: number | BigNumber, n: number | BigNumber, format?: string): MathCollection
function zeros(m: number | BigNumber, n: number | BigNumber, p: number | BigNumber, format?: string): MathCollection
```

**Note:** When a single Array argument is passed (e.g. `zeros([2, 3])`), the return value is a plain JavaScript Array, not a DenseMatrix. Passing numeric arguments (e.g. `zeros(2, 3)`) or a `'dense'`/`'sparse'` format string returns a DenseMatrix or SparseMatrix respectively.

### ones

Create a matrix filled with ones.

```typescript { .api }
function ones(size?: number | number[] | BigNumber | BigNumber[], format?: string): MathCollection
function ones(m: number | BigNumber, n: number | BigNumber, format?: string): MathCollection
function ones(m: number | BigNumber, n: number | BigNumber, p: number | BigNumber, format?: string): MathCollection
```

**Note:** Same behavior as `zeros()` — passing a single Array argument returns a plain JavaScript Array; numeric args or a format string return a Matrix.

### identity / eye

Create a 2D identity matrix (ones on the diagonal, zeros elsewhere). `eye` is an alias.

```typescript { .api }
function identity(size: number | number[] | MathCollection, format?: string): MathCollection | number
function identity(m: number, n: number, format?: string): MathCollection | number
```

### range

Create a 1D matrix (range vector) from a start/end/step specification.

```typescript { .api }
function range(str: string, includeEnd?: boolean): Matrix
function range(start: number | BigNumber, end: number | BigNumber, includeEnd?: boolean): Matrix
function range(
  start: number | BigNumber | Unit,
  end:   number | BigNumber | Unit,
  step:  number | BigNumber | Unit,
  includeEnd?: boolean
): Matrix
```

**Note:** The end value is **exclusive** by default. `range(0, 10, 2)` returns `[0, 2, 4, 6, 8]` (not `10`). Pass `true` as the final argument to make the end inclusive: `range(0, 10, 2, true)` returns `[0, 2, 4, 6, 8, 10]`. The string form follows the same rule: `range('0:2:10')` → `[0, 2, 4, 6, 8]`; `range('0:2:10', true)` → `[0, 2, 4, 6, 8, 10]`.

### reshape

Reshape a matrix to new dimensions without changing its elements.

```typescript { .api }
function reshape<T extends MathCollection>(x: T, sizes: number[]): T
```

### resize

Resize a matrix, filling new entries with a default value.

```typescript { .api }
function resize<T extends MathCollection>(x: T, size: MathCollection, defaultValue?: number | string): T
```

### squeeze

Remove singleton (size-1) dimensions from a matrix.

```typescript { .api }
function squeeze<T extends MathCollection>(x: T): T
```

### flatten

Flatten a multi-dimensional matrix into a single 1D matrix.

```typescript { .api }
function flatten<T extends MathCollection>(x: T): T
```

### matrixFromRows

Build a dense matrix from row vectors. Rows can be 1D arrays, column-vector arrays, or Matrix objects.

```typescript { .api }
function matrixFromRows(...rows: Matrix[]): Matrix
function matrixFromRows<T extends MathScalarType>(...rows: (T[] | [T][] | Matrix)[]): T[][]
```

**Note:** Returns a plain JavaScript 2D Array, not a DenseMatrix object, even when Matrix inputs are provided.

### matrixFromColumns

Build a dense matrix from column vectors. Row vectors are transposed (but not conjugated).

```typescript { .api }
function matrixFromColumns(...cols: Matrix[]): Matrix
function matrixFromColumns<T extends MathScalarType>(...cols: (T[] | [T][] | Matrix)[]): T[][]
```

**Note:** Returns a plain JavaScript 2D Array, not a DenseMatrix object.

### matrixFromFunction

Create a matrix by evaluating a generating callback at each index position.

```typescript { .api }
function matrixFromFunction<T extends MathScalarType>(size: [number], fn: MatrixFromFunctionCallback<T>): T[]
function matrixFromFunction<T extends MathScalarType>(size: [number, number], fn: MatrixFromFunctionCallback<T>): T[][]
function matrixFromFunction<T extends MathScalarType>(size: number[], fn: MatrixFromFunctionCallback<T>): MathArray<T>
function matrixFromFunction(size: Matrix<number>, fn: MatrixFromFunctionCallback<MathScalarType>): Matrix
function matrixFromFunction(
  size: number[] | Matrix<number>,
  fn: MatrixFromFunctionCallback<MathScalarType>,
  format: MatrixStorageFormat,
  datatype?: string
): Matrix
function matrixFromFunction(
  size:     number[] | Matrix<number>,
  format:   MatrixStorageFormat,
  fn:       MatrixFromFunctionCallback<MathScalarType>,
  datatype?: string
): Matrix
```

### rotationMatrix

Compute a 2D (theta only) or 3D (theta + axis) rotation matrix.

```typescript { .api }
function rotationMatrix<T extends MathCollection>(
  theta?:  number | BigNumber | Complex | Unit,
  axis?:   T,
  format?: 'sparse' | 'dense'
): T
```

---

## Matrix Operations

### add / subtract

Element-wise addition and subtraction. For matrices, both operands must be the same size or broadcastable.

```typescript { .api }
function add<T extends MathType>(x: T, y: T, ...values: T[]): T
function subtract<T extends MathType>(x: T, y: T): T
```

### multiply / divide

`multiply` performs matrix multiplication when both operands are matrices. `divide` multiplies the first operand by the inverse of the second for matrices (i.e., A / B = A * inv(B)).

```typescript { .api }
function multiply(x: MathCollection, y: MathCollection): MathCollection | number
function multiply(x: MathType, y: MathType, ...values: MathType[]): MathType
function divide(x: Unit, y: Unit): Unit | number
function divide(x: MathType, y: MathType): MathType
```

### dotMultiply / dotDivide

Element-wise (Hadamard) multiplication and division.

```typescript { .api }
function dotMultiply<T extends MathCollection>(x: T, y: MathType): T
function dotDivide<T extends MathCollection>(x: T, y: MathType): T
```

### dot

Dot (inner) product of two vectors: `A·B = Σ aᵢbᵢ`.

```typescript { .api }
function dot(x: MathCollection, y: MathCollection): number
```

### cross

3D cross product of two vectors.

```typescript { .api }
function cross(x: MathCollection, y: MathCollection): MathCollection
```

### kron

Kronecker (tensor) product of two matrices or vectors.

```typescript { .api }
function kron(x: MathCollection, y: MathCollection): Matrix
```

### transpose

Transpose a 2D matrix (reflect over main diagonal). Only two-dimensional matrices are supported.

```typescript { .api }
function transpose<T extends MathCollection>(x: T): T
```

### ctranspose

Conjugate transpose (Hermitian transpose): transpose then apply complex conjugation to each element.

```typescript { .api }
function ctranspose(x: MathCollection): MathCollection
```

### diag

Create a diagonal matrix from a vector, or extract the k-th diagonal from a matrix. Positive `k` targets the super-diagonal; negative `k` targets the sub-diagonal.

```typescript { .api }
function diag(X: MathCollection, format?: string): Matrix
function diag(X: MathCollection, k: number | BigNumber, format?: string): MathCollection
```

### trace

Sum of the elements on the main diagonal of a square matrix.

```typescript { .api }
function trace(x: MathCollection): number
```

### concat

Concatenate two or more matrices. An optional trailing numeric argument specifies the zero-based dimension (default: last dimension).

```typescript { .api }
function concat(...args: Array<MathCollection | number | BigNumber>): MathCollection
```

### size

Return the dimensions of a matrix or scalar as a vector.

```typescript { .api }
function size(x: boolean | number | Complex | Unit | string | MathCollection): MathCollection
```

### subset

Get or set a sub-matrix or sub-string using an `Index` object.

```typescript { .api }
function subset<T extends MathCollection | string>(
  value:        T,
  index:        Index,
  replacement?: any,
  defaultValue?: any
): T
```

### index

Construct an `Index` object for use with `subset`. (Part of the construction functions — see `math.index()`.)

```typescript { .api }
// Constructed via math.index():
// math.index(range1, range2, ...)
// Returns: Index
interface Index {}
```

### row / column

Extract a single row or column from a matrix as a new matrix.

```typescript { .api }
function row<T extends MathCollection>(value: T, row: number): T
function column<T extends MathCollection>(value: T, column: number): T
```

### diff

Finite differences between adjacent elements. An optional dimension argument selects the axis.

```typescript { .api }
function diff<T extends MathCollection>(x: T, dim?: number | BigNumber): T
```

---

## Linear Algebra

### det

Determinant of a square matrix.

```typescript { .api }
function det(x: MathCollection): number
```

### inv

Inverse of a square matrix (or scalar reciprocal for numbers/Complex).

```typescript { .api }
function inv<T extends number | Complex | MathCollection>(x: T): NoLiteralType<T>
```

### pinv

Moore–Penrose pseudoinverse of a matrix (or scalar).

```typescript { .api }
function pinv<T extends MathType>(x: T): T
```

### norm

Vector or matrix norm. `p` may be a number, `'inf'`, `'-inf'`, or `'fro'` (Frobenius). Default is 2-norm.

```typescript { .api }
function norm(
  x: number | BigNumber | Complex | MathCollection,
  p?: number | BigNumber | string
): number | BigNumber
```

### lup

LU decomposition with partial pivoting. Returns `{L, U, p}` where `A[p,:] = L * U`.

```typescript { .api }
function lup(A?: MathCollection): LUDecomposition
```

### lusolve

Solve linear system `Ax = b` using LU decomposition. Accepts a matrix `A`, an `LUDecomposition` result, or a sparse matrix with ordering/threshold for sparse factorization.

```typescript { .api }
function lusolve(A: Matrix,          b: MathCollection, order?: number, threshold?: number): Matrix
function lusolve(A: MathArray,       b: MathCollection, order?: number, threshold?: number): MathArray
function lusolve(A: LUDecomposition, b: MathCollection): Matrix
```

### qr

QR decomposition. Returns `{Q, R}` where `Q` is orthogonal and `R` is upper-triangular.

```typescript { .api }
function qr(A: MathCollection): QRDecomposition
```

### slu

Sparse LU decomposition with full pivoting. Returns `{L, U, p, q}` where `P * A * Q = L * U`.

`order` controls symbolic ordering:
- `0` — natural ordering (no column permutation `q` returned)
- `1` — square matrix; ordering on `M = A + A'`
- `2` — ordering on `M = A' * A` (drops dense columns; good for unsymmetric matrices)
- `3` — ordering on `M = A' * A` (best when `M` has no dense rows)

```typescript { .api }
function slu(A: Matrix, order: number, threshold: number): SLUDecomposition
```

### lsolve

Solve a lower-triangular linear system `Lx = b` by forward substitution.

```typescript { .api }
function lsolve(L: Matrix,    b: MathCollection): Matrix
function lsolve(L: MathArray, b: MathCollection): MathArray
```

### usolve

Solve an upper-triangular linear system `Ux = b` by back substitution.

```typescript { .api }
function usolve(U: Matrix,    b: MathCollection): Matrix
function usolve(U: MathArray, b: MathCollection): MathArray
```

### lsolveAll

Find **all** solutions of a lower-triangular linear system `Lx = b` using forward substitution. Unlike `lsolve`, handles singular matrices by returning the affine subspace of solutions. Returns an empty array when there is no solution.

```typescript { .api }
function lsolveAll(L: Matrix,    b: MathCollection): Matrix[]
function lsolveAll(L: MathArray, b: MathCollection): MathArray[]
```

### usolveAll

Find **all** solutions of an upper-triangular linear system `Ux = b` using back substitution. Unlike `usolve`, handles singular matrices by returning the affine subspace of solutions. Returns an empty array when there is no solution.

```typescript { .api }
function usolveAll(U: Matrix,    b: MathCollection): Matrix[]
function usolveAll(U: MathArray, b: MathCollection): MathArray[]
```

### schur

Real Schur decomposition `A = U T U'` where `U` is orthogonal and `T` is upper quasi-triangular.

```typescript { .api }
function schur(A: MathCollection): SchurDecomposition
```

### eigs

Compute eigenvalues and eigenvectors of a matrix. Eigenvalues are sorted by absolute value (ascending). A repeated eigenvalue with multiplicity `k` appears `k` times.

```typescript { .api }
function eigs(
  x:     MathCollection,
  opts?: number | BigNumber | { precision?: number | BigNumber; eigenvectors?: true }
): {
  values:      MathCollection
  eigenvectors: { value: number | BigNumber; vector: MathCollection }[]
}

function eigs(
  x:    MathCollection,
  opts: { eigenvectors: false; precision?: number | BigNumber }
): { values: MathCollection }
```

### sqrtm

Principal matrix square root `X` such that `X * X = A`.

```typescript { .api }
function sqrtm<T extends MathCollection>(A: T): T
```

### expm

Matrix exponential `e^A`. Uses a Padé approximant with scaling and squaring. Not to be confused with the element-wise `exp(A)`.

```typescript { .api }
function expm(x: Matrix): Matrix
```

### sylvester

Solve the Sylvester equation `AX + XB = C` using the Bartels–Stewart algorithm.

```typescript { .api }
function sylvester(A: MathCollection, B: MathCollection, C: MathCollection): MathCollection
```

### lyap

Solve the continuous-time Lyapunov equation `AP + PA' = Q` for `P`, where `Q` is positive semi-definite.

```typescript { .api }
function lyap(A: MathCollection, Q: MathCollection): MathCollection
```

---

## Iteration and Transformation

### map

Map a callback over every element of a matrix. Also supports iterating over multiple matrices simultaneously.

```typescript { .api }
function map<T extends MathCollection>(
  x:        T,
  callback: (value: any, index: number[], matrix: T) => MathType | string
): T

// Multi-matrix variant
function map<T extends MathCollection>(
  x:    T,
  ...args: Array<T | ((value: any, ...args: Array<any | number[] | T>) => MathType | string)>
): T
```

### forEach

Iterate over all elements of a matrix without returning a value.

```typescript { .api }
function forEach<T extends MathCollection>(
  x:        T,
  callback: (value: any, index: number[], matrix: T) => void
): void
```

### filter

Filter elements of a 1D matrix or array using a predicate function or RegExp.

```typescript { .api }
function filter(
  x:    MathCollection | string[],
  test: ((value: any, index: number[], matrix: MathCollection | string[]) => boolean) | RegExp
): MathCollection
```

### mapSlices

Apply a reducing function along a given axis (dimension). Returns a matrix with one fewer dimension. (`apply` is a deprecated alias.)

```typescript { .api }
function mapSlices<T extends MathCollection>(
  array:    T,
  dim:      number,
  callback: (array: MathCollection) => number
): T
```

### sort

Sort a 1D matrix. Comparator can be a function or one of the built-in names.

```typescript { .api }
function sort<T extends MathCollection>(
  x:       T,
  compare: ((a: any, b: any) => number) | 'asc' | 'desc' | 'natural'
): T
```

### partitionSelect

Partition-based kth-smallest selection (Quickselect). **Mutates** the input array. Returns the kth-lowest value.

```typescript { .api }
function partitionSelect(
  x:        MathCollection,
  k:        number,
  compare?: 'asc' | 'desc' | ((a: any, b: any) => number)
): any
```

---

## Matrix Inspection

### getMatrixDataType

Return the data type of the elements of a matrix as a string (e.g. `'number'`, `'BigNumber'`, `'Complex'`), or `'mixed'` if elements have mixed types.

```typescript { .api }
function getMatrixDataType(m: MathCollection): string
```

### count

Count the total number of elements in a matrix, array, or string.

```typescript { .api }
function count(x: MathCollection | string): number
```

---

## Matrix Testing

### isNumeric

Returns `true` if `x` is any numeric type (`number`, `BigNumber`, `bigint`, `Fraction`, or `boolean`).

```typescript { .api }
function isNumeric(x: any): x is number | BigNumber | bigint | Fraction | boolean
```

### hasNumericValue

Returns `true` if `x` is numeric or a string that can be parsed as a number. Works element-wise on arrays/matrices.

```typescript { .api }
function hasNumericValue(x: any): boolean | boolean[]
```

---

## Rotation

### rotate

Rotate a vector by angle `theta` around an optional axis `v`. Returns the product of the rotation matrix and `w`.

```typescript { .api }
function rotate<T extends MathCollection>(
  w:      T,
  theta:  number | BigNumber | Complex | Unit,
  v?:     T
): T
```
