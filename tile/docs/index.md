# mathjs

mathjs is an extensive mathematics library for JavaScript and Node.js. It features a flexible expression parser with support for symbolic computation, a large set of built-in functions and constants, and an integrated solution to work with different data types including numbers, big numbers, complex numbers, fractions, physical units, strings, arrays, and matrices.

## Package Information

- **Package Name**: mathjs
- **Package Type**: npm
- **Language**: JavaScript/TypeScript
- **Installation**: `npm install mathjs`
- **Version**: 15.1.0

## Core Imports

ESM (full build):

```typescript
import * as math from 'mathjs'
// or named imports:
import { create, all, evaluate, sqrt, pi } from 'mathjs'
```

ESM (number-only, smaller bundle):

```typescript
import { create, allNumber } from 'mathjs/number'
```

CommonJS:

```javascript
const math = require('mathjs')
const { create, allNumber } = require('mathjs/number')
```

## Basic Usage

```typescript
import { evaluate, sqrt, pi, complex, matrix } from 'mathjs'

// Evaluate expressions
evaluate('sqrt(3^2 + 4^2)')           // 5
evaluate('2 inch to cm')              // 5.08 cm
evaluate('cos(45 deg)')               // 0.7071...

// Math functions
sqrt(25)                              // 5
pi                                    // 3.14159...

// Complex numbers
complex(3, -4).abs()                  // 5

// Matrices
matrix([[1, 2], [3, 4]])
```

Custom instance with configuration:

```typescript
import { create, all } from 'mathjs'

const math = create(all, {
  number: 'BigNumber',   // use BigNumber for all calculations
  precision: 64          // 64 significant digits
})

math.sqrt(math.bignumber(2))   // BigNumber with 64 digits precision
```

## Architecture

mathjs is built around a **factory pattern** that enables:
- **Tree-shaking**: Import only the functions you need
- **Custom instances**: Configure behavior (number type, precision, etc.)
- **Extensibility**: Add custom functions and units via `math.import()`
- **Chaining**: Fluent API via `math.chain()`

The library supports multiple numeric types: `number`, `BigNumber` (arbitrary precision), `bigint` (arbitrary integer), `Complex`, and `Fraction`.

## Capabilities

### Core and Arithmetic

Package setup, configuration, factory pattern, import/extend, chain API, typed functions, construction functions, and arithmetic operations (abs, add, subtract, multiply, divide, sqrt, pow, round, mod, gcd, invmod, etc.).

```typescript { .api }
import { create, all, evaluate, chain } from 'mathjs'

function create(factories: FactoryFunctionMap, config?: ConfigOptions): MathJsInstance;

interface ConfigOptions {
  relTol?: number         // default: 1e-12
  absTol?: number         // default: 1e-15
  matrix?: 'Matrix' | 'Array'  // default: 'Matrix'
  number?: 'number' | 'BigNumber' | 'bigint' | 'Fraction'  // default: 'number'
  precision?: number      // default: 64 (BigNumber digits)
  predictable?: boolean   // default: false
  randomSeed?: string | null
}
```

[Core and Arithmetic](./core.md)

### Types and Classes

All mathjs data types: `BigNumber`, `Complex`, `Fraction`, `Matrix`, `Unit`, `Index`, `ResultSet`, core type aliases, and supporting interfaces.

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex
type MathCollection<T = MathGeneric> = MathArray<T> | Matrix<T>
type MathType = MathScalarType | MathCollection

interface Matrix<T = MathGeneric> {
  get(index: number[]): T
  set(index: number[], value: T, defaultValue?: T): Matrix<T>
  resize(size: MathCollection, defaultValue?: T): Matrix<T>
  clone(): Matrix<T>
  size(): number[]
  toArray(): MathArray<T>
}
```

[Types and Classes](./types.md)

### Expression Parser

Parse and evaluate mathematical expression strings, compile for reuse, manage a stateful parser scope, and traverse/transform AST nodes.

```typescript { .api }
function evaluate(expr: MathExpression, scope?: MathScope): any;
function parse(expr: MathExpression, options?: ParseOptions): MathNode;
function compile(expr: MathExpression): EvalFunction;
function parser(): Parser;

