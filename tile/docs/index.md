# Math.js

Math.js is an extensive mathematical computation library for JavaScript and Node.js. It provides a flexible expression parser with symbolic computation support, multiple numeric types (numbers, BigNumber, bigint, Complex, Fraction), physical units with conversions, matrix operations, and over 200 built-in mathematical functions covering arithmetic, algebra, trigonometry, calculus, statistics, and linear algebra.

## Package Information

- **Package Name**: mathjs
- **Package Type**: npm
- **Language**: JavaScript/TypeScript
- **Installation**: `npm install mathjs`

## Core Imports

```javascript
import { create, all } from 'mathjs'

// Import all functions (default)
import * as math from 'mathjs'

// Import specific functions for tree-shaking
import { sqrt, pow, derivative, matrix, evaluate } from 'mathjs'
```

For CommonJS:

```javascript
const math = require('mathjs')
const { sqrt, pow, derivative } = require('mathjs')
```

## Factory Pattern and Custom Instances

**IMPORTANT**: The default import from mathjs provides a **read-only** instance with fixed configuration. To modify configuration or create custom instances, you must use the factory pattern with `create()` and `all`.

### Creating Custom Instances

```javascript { .api }
import { create, all } from 'mathjs'

// Create a custom mathjs instance with all functions
const math = create(all)

// Now you can modify configuration
math.config({
  number: 'BigNumber',
  precision: 128
})

// Use the custom instance
const result = math.evaluate('1/3')  // Returns BigNumber with custom precision
```

### Why Use Custom Instances?

- **Mutable Configuration**: Change numeric types, precision, tolerances at runtime
- **Isolated State**: Multiple instances with different configurations
- **Custom Builds**: Include only needed functions (use selective imports instead of `all`)

### Configuration Rules

1. **Global Import**: Configuration is read-only
   ```javascript
   import { config, evaluate } from 'mathjs'
   config({ precision: 128 })  // ❌ ERROR: Global config is readonly
   ```

2. **Custom Instance**: Configuration is mutable
   ```javascript
   import { create, all } from 'mathjs'
   const math = create(all)
   math.config({ precision: 128 })  // ✅ Works
   ```

### Custom Builds

For smaller bundle sizes, import only needed functions:

```javascript { .api }
import { create, evaluateDependencies, parseDependencies } from 'mathjs'

const math = create({
  evaluateDependencies,
  parseDependencies
})

// Only evaluate and parse are available
math.evaluate('2 + 3')  // ✅ Works
math.sqrt(4)  // ❌ Not available in this instance
```

## Basic Usage

```javascript
import { sqrt, pow, evaluate, derivative, matrix, unit } from 'mathjs'

// Basic arithmetic with functions
sqrt(25)  // 5
pow(2, 3) // 8

// Expression evaluation
evaluate('12 / (2.3 + 0.7)')    // 4
evaluate('sin(45 deg) ^ 2')     // 0.5
evaluate('det([-1, 2; 3, 1])')  // -7

// Symbolic computation
derivative('x^2 + x', 'x')  // '2 * x + 1'

// Matrix operations
const a = matrix([[1, 2], [3, 4]])
const b = matrix([[5, 6], [7, 8]])
// Element-wise operations, linear algebra, etc.

// Units
evaluate('5.08 cm to inch')  // 2 inch
unit('10 kg').to('pound')    // 22.046... lbm
```

## Architecture

Math.js is built around several key architectural components:

- **Type System**: Supports multiple numeric types (number, BigNumber, bigint, Complex, Fraction) with automatic type detection and conversion
- **Expression Parser**: Flexible parser that converts string expressions into executable Abstract Syntax Trees (AST)
- **Factory Pattern**: Modular design allowing custom builds with only needed functions
- **Matrix System**: Dense and sparse matrix implementations with comprehensive linear algebra operations
- **Unit System**: Physical units with automatic conversions across dimensions
- **Chain Interface**: Fluent API for composing mathematical operations
- **Symbolic Computation**: Simplification, rationalization, and differentiation of symbolic expressions

