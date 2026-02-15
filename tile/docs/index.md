# Math.js

Math.js is an extensive mathematical computation library for JavaScript and Node.js that provides comprehensive support for mathematical operations, multiple numeric types, symbolic computation, and expression parsing. It features 260+ built-in functions covering arithmetic, algebra, statistics, trigonometry, geometry, signal processing, matrix operations, and more, with full support for numbers, BigNumber (arbitrary precision), complex numbers, fractions, units, and matrices.

## Package Information

- **Package Name**: mathjs
- **Package Type**: npm
- **Language**: JavaScript/TypeScript
- **Installation**: `npm install mathjs`
- **Website**: https://mathjs.org

## Core Imports

ESM (recommended):

```javascript { .api }
import { add, sqrt, pi, matrix, evaluate } from 'mathjs';
```

Import everything:

```javascript { .api }
import * as math from 'mathjs';
```

CommonJS:

```javascript { .api }
const { add, sqrt, pi, matrix, evaluate } = require('mathjs');
```

Number-only build (lightweight, no BigNumber, Complex, Fraction, or Unit support):

```javascript { .api }
import { add, sqrt, pi } from 'mathjs/number';
```

## Basic Usage

```javascript
import { sqrt, pi, complex, evaluate } from 'mathjs';

// Basic arithmetic
sqrt(16);                    // 4

// Works with different numeric types
sqrt(-4);                    // 2i (complex number)

// Matrix operations
const matrix = [[1, 2], [3, 4]];
evaluate('det(A)', { A: matrix });  // -2

// Unit conversion
evaluate('5.08 cm to inch'); // 2 inch

// Expression evaluation
evaluate('sin(45 deg) ^ 2'); // 0.5

// Symbolic computation
evaluate('derivative(x^2 + x, x)');  // 2*x + 1
```

## Architecture

Math.js is organized around several key architectural components:

- **Default Export**: Full-featured math instance with all data types (number, BigNumber, Complex, Fraction, Unit)
- **Number-only Export**: Lightweight build supporting only native JavaScript numbers
- **Type System**: Multiple numeric types with automatic conversion and type safety
- **Expression Parser**: Flexible parser for mathematical expressions with support for variables, functions, and operators
- **Factory System**: Dependency injection system allowing custom math instances with selected functions
- **Immutable Operations**: All operations return new values, never modifying input
- **Chainable Interface**: Support for method chaining via `chain()` function

## Capabilities

### Type System

Core data types and type constructors for working with numbers, complex numbers, fractions, units, and matrices. Includes type checking functions and conversion utilities.

```typescript { .api }
function bignumber(x: number | string): BigNumber;
function complex(re: number, im?: number): Complex;
function fraction(numerator: number, denominator?: number): Fraction;
function unit(value: number | string, unit?: string): Unit;
function matrix(data: Array, format?: 'dense' | 'sparse'): Matrix;
```

[Type System](./types.md)

### Constants

Mathematical and physical constants including pi, e, i (imaginary unit), and 40+ physical constants.

```typescript { .api }
const pi: number;
const e: number;
const i: Complex;
const tau: number;
const phi: number;
```

[Constants Reference](./constants.md)

### Arithmetic Operations

Basic mathematical operations including addition, subtraction, multiplication, division, powers, roots, logarithms, and rounding functions. All operations support multiple data types.

```typescript { .api }
function add(x: MathType, y: MathType): MathType;
function multiply(x: MathType, y: MathType): MathType;
function pow(x: MathType, y: MathType): MathType;
function sqrt(x: MathType): MathType;
function log(x: MathType, base?: MathType): MathType;
```

[Arithmetic Operations](./arithmetic.md)

### Matrix and Linear Algebra

Comprehensive matrix operations including creation, manipulation, decomposition, and solving linear systems. Supports both dense and sparse matrices.

```typescript { .api }
function det(matrix: Matrix | Array): number;
function inv(matrix: Matrix | Array): Matrix;
function multiply(a: Matrix | Array, b: Matrix | Array): Matrix;
function transpose(matrix: Matrix | Array): Matrix;
function eigs(matrix: Matrix | Array): {values: Array, vectors: Array};
```

[Matrix Operations](./matrix.md)

### Statistics and Probability

Statistical functions for data analysis including mean, median, standard deviation, correlation, and probability distributions.

```typescript { .api }
function mean(data: Array | Matrix, dim?: number): number | Array;
function std(data: Array | Matrix, dim?: number, normalization?: string): number | Array;
function median(data: Array | Matrix): number | Array;
function random(size?: number | Array, min?: number, max?: number): number | Array | Matrix;
```

[Statistics and Probability](./statistics.md)

### Trigonometric Functions

Complete set of trigonometric functions including sine, cosine, tangent, and their inverses, plus hyperbolic variants.

```typescript { .api }
function sin(x: number | Complex | Unit): number | Complex;
function cos(x: number | Complex | Unit): number | Complex;
function tan(x: number | Complex | Unit): number | Complex;
function asin(x: number | Complex): number | Complex;
```

[Trigonometry](./trigonometry.md)

### Expression Parser and Evaluation

Parse and evaluate mathematical expressions as strings, with support for variables, custom functions, and symbolic computation.

```typescript { .api }
function evaluate(expr: string | Array<string>, scope?: Object): any;
function parse(expr: string): MathNode;
function compile(expr: string): EvalFunction;
function simplify(expr: string | MathNode, rules?: Array, scope?: Object): MathNode;
function derivative(expr: string | MathNode, variable: string): MathNode;
```

[Expression Parser](./parser.md)

### Unit Conversion

Create units with physical dimensions and convert between different unit systems. Supports 100+ built-in units and allows defining custom units.