interface Parser {
  evaluate(expr: string): any
  get(name: string): any
  set(name: string, value: any): void
  clear(): void
}
```

[Expression Parser](./expression-parser.md)

### Matrix and Linear Algebra

Create and manipulate matrices; perform linear algebra operations including LU/QR/Schur decomposition, eigenvalues, determinant, inverse, and solving linear systems (including all-solutions variants lsolveAll and usolveAll).

```typescript { .api }
function matrix(data?: MathArray | Matrix, format?: MatrixStorageFormat, datatype?: string): Matrix;
function zeros(size: number | number[] | MathCollection, ...sizes: number[]): MathCollection;
function ones(size: number | number[] | MathCollection, ...sizes: number[]): MathCollection;
function identity(size: number | number[] | MathCollection): MathCollection | number;
function det(x: MathCollection): number;
function inv(x: MathCollection): MathCollection;
function lup(A?: MathCollection): LUDecomposition;
function eigs(x: MathCollection, opts?: { precision?: number, eigenvectors?: boolean }): { values: MathCollection, eigenvectors?: Array<{value: number | BigNumber | Complex, vector: MathCollection}> };
```

[Matrix and Linear Algebra](./matrix.md)

### Algebra

Symbolic computation: simplify expressions, differentiate, rationalize, find polynomial roots, and test symbolic equality.

```typescript { .api }
function simplify(expr: MathNode | string, rules?: SimplifyRule[], scope?: MathScope, options?: SimplifyOptions): MathNode;
function derivative(expr: MathNode | string, variable: MathNode | string, options?: { simplify?: boolean }): MathNode;
function rationalize(expr: MathNode | string, scope?: MathScope, detailed?: false): MathNode;
function rationalize(expr: MathNode | string, scope?: MathScope, detailed?: true): { expression: MathNode, variables: string[], coefficients: MathType[] };
function polynomialRoot(constant: number | BigNumber | Complex, linearCoeff: number | BigNumber | Complex, quadraticCoeff?: number | BigNumber | Complex, cubicCoeff?: number | BigNumber | Complex): Array<number | Complex>;
```

[Algebra](./algebra.md)

### Trigonometry, Bitwise, and Logical

Trigonometric functions (standard, inverse, hyperbolic, inverse hyperbolic), bitwise operations (AND, OR, XOR, NOT, shifts), logical operations (and, or, not, xor, nullish), and complex number component extraction (re, im, arg, conj).

```typescript { .api }
function sin(x: number | BigNumber | Complex | Unit): number | BigNumber | Complex;
function cos(x: number | BigNumber | Complex | Unit): number | BigNumber | Complex;
function atan2(y: number | MathCollection, x: number | MathCollection): number | MathCollection;
function bitAnd(x: number | BigNumber | bigint | MathCollection, y: number | BigNumber | bigint | MathCollection): number | BigNumber | bigint | MathCollection;
```

[Trigonometry, Bitwise, and Logical](./trigonometry.md)

### Statistics and Probability

Descriptive statistics (mean, median, std, variance, etc.), probability functions (distributions, combinations, permutations, gamma), combinatorics, and special functions.

```typescript { .api }
function mean(...args: MathType[]): MathType;
function std(array: MathCollection, normalization?: 'unbiased' | 'uncorrected' | 'biased'): MathNumericType;
function random(min?: number, max?: number): number;
function random<T extends MathCollection>(size: T, min?: number, max?: number): T;
function combinations(n: number | BigNumber, k: number | BigNumber): number | BigNumber;
function factorial(n: number | BigNumber | MathCollection): number | BigNumber | MathCollection;
```

[Statistics and Probability](./statistics.md)

### Constants

Mathematical constants (e, pi, tau, phi, i, etc.) and physical constants (speed of light, Planck constant, Boltzmann constant, etc.).

```typescript { .api }
const e: number          // 2.718281828459045
const pi: number         // 3.141592653589793
const tau: number        // 6.283185307179586
const phi: number        // 1.618033988749895
const i: Complex         // Complex(0, 1)
const speedOfLight: Unit // 299792458 m/s
const planckConstant: Unit
```

[Constants](./constants.md)

### Utilities and Comparison

Comparison functions, set operations, signal processing (FFT), string formatting (including bin/oct/hex), unit conversion, type-check predicates, serialization (JSON reviver/replacer), error classes, and ODE solving (solveODE).

```typescript { .api }
function format(value: any, options?: FormatOptions | number | BigNumber | ((value: any) => string)): string;
function bin(value: number | BigNumber, wordSize?: number | BigNumber): string;
function oct(value: number | BigNumber, wordSize?: number | BigNumber): string;
function hex(value: number | BigNumber, wordSize?: number | BigNumber): string;
function typeOf(x: any): string;
function isNumeric(x: any): x is number | BigNumber | bigint | Fraction;
function isInteger(x: any): boolean;
function compare(x: MathType, y: MathType): number | BigNumber | Fraction;

// ODE solver
function solveODE(
  func: (t: any, y: any) => any,
  tspan: [any, any],
  y0: any,
  options?: { method?: 'RK23' | 'RK45', tol?: number, maxStep?: number }
): { t: number[], y: number[] | number[][] };

// JSON serialization
function reviver(): (key: any, value: any) => any;
function replacer(): (key: any, value: any) => any;
```

[Utilities and Comparison](./utilities.md)