## Configuration

**Note**: Configuration can only be modified on custom mathjs instances created with `create(all)`. The default import has read-only configuration. See [Factory Pattern and Custom Instances](#factory-pattern-and-custom-instances) above.

Math.js behavior can be customized via configuration:

```javascript { .api }
import { create, all } from 'mathjs'

const math = create(all)

math.config({
  number?: 'number' | 'BigNumber' | 'Fraction',  // Default numeric type
  matrix?: 'Matrix' | 'Array',                   // Default matrix type
  precision?: number,                             // BigNumber precision (default 64)
  relTol?: number,                                // Relative tolerance for comparisons
  absTol?: number,                                // Absolute tolerance for comparisons
  randomSeed?: string | null,                     // Seed for random number generation
  parenthesis?: 'keep' | 'auto' | 'all'          // Parenthesis display mode
})
```

## Capabilities

### Arithmetic Operations

Basic and advanced arithmetic including standard operations, logarithms, roots, rounding, and greatest common divisor calculations.

```javascript { .api }
function abs(x: MathType): MathType
function add(x: MathType, y: MathType, ...values: MathType[]): MathType
function ceil(x: MathType, n?: number | BigNumber): MathType
function divide(x: MathType, y: MathType): MathType
function exp(x: MathNumericType): MathNumericType
function floor(x: MathType, n?: number | BigNumber): MathType
function gcd(...args: MathType[]): MathType
function log(x: MathNumericType, base?: MathNumericType): MathNumericType
function mod(x: MathType, y: MathType): MathType
function multiply(x: MathType, y: MathType, ...values: MathType[]): MathType
function pow(x: MathType, y: MathNumericType): MathType
function round(x: MathType, n?: number | BigNumber): MathType
function sqrt(x: MathType): MathType
function subtract(x: MathType, y: MathType): MathType
```

[Arithmetic Operations](./arithmetic.md)

### Trigonometric Functions

Complete set of trigonometric, inverse trigonometric, hyperbolic, and inverse hyperbolic functions for angular calculations.

```javascript { .api }
function sin(x: MathType): MathType
function cos(x: MathType): MathType
function tan(x: MathType): MathType
function asin(x: MathNumericType): MathNumericType
function acos(x: MathNumericType): MathNumericType
function atan(x: MathNumericType): MathNumericType
function atan2(y: MathType, x: MathType): MathType
function sinh(x: MathType): MathType
function cosh(x: MathType): MathType
function tanh(x: MathType): MathType
```

[Trigonometric Functions](./trigonometry.md)

### Matrix Operations

Comprehensive linear algebra operations including basic matrix operations, decompositions, and linear system solvers.

```javascript { .api }
function matrix(data?: MathCollection, format?: 'dense' | 'sparse', dataType?: string): Matrix
function det(x: MathCollection): number
function inv(x: MathCollection): MathCollection
function transpose(x: MathCollection): MathCollection
function multiply(x: Matrix, y: Matrix): Matrix
function eigs(x: MathCollection): { values: MathCollection, vectors: MathCollection }
function lup(A: MathCollection): { L: MathCollection, U: MathCollection, p: number[] }
function qr(A: MathCollection): { Q: MathCollection, R: MathCollection }
```

[Matrix Operations](./matrix.md)

### Statistics and Probability

Statistical functions for data analysis including descriptive statistics, probability distributions, and random number generation.

```javascript { .api }
function mean(...args: MathType[]): MathType
function mean(A: MathCollection, dimension?: number): MathCollection | number
function median(...args: MathType[]): MathType
function std(array: MathCollection, normalization?: 'unbiased' | 'uncorrected' | 'biased'): number
function variance(array: MathCollection, normalization?: 'unbiased' | 'uncorrected' | 'biased'): number
function max(...args: MathType[]): MathType
function min(...args: MathType[]): MathType
function random(min?: number, max?: number): number
function randomInt(min: number, max?: number): number
```

[Statistics and Probability](./statistics.md)

### Expression Parsing and Evaluation

Parse and evaluate mathematical expressions from strings, with support for variables, functions, and symbolic computation.

```javascript { .api }
function evaluate(expr: string | string[], scope?: object): any
function parse(expr: string): MathNode
function compile(expr: string): EvalFunction
function simplify(expr: string | MathNode, rules?: object[], scope?: object): MathNode
function derivative(expr: string | MathNode, variable: string | MathNode): MathNode
```

[Expression Parsing](./expressions.md)

### Complex Numbers

Operations for complex number arithmetic and properties.

```javascript { .api }
function complex(re?: number, im?: number): Complex
function complex(arg: { r: number, phi: number }): Complex
function re(x: Complex): number
function im(x: Complex): number
function arg(x: Complex): number
function conj(x: Complex): Complex
```

[Complex Numbers](./complex.md)

### Type Constructors

Create and convert between different numeric types including BigNumber (arbitrary precision), Fraction (rational), and standard numbers.

```javascript { .api }
function bignumber(x?: number | string): BigNumber
function bigint(x?: number | string): bigint
function fraction(value: number | string): Fraction
function fraction(numerator: number, denominator: number): Fraction
function number(value: any): number
function boolean(x: any): boolean
function string(value: any): string
```

[Type Constructors](./types.md)

### Units

Physical units with automatic conversions and arithmetic operations.

```javascript { .api }
function unit(value?: number | string, unit?: string): Unit
function createUnit(name: string, definition?: string | object): Unit
function to(x: Unit, unit: string): Unit
```

[Units](./units.md)

### Logical and Comparison Operations

Boolean logic, comparison operators, and relational operations.

```javascript { .api }
function and(x: MathType, y: MathType): boolean | MathCollection
function or(x: MathType, y: MathType): boolean | MathCollection
function xor(x: MathType, y: MathType): boolean | MathCollection
function not(x: MathType): boolean | MathCollection
function equal(x: MathType, y: MathType): boolean | MathCollection
function larger(x: MathType, y: MathType): boolean | MathCollection
function smaller(x: MathType, y: MathType): boolean | MathCollection
```

[Logical Operations](./logical.md)

### Bitwise Operations

Binary operations on integers including shifts and bitwise logic.

```javascript { .api }
function bitAnd(x: MathType, y: MathType): MathType
function bitOr(x: MathType, y: MathType): MathType
function bitXor(x: MathType, y: MathType): MathType
function bitNot(x: MathType): MathType
function leftShift(x: MathType, y: number): MathType
function rightArithShift(x: MathType, y: number): MathType
function rightLogShift(x: MathType, y: number): MathType
```

[Bitwise Operations](./bitwise.md)

### Combinatorics and Special Functions

Functions for combinatorial mathematics and special mathematical functions.

```javascript { .api }
function factorial(n: MathType): MathType
function combinations(n: MathType, k: MathType): MathType
function permutations(n: MathType, k?: MathType): MathType
function gamma(n: MathNumericType): MathNumericType
function erf(x: number): number
function zeta(s: number | Complex | BigNumber): number | Complex | BigNumber
```

[Combinatorics](./combinatorics.md)

### Set Operations

Mathematical set operations on arrays.

```javascript { .api }
function setUnion(a1: MathCollection, a2: MathCollection): MathCollection
function setIntersect(a1: MathCollection, a2: MathCollection): MathCollection
function setDifference(a1: MathCollection, a2: MathCollection): MathCollection
function setDistinct(a: MathCollection): MathCollection
function setIsSubset(a1: MathCollection, a2: MathCollection): boolean
```

[Set Operations](./sets.md)

### Type Checking

Comprehensive type checking utilities for all math.js types.

```javascript { .api }
function isNumber(x: any): x is number
function isBigNumber(x: any): x is BigNumber
function isComplex(x: any): x is Complex
function isFraction(x: any): x is Fraction
function isUnit(x: any): x is Unit
function isMatrix(x: any): x is Matrix
function isArray(x: any): x is Array
function typeOf(x: any): string
```

[Type Checking](./type-checking.md)

### Chain Interface

Fluent API for composing operations with automatic result chaining.

```javascript { .api }
function chain(value?: any): MathJsChain

interface MathJsChain {
  // All math functions available as methods
  done(): any
}
```

[Chain Interface](./chaining.md)

### Constants

Mathematical and physical constants.

[Constants](./constants.md)

### Extension and Customization

Import custom functions and constants into a math.js instance.

```javascript { .api }
function import(object: object | object[], options?: { override?: boolean, silent?: boolean, wrap?: boolean }): void
```

**Usage Examples:**

```javascript
import { create, all } from 'mathjs'

const math = create(all)

// Import custom functions
math.import({
  myCustomFunction: function (x) {
    return x * 2 + 1
  }
})

math.myCustomFunction(5)  // 11

// Import with options
math.import({
  sin: function (x) {
    return 'my custom sin'
  }
}, { override: true })
```

### Geometry

Calculate distances and find intersections between geometric entities.

```javascript { .api }
function distance(x: MathCollection | object, y: MathCollection | object, z?: MathCollection | object): number | BigNumber

function intersect(w: MathCollection, x: MathCollection, y: MathCollection, z?: MathCollection): MathArray
```

**Usage Examples:**

```javascript
import { distance, intersect } from 'mathjs'

// Distance between two points
distance([0, 0], [3, 4])  // 5

// Distance between point and line in 2D
distance([2, 3], [0, 0], [4, 0])  // 3

// Intersection of two lines
intersect([0, 0], [1, 1], [0, 1], [1, 0])  // [0.5, 0.5]
```

### JSON Serialization

Serialize and deserialize math.js types to/from JSON.

```javascript { .api }
function replacer(): (key: any, value: any) => any

function reviver(): (key: any, value: any) => any
```

**Usage Examples:**

```javascript
import { bignumber, complex, unit, replacer, reviver } from 'mathjs'

const obj = {
  a: bignumber('1.5'),
  b: complex(2, 3),
  c: unit('5 cm')
}

// Serialize
const json = JSON.stringify(obj, replacer())

// Deserialize
const restored = JSON.parse(json, reviver())
```

### Utilities

Utility functions for help, formatting, and output.

```javascript { .api }
function help(search: Function | string): Help

function format(value: any, options?: FormatOptions | number | ((item: any) => string)): string

function print(template: string, values: any, precision?: number, options?: number | object): string
```

**Usage Examples:**

```javascript
import { help, format, print } from 'mathjs'

// Get help on a function
const helpInfo = help('sqrt')
console.log(helpInfo.toString())

// Format values
format(12.3456789, 3)        // '12.3'
format(math.pi, { notation: 'fixed', precision: 2 })  // '3.14'

// Template interpolation
print('The answer is $x', { x: 42 })           // 'The answer is 42'
print('Value: $value', { value: 2/3 }, 4)     // 'Value: 0.6667'
```

## Types

```javascript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex
type MathScalarType = MathNumericType | Unit
type MathArray<T> = T[] | Array<MathArray<T>>
type MathCollection<T> = MathArray<T> | Matrix<T>
type MathType = MathScalarType | MathCollection
type MathExpression = string | string[] | MathCollection

interface Matrix<T = any> {
  // Dense or sparse matrix
  size(): number[]
  get(index: number[]): T
  set(index: number[], value: T): Matrix<T>
  subset(index: Index, replacement?: any): Matrix<T>
  // ... additional matrix methods
}

interface Complex {
  re: number  // Real part
  im: number  // Imaginary part
}

interface Unit {
  value: number | BigNumber | Fraction
  // Unit operations like to(), toNumber(), etc.
}

interface BigNumber {
  // Arbitrary precision decimal (from decimal.js)
}

interface Fraction {
  n: number   // Numerator
  d: number   // Denominator
}

interface MathNode {
  // Abstract syntax tree node
  compile(): EvalFunction
  evaluate(scope?: object): any
  toString(): string
  // ... additional node methods
}

interface EvalFunction {
  (scope?: object): any
  evaluate(scope?: object): any
}
```
