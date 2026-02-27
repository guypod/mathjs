# mathjs Algebra

mathjs v15.1.0 — Symbolic computation, simplification, differentiation, and algebraic functions.

---

## Core Types

```typescript { .api }
type MathNode     // expression parse-tree node (base interface)
type MathScope<TValue = any> = Record<string, TValue> | MapLike<string, TValue>
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
```

---

## Simplification

### SimplifyRule

A rule describing an algebraic transformation. Four forms are accepted:

```typescript { .api }
type SimplifyRule =
  | {
      l:              string           // left-hand side pattern
      r:              string           // right-hand side replacement
      repeat?:        boolean
      assuming?:      SimplifyContext
      imposeContext?: SimplifyContext
    }
  | {
      s:              string           // symmetric rule (both directions)
      repeat?:        boolean
      assuming?:      SimplifyContext
      imposeContext?: SimplifyContext
    }
  | string                             // shorthand "l = r" rule string
  | ((node: MathNode) => MathNode)     // transform function
```

### SimplifyContext

Maps operator names to their algebraic properties, controlling which simplifications are permitted.

```typescript { .api }
type SimplifyContext = Partial<
  Record<
    OperatorNodeFn,
    {
      trivial:      boolean   // op(x) = x (single-argument identity)
      total:        boolean   // op is defined for all arguments
      commutative:  boolean   // op(a,b) = op(b,a)
      associative:  boolean   // op(op(a,b),c) = op(a,op(b,c))
    }
  >
>
```

### SimplifyOptions

```typescript { .api }
interface SimplifyOptions {
  exactFractions?: boolean          // Return fractions instead of decimals where possible. Default: true
  fractionsLimit?: number           // Max numerator/denominator magnitude for exact fractions. Default: 10000
  consoleDebug?:   boolean          // Log rule applications to console. Default: false
  context?:        SimplifyContext  // Override per-operator algebraic properties
}
```

### Simplify interface

`math.simplify` is callable in three ways and also exposes the default rule set.

```typescript { .api }
interface Simplify {
  // Simplify with default rules
  (expr: MathNode | string): MathNode

  // Simplify with a custom rule set, optional scope and options
  (
    expr:     MathNode | string,
    rules:    SimplifyRule[],
    scope?:   MathScope,
    options?: SimplifyOptions
  ): MathNode

  // Simplify substituting scope variable values, with optional options
  (
    expr:     MathNode | string,
    scope:    MathScope,
    options?: SimplifyOptions
  ): MathNode

  /** The default set of simplification rules. */
  rules: SimplifyRule[]
}
```

### simplify

Simplify an expression tree by repeatedly applying transformation rules until no further changes occur.

```typescript { .api }
const simplify: Simplify
```

### simplifyConstant

Evaluate and collapse constant sub-expressions within a parse tree, leaving symbolic parts untouched.

```typescript { .api }
function simplifyConstant(expr: MathNode | string, options?: SimplifyOptions): MathNode
```

### simplifyCore

Apply core algebraic simplifications (identity elements, double negation, etc.) without the full rule set. Faster than `simplify` for light transformations.

```typescript { .api }
function simplifyCore(expr: MathNode | string, options?: SimplifyOptions): MathNode
```

---

## Differentiation

### derivative

Symbolically differentiate an expression with respect to a variable. The result is simplified by default.

```typescript { .api }
function derivative(
  expr:     MathNode | string,
  variable: MathNode | string,
  options?: { simplify: boolean }   // simplify defaults to true
): MathNode
```

---

## Rationalization

### rationalize

Transform a rationalizable expression into a rational fraction polynomial. When `detailed` is `true`, returns a structured object containing the expression, variable names, and polynomial coefficients (numerator, in decreasing exponent order).

```typescript { .api }
function rationalize(
  expr:      MathNode | string,
  optional?: object | boolean,   // scope or true for pre-evaluated input
  detailed?: false
): MathNode

function rationalize(
  expr:      MathNode | string,
  optional?: object | boolean,
  detailed:  true
): {
  expression:   MathNode | string
  variables:    string[]
  coefficients: MathType[]
}
```

---

## Parse-Tree Analysis

### leafCount

Count the number of leaf nodes (symbols and constants) in a parse tree. Unary operators do not add a leaf; function symbols do.

```typescript { .api }
function leafCount(expr: MathNode): number
```

---

## Polynomial

### polynomialRoot

Find all roots of a polynomial of degree three or less. Coefficients are given in ascending order (constant first).

```typescript { .api }
function polynomialRoot(
  constantCoeff:    number | Complex,
  linearCoeff:      number | Complex,
  quadraticCoeff?:  number | Complex,
  cubicCoeff?:      number | Complex
): (number | Complex)[]
```

---

## Symbolic Equality

### symbolicEqual

Determine whether two expression trees are symbolically equal by attempting to find a valid algebraic manipulation that equates them (internally uses `simplify`).

```typescript { .api }
function symbolicEqual(
  expr1:    MathNode,
  expr2:    MathNode,
  options?: SimplifyOptions
): boolean
```

---

## Variable Resolution

### resolve

Replace `SymbolNode`s in a parse tree with their values from a scope, returning a new tree with those nodes substituted.

```typescript { .api }
function resolve(node: MathNode | string,            scope?: MathScope): MathNode
function resolve(node: (MathNode | string)[],        scope?: MathScope): MathNode[]
function resolve(node: Matrix,                       scope?: MathScope): Matrix
```

---

## Linear System Solvers

### lup

LU decomposition with partial pivoting. Returns `{L, U, p}` where `A[p,:] = L * U`.

```typescript { .api }
function lup(A?: MathCollection): LUDecomposition
```

### lusolve

Solve linear system `Ax = b`. Accepts a plain matrix, an already-computed `LUDecomposition`, or a sparse matrix with symbolic ordering and pivoting parameters.

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

`order` controls symbolic ordering and analysis:
- `0` — natural ordering; no column permutation `q` is returned
- `1` — square matrix; symbolic ordering on `M = A + A'`
- `2` — symbolic ordering on `M = A' * A` (drops dense columns from `A'`; suitable for unsymmetric matrices)
- `3` — symbolic ordering on `M = A' * A` (best when `M` has no dense rows)

```typescript { .api }
function slu(A: Matrix, order: number, threshold: number): SLUDecomposition
```

### lsolve

Solve lower-triangular system `Lx = b` by forward substitution. `L` must be a lower-triangular matrix.

```typescript { .api }
function lsolve(L: Matrix,    b: MathCollection): Matrix
function lsolve(L: MathArray, b: MathCollection): MathArray
```

### usolve

Solve upper-triangular system `Ux = b` by back substitution. `U` must be an upper-triangular matrix.

```typescript { .api }
function usolve(U: Matrix,    b: MathCollection): Matrix
function usolve(U: MathArray, b: MathCollection): MathArray
```

### lsolveAll

Find **all** solutions of a lower-triangular system `Lx = b`. Returns the complete affine subspace of solutions for singular matrices, and an empty array when there is no solution.

```typescript { .api }
function lsolveAll(L: Matrix,    b: MathCollection): Matrix[]
function lsolveAll(L: MathArray, b: MathCollection): MathArray[]
```

### usolveAll

Find **all** solutions of an upper-triangular system `Ux = b`. Returns the complete affine subspace of solutions for singular matrices, and an empty array when there is no solution.

```typescript { .api }
function usolveAll(U: Matrix,    b: MathCollection): Matrix[]
function usolveAll(U: MathArray, b: MathCollection): MathArray[]
```