```typescript { .api }
function unit(value: number | string, unitName?: string): Unit;
function createUnit(name: string, definition?: string | Object): Unit;
function to(value: Unit, targetUnit: string): Unit;
```

[Units](./units.md)

### Bitwise Operations

Bitwise operations on integers including AND, OR, XOR, NOT, and bit shifts.

```typescript { .api }
function bitAnd(x: number, y: number): number;
function bitOr(x: number, y: number): number;
function leftShift(x: number, y: number): number;
```

[Bitwise Operations](./bitwise.md)

### Set Operations

Mathematical set operations including union, intersection, difference, and set relations.

```typescript { .api }
function setUnion(a: Array, b: Array): Array;
function setIntersect(a: Array, b: Array): Array;
function setDifference(a: Array, b: Array): Array;
```

[Set Operations](./sets.md)

### Logical Operations

Boolean logic operations for conditional expressions.

```typescript { .api }
function and(x: boolean | number, y: boolean | number): boolean | number;
function or(x: boolean | number, y: boolean | number): boolean | number;
function not(x: boolean | number): boolean | number;
```

[Logical Operations](./logical.md)

### Relational Operations

Comparison and equality checking with support for all data types.

```typescript { .api }
function equal(x: MathType, y: MathType): boolean | Array;
function larger(x: MathType, y: MathType): boolean | Array;
function smaller(x: MathType, y: MathType): boolean | Array;
function compare(x: MathType, y: MathType): number;
```

[Relational Operations](./relational.md)

### String Formatting

Format numbers and values as strings with various representations.

```typescript { .api }
function format(value: any, options?: Object): string;
function hex(value: number): string;
function bin(value: number): string;
```

[String Formatting](./strings.md)

### Utility Functions

Type checking, cloning, and other utility functions.

```typescript { .api }
function clone(x: any): any;
function typeOf(x: any): string;
function isNumeric(x: any): boolean;
function isPrime(x: number): boolean;
```

[Utilities](./utils.md)

### Special Functions

Special mathematical functions including error function, zeta function, and combinatorics.

```typescript { .api }
function erf(x: number): number;
function gamma(x: number): number;
function factorial(n: number): number;
function combinations(n: number, k: number): number;
```

[Special Functions](./special.md)

### Geometry

Geometric functions for calculating distances and intersections in 2D and 3D space.

```typescript { .api }
function distance(x: MathArray | Matrix, y: MathArray | Matrix): number | BigNumber | Fraction;
function intersect(w: MathArray | Matrix, x: MathArray | Matrix, y: MathArray | Matrix, z: MathArray | Matrix): MathArray | Matrix | null;
```

[Geometry Functions](./geometry.md)

### Signal Processing

Signal processing functions for analyzing filters and converting between system representations.

```typescript { .api }
function zpk2tf(z: number[] | Matrix, p: number[] | Matrix, k?: number): { num: number[], den: number[] };
function freqz(b: number[] | Matrix, a: number[] | Matrix, w?: number | number[] | Matrix): { w: number[], h: Complex[], phase: number[] };
```

[Signal Processing](./signal.md)

### Chaining API

Fluent, chainable interface for sequential mathematical operations.

```typescript { .api }
function chain<T>(value: T): MathJsChain<T>;

interface MathJsChain<T> {
  done(): T;
  valueOf(): T;
  // All math.js functions available as chainable methods
}
```

[Chaining API](./chains.md)

### Configuration

Configure library behavior including number type, precision, and matrix format.

```typescript { .api }
function config(options: ConfigOptions): ConfigOptions;

interface ConfigOptions {
  number?: 'number' | 'BigNumber' | 'Fraction';
  precision?: number;
  matrix?: 'Matrix' | 'Array';
  randomSeed?: string | number | null;
}
```

[Configuration](./config.md)

### Factory and Customization

Create custom math instances with selected functions and dependencies.

```typescript { .api }
function create(factories?: Object, config?: Object): MathJsInstance;
function factory(name: string, dependencies: Array<string>, create: Function): Factory;
```

[Factory System](./factory.md)

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathScalarType = MathNumericType | Unit;
type MathArray<T> = T[] | Array<MathArray<T>>;
type MathCollection = MathArray<any> | Matrix;
type MathType = MathScalarType | MathCollection;
type MathExpression = string | string[] | MathCollection;

interface Matrix<T = any> {
  type: string;
  storage: string;
  size(): number[];
  get(index: number[]): T;
  set(index: number[], value: T): Matrix<T>;
  subset(index: Index, replacement?: any): Matrix<T>;
  toArray(): MathArray<T>;
  valueOf(): MathArray<T>;
}

interface Unit {
  value: number;
  units: Object;
  fixPrefix: boolean;
  skipAutomaticSimplification: boolean;
  toNumber(unit?: string): number;
  toString(): string;
  toJSON(): Object;
  format(options?: Object): string;
  to(unit: string): Unit;
  equals(other: Unit): boolean;
}

interface Complex {
  re: number;
  im: number;
  toString(): string;
  toJSON(): Object;
  toPolar(): {r: number, phi: number};
  format(options?: Object): string;
}

interface BigNumber {
  toString(): string;
  toNumber(): number;
  toFixed(decimals?: number): string;
  toPrecision(precision?: number): string;
  toExponential(decimals?: number): string;
}

interface Fraction {
  s: number;
  n: number;
  d: number;
  toString(): string;
  toFraction(excludeWhole?: boolean): string;
  toLatex(excludeWhole?: boolean): string;
}
```

## Error Classes

```typescript { .api }
class ArgumentsError extends Error {
  constructor(message: string);
}

class DimensionError extends Error {
  constructor(actual: number[], expected: number[], message?: string);
}

class IndexError extends Error {
  constructor(index: number, min?: number, max?: number);
}
```
