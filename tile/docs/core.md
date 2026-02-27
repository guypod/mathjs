# mathjs Core and Arithmetic

mathjs (v15.1.0) is an extensive math library for JavaScript and Node.js. This document covers core functionality — package initialization, configuration, the factory pattern, imports, the chain API, typed functions, construction functions, and arithmetic functions.

---

## Package Initialization and Imports

### ESM — Full Build

```typescript { .api }
// Import the default (full) instance
import * as math from 'mathjs'

// Named imports from the full build
import { create, all, sqrt, evaluate } from 'mathjs'
```

### ESM — Number-Only Build

The number-only build excludes BigNumber, Fraction, and Complex support, resulting in a smaller bundle.

```typescript { .api }
// Import from the number-only entry point
import * as math from 'mathjs/number'

import { create, all } from 'mathjs/number'
```

### CommonJS

```typescript { .api }
// Full build
const math = require('mathjs')

// Number-only build
const math = require('mathjs/number')
```

### Custom Instance via Factory

```typescript { .api }
import { create, all } from 'mathjs'

// Create an instance with all functions and optional initial config
const math = create(all, {
  number: 'BigNumber',
  precision: 32
})
```

### Version String

```typescript { .api }
// The library version string (e.g. '15.1.0')
const version: string
```

---

## Configuration

### `config`

```typescript { .api }
/**
 * Get or set configuration options for the math instance.
 * Emits a 'config' event with arguments (curr, prev, changes) on change.
 * @param options - Partial configuration options to apply
 * @returns The current (updated) configuration
 */
config(options: ConfigOptions): ConfigOptions
```

### `ConfigOptions` Interface

```typescript { .api }
interface ConfigOptions {
  /**
   * Minimum relative difference between two compared values, used by all
   * comparison functions. Default: 1e-12.
   */
  relTol?: number

  /**
   * Minimum absolute difference between two compared values, used by all
   * comparison functions. Default: 1e-15.
   */
  absTol?: number

  /**
   * @deprecated Use `relTol` and `absTol` instead.
   */
  epsilon?: number

  /**
   * Default matrix output type. Either 'Matrix' (default) or 'Array'.
   */
  matrix?: 'Matrix' | 'Array'

  /**
   * Default numeric type. One of 'number' (default), 'BigNumber',
   * 'bigint', or 'Fraction'.
   */
  number?: 'number' | 'BigNumber' | 'bigint' | 'Fraction'

  /**
   * Fallback numeric type when parsing fails. One of 'number' or 'BigNumber'.
   */
  numberFallback?: 'number' | 'BigNumber'

  /**
   * Number of significant digits for BigNumber arithmetic.
   * Not applicable for standard number type. Default: 64.
   */
  precision?: number

  /**
   * When true, operations always return real results rather than
   * complex numbers. Default: false.
   */
  predictable?: boolean

  /**
   * Seed for the pseudo-random number generator. Set to null (default)
   * to use a random seed on each run.
   */
  randomSeed?: string | null
}
```

**Example:**

```typescript { .api }
// Read current config
const current = math.config({})

// Change to BigNumber mode with 32 significant digits
math.config({ number: 'BigNumber', precision: 32 })
```

---

## Factory Pattern

### `create`

```typescript { .api }
/**
 * Create a new MathJsInstance from a map of factory functions.
 * Pass `all` to include every built-in function, or a subset for
 * tree-shaking. An optional initial configuration may be provided.
 * @param factories - A map of factory functions (e.g., `all` or a subset)
 * @param config - Optional initial configuration
 * @returns A new configured math instance
 */
create(factories: FactoryFunctionMap, config?: ConfigOptions): MathJsInstance
```

### `factory`

```typescript { .api }
/**
 * Create a FactoryFunction with typed dependency injection.
 * @param name - Name of the function being created
 * @param dependencies - Array of dependency names to inject
 * @param create - Function receiving injected dependencies, returns the implementation
 * @param meta - Optional metadata
 * @returns A FactoryFunction that can be passed to `create()`
 */
factory<T, TDeps extends readonly MathJsFunctionName[]>(
  name: string,
  dependencies: TDeps,
  create: (injected: Pick<MathJsInstance, Extract<MathJsFunctionName, TDeps[number]>>) => T,
  meta?: any
): FactoryFunction<T>
```

**Example — custom instance with tree-shaking:**

```typescript { .api }
import { create, addDependencies, multiplyDependencies } from 'mathjs'

const math = create({ addDependencies, multiplyDependencies })
```

**Dependency exports:** Every built-in function is paired with a corresponding `*Dependencies` export (e.g. `addDependencies`, `sqrtDependencies`, `evaluateDependencies`). These are `FactoryFunctionMap` objects that list the transitive dependencies of each function. Importing only the `*Dependencies` objects you need and passing them to `create()` enables dead-code elimination (tree-shaking). The complete list is available in the `types/index.d.ts` file under the `/* Factory Exports */` section.

---

## Import / Extend

### `math.import`

```typescript { .api }
/**
 * Extend the math instance with new functions, constants, or type overrides.
 * Imported names become available in the expression parser.
 * @param object - An object (or array of objects) mapping names to values
 * @param options - Import options controlling override and wrapping behavior
 */
import(object: ImportObject | ImportObject[], options?: ImportOptions): void
```

### `ImportOptions`

```typescript { .api }
interface ImportOptions {
  /**
   * Allow overriding existing functions or constants. Default: false.
   */
  override?: boolean

  /**
   * Suppress errors when a function already exists. Default: false.
   */
  silent?: boolean

  /**
   * Wrap plain functions so they support element-wise operations on
   * arrays and matrices. Default: false.
   */
  wrap?: boolean
}
```

**Example:**

```typescript { .api }
// Add a custom function
math.import({
  myFunc: (x: number) => x * 2
}, { override: false })

// TypeScript module augmentation for type safety
declare module 'mathjs' {
  interface MathJsInterface {
    myFunc(x: number): number
  }
}
```

---

## Chain API

### `chain`

```typescript { .api }
/**
 * Wrap a value in a MathJsChain for fluent method chaining.
 * All math functions are available as methods on the chain.
 * The chained value is passed as the first argument automatically.
 * @param value - The initial value to wrap
 * @returns A MathJsChain wrapping the value
 */
chain<TValue>(value?: TValue): MathJsChain<TValue>
```

### `MathJsChain` Interface

```typescript { .api }
interface MathJsChain<TValue> {
  /**
   * Finalize the chain and return the current value.
   * @returns The unwrapped value
   */
  done(): TValue

  /**
   * Same as done(). Returns the current value.
   * @returns The unwrapped value
   */
  valueOf(): TValue

  /**
   * Format the current value as a string using math.format().
   * @returns String representation of the current value
   */
  toString(): string

  // Every math function is available as a chained method.
  // Each method returns MathJsChain<Result>, enabling further chaining.
  // Example methods (non-exhaustive):
  abs(): MathJsChain<number>
  add(y: MathType): MathJsChain<MathType>
  multiply(y: MathType): MathJsChain<MathType>
  // ... all other math functions follow the same pattern
}
```

**Example:**

```typescript { .api }
const result = math.chain(3)
  .add(4)
  .multiply(2)
  .done()
// result === 14
```

---

## typed

### `math.typed`

```typescript { .api }
/**
 * Create a typed-function that dispatches on argument types and supports
 * automatic type conversion. Typed functions throw informative errors for
 * invalid inputs.
 * @param name - Name for the typed-function (used in error messages)
 * @param signatures - Object mapping type signature strings to implementations
 * @returns A typed-function that selects the matching signature at runtime
 */
typed(
  name: string,
  signatures: Record<string, (...args: any[]) => any>
): (...args: any[]) => any
```

**Example:**

```typescript { .api }
const myFunc = math.typed('myFunc', {
  'number': (x: number) => x * 2,
  'BigNumber': (x: BigNumber) => x.times(2),
  'Array | Matrix': (x: MathCollection) => math.map(x, (v) => v * 2)
})
```

---

## Construction Functions

### `bignumber`

```typescript { .api }
/**
 * Create a BigNumber with arbitrary precision.
 * When a matrix is provided, all elements are converted element-wise.
 * @param x - Value to convert. Defaults to 0.
 * @returns A BigNumber
 */
bignumber(
  x?: number | string | Fraction | BigNumber | bigint | Unit | boolean | null
): BigNumber
bignumber<T extends MathCollection>(x: T): T
```

### `bigint`

```typescript { .api }
/**
 * Create a native JavaScript bigint with arbitrary-precision integer arithmetic.
 * When a matrix is provided, all elements are converted element-wise.
 * @param x - Value to convert. Defaults to 0.
 * @returns A bigint
 */
bigint(
  x?: number | string | Fraction | BigNumber | bigint | boolean | null
): bigint
bigint<T extends MathCollection>(x: T): T
```

### `boolean`

```typescript { .api }
/**
 * Create or convert a value to boolean.
 * Non-zero numbers return true; zero returns false.
 * Strings must be 'true', 'false', or a numeric string.
 * @param x - Value to convert
 * @returns boolean representation of x
 */
boolean(x: string | number | boolean | null): boolean
boolean(x: MathCollection): MathCollection
```

### `chain`

```typescript { .api }
/**
 * Wrap a value in a chain for fluent method chaining.
 * @param value - A value of any type
 * @returns A MathJsChain wrapping the value
 */
chain<TValue>(value?: TValue): MathJsChain<TValue>
```

### `complex`

```typescript { .api }
/**
 * Create a complex number.
 * @param arg - A numeric value, string, or polar coordinates object
 * @returns A Complex number
 */
complex(arg?: MathNumericType | string | PolarCoordinates): Complex
complex(arg?: MathCollection): MathCollection

/**
 * @param re - Real part
 * @param im - Imaginary part
 * @returns A Complex number
 */
complex(re: number, im: number): Complex
```

### `createUnit`

```typescript { .api }
/**
 * Create a custom unit and register it globally with the Unit type.
 * @param name - Unique name for the new unit (e.g., 'knot')
 * @param definition - Definition in terms of existing units (e.g., '0.514444 m/s')
 * @param options - Optional: prefixes, aliases, offset, override
 * @returns The new Unit
 */
createUnit(
  name: string,
  definition?: string | UnitDefinition | Unit,
  options?: CreateUnitOptions
): Unit

/**
 * Create multiple custom units at once.
 * @param units - Map of unit names to definitions
 * @param options - Optional unit creation options
 * @returns The last created Unit
 */
createUnit(
  units: Record<string, string | UnitDefinition | Unit>,
  options?: CreateUnitOptions
): Unit
```

### `fraction`

```typescript { .api }
/**
 * Create an exact rational Fraction from a value.
 * @param value - A number, string, BigNumber, bigint, Unit, Fraction,
 *                or FractionDefinition ({n, d})
 * @returns A Fraction
 */
fraction(
  value: number | string | BigNumber | bigint | Unit | Fraction | FractionDefinition
): Fraction
fraction(values: MathCollection): MathCollection

/**
 * @param numerator - Numerator
 * @param denominator - Denominator
 * @returns A Fraction
 */
fraction(numerator: number, denominator: number): Fraction
fraction(numerator: bigint, denominator: bigint): Fraction
```

### `index`

```typescript { .api }
/**
 * Create an Index from ranges. Used by subset(), Matrix.get(), Matrix.set().
 * @param ranges - Zero or more ranges or numbers defining each dimension
 * @returns An Index object
 */
index(...ranges: any[]): Index
```

### `matrix`

```typescript { .api }
/**
 * Create an empty Matrix with the specified storage format.
 * @param format - Storage format: 'dense' (default) or 'sparse'
 * @returns An empty Matrix
 */
matrix(format?: MatrixStorageFormat): Matrix

/**
 * Create a Matrix from a data array.
 * @param data - A multi-dimensional array or string array
 * @param format - Storage format: 'dense' or 'sparse'
 * @param dataType - Data type string for sparse matrices
 * @returns A Matrix
 */
matrix(
  data: MathCollection | string[],
  format?: MatrixStorageFormat,
  dataType?: string
): Matrix
matrix<T extends MathScalarType>(
  data: MathCollection<T>,
  format?: MatrixStorageFormat,
  dataType?: string
): Matrix<T>
```

### `number`

```typescript { .api }
/**
 * Create a number or convert a value to a JavaScript number.
 * @param value - Value to convert
 * @returns A JavaScript number
 */
number(
  value?: string | number | BigNumber | bigint | Fraction | boolean | Unit | null
): number
number(value?: MathCollection): number | MathCollection

/**
 * Convert a unit to a number expressed in the given valueless unit.
 * @param unit - A unit with a value (e.g., math.unit('5 m'))
 * @param valuelessUnit - The target unit for conversion (e.g., 'cm')
 * @returns A number
 */
number(unit: Unit, valuelessUnit: Unit | string): number
```

### `numeric`

```typescript { .api }
/**
 * Convert a numeric value to a specific output type.
 * @param value - The value to convert
 * @param outputType - The desired output type
 * @returns The converted value
 */
numeric(
  value: string | number | BigNumber | bigint | Fraction,
  outputType: 'number'
): number
numeric(
  value: string | number | BigNumber | bigint | Fraction,
  outputType: 'BigNumber'
): BigNumber
numeric(
  value: string | number | BigNumber | bigint | Fraction,
  outputType: 'bigint'
): bigint
numeric(
  value: string | number | BigNumber | bigint | Fraction,
  outputType: 'Fraction'
): Fraction
```

### `sparse`

```typescript { .api }
/**
 * Create a sparse Matrix from a two-dimensional array.
 * @param data - A two-dimensional array (optional)
 * @param dataType - Data type string for the sparse matrix entries
 * @returns A sparse Matrix
 */
sparse(data?: MathCollection, dataType?: string): Matrix
```

### `splitUnit`

```typescript { .api }
/**
 * Split a unit into an array of parts whose sum equals the original unit.
 * @param unit - The unit to split
 * @param parts - Array of target unit strings or valueless Unit objects
 * @returns An array of Unit objects
 */
splitUnit(unit: Unit, parts: Unit[]): Unit[]
```

**Example:**

```typescript { .api }
math.splitUnit(math.unit('1 m'), [math.unit('ft'), math.unit('in')])
// [3 ft, 3.37 in]
```

### `string`

```typescript { .api }
/**
 * Convert a value to a string. Elements of Arrays and Matrices are
 * processed element-wise.
 * @param value - A value to convert to a string
 * @returns String representation
 */
string(value: MathNumericType | string | Unit | null): string
string(value: MathCollection): MathCollection
```

### `unit`

```typescript { .api }
/**
 * Create a Unit from a string expression.
 * @param unit - A string like '5 m' or 'kg'
 * @returns A Unit
 */
unit(unit: string): Unit

/**
 * Create a Unit from an existing Unit.
 * @param unit - An existing Unit
 * @returns A Unit
 */
unit(unit: Unit): Unit

/**
 * Create a Unit from a numeric value and unit string.
 * @param value - The numeric value
 * @param unit - The unit string (optional)
 * @returns A Unit
 */
unit(value: MathNumericType, unit?: string): Unit
unit(value: MathCollection): Unit[]
```

---

## Arithmetic Functions

### `abs`

```typescript { .api }
/**
 * Calculate the absolute value of a number. For complex numbers, returns
 * the magnitude. For matrices, evaluated element-wise.
 * @param x - A number or matrix
 * @returns Absolute value of x
 */
abs(x: Complex): number
abs<T extends MathType>(x: T): T
```

### `add`

```typescript { .api }
/**
 * Add two or more values: x + y. For matrices, evaluated element-wise.
 * @param x - First value
 * @param y - Second value
 * @param values - Additional values (variadic)
 * @returns Sum of all values
 */
add<T extends MathType>(x: T, y: T): T
add<T extends MathType>(x: T, y: T, ...values: T[]): T
add(x: MathType, y: MathType): MathType
add(x: MathType, y: MathType, ...values: MathType[]): MathType
```

### `cbrt`

```typescript { .api }
/**
 * Calculate the cubic root of a value.
 * @param x - Value for which to calculate the cubic root
 * @param allRoots - If true, returns all three complex roots as a DenseMatrix. Default: false.
 * @returns The principal cubic root of x, or when allRoots=true a DenseMatrix of three
 *          Complex values. Use .toArray() to get a plain array of roots.
 */
cbrt(x: Complex, allRoots?: boolean): Complex
cbrt<T extends number | BigNumber | Unit>(x: T): T
```

### `ceil`

```typescript { .api }
/**
 * Round a value towards plus infinity (ceiling). For complex numbers, both
 * real and imaginary parts are rounded. Evaluated element-wise on matrices.
 * @param x - Number to be rounded
 * @param n - Number of decimal places. Default: 0.
 * @returns Rounded value
 */
ceil<T extends MathNumericType | MathCollection>(
  x: T,
  n?: number | BigNumber
): NoLiteralType<T>
ceil<U extends MathCollection>(x: MathNumericType, n: U): U
ceil<U extends MathCollection<Unit>>(x: U, unit: Unit): U
ceil(x: Unit, unit: Unit): Unit
ceil(x: Unit, n: number | BigNumber, unit: Unit): Unit
ceil<U extends MathCollection<Unit>>(x: U, n: number | BigNumber, unit: Unit): U
```

### `cube`

```typescript { .api }
/**
 * Compute the cube of a value: x * x * x.
 * Evaluated element-wise on matrices.
 * @param x - Number for which to calculate the cube
 * @returns x cubed
 */
cube<T extends MathNumericType | Unit>(x: T): T
```

### `divide`

```typescript { .api }
/**
 * Divide two values: x / y.
 * For matrices, computes x multiplied by the inverse of y.
 * @param x - Numerator
 * @param y - Denominator
 * @returns Quotient x / y
 */
divide(x: Unit, y: Unit): Unit | number
divide(x: Unit, y: number): Unit
divide(x: number, y: number): number
divide(x: MathType, y: MathType): MathType
```

### `dotDivide`

```typescript { .api }
/**
 * Divide two matrices or values element-wise: x ./ y.
 * Accepts both matrices and scalar values.
 * @param x - Numerator
 * @param y - Denominator
 * @returns Element-wise quotient
 */
dotDivide<T extends MathCollection>(x: T, y: MathType): T
dotDivide<T extends MathCollection>(x: MathType, y: T): T
dotDivide(x: Unit, y: MathType): Unit
dotDivide(x: MathType, y: Unit): Unit
dotDivide(x: MathNumericType, y: MathNumericType): MathNumericType
```

### `dotMultiply`

```typescript { .api }
/**
 * Multiply two matrices or values element-wise: x .* y.
 * Accepts both matrices and scalar values.
 * @param x - Left-hand value
 * @param y - Right-hand value
 * @returns Element-wise product
 */
dotMultiply<T extends MathCollection>(x: T, y: MathType): T
dotMultiply<T extends MathCollection>(x: MathType, y: T): T
dotMultiply(x: Unit, y: MathType): Unit
dotMultiply(x: MathType, y: Unit): Unit
dotMultiply(x: MathNumericType, y: MathNumericType): MathNumericType
```

### `dotPow`

```typescript { .api }
/**
 * Raise each element of x to the power y element-wise: x .^ y.
 * @param x - The base
 * @param y - The exponent
 * @returns Element-wise power
 */
dotPow<T extends MathType>(x: T, y: MathType): T
```

### `exp`

```typescript { .api }
/**
 * Calculate the natural exponential of a value: e^x.
 * Evaluated element-wise on matrices.
 * @param x - A number or matrix to exponentiate
 * @returns e raised to the power x
 */
exp<T extends number | BigNumber | Complex>(x: T): T
```

### `expm1`

```typescript { .api }
/**
 * Calculate e^x - 1. More accurate than exp(x) - 1 for values near x = 0.
 * Evaluated element-wise on matrices.
 * @param x - A number or matrix
 * @returns e^x - 1
 */
expm1<T extends number | BigNumber | Complex>(x: T): T
```

### `fix`

```typescript { .api }
/**
 * Round a value towards zero (truncate). Evaluated element-wise on matrices.
 * @param x - Number to be rounded
 * @param n - Number of decimal places. Default: 0.
 * @returns Truncated value
 */
fix<T extends MathNumericType | MathCollection>(
  x: T,
  n?: number | BigNumber
): NoLiteralType<T>
fix<U extends MathCollection>(x: MathNumericType, n: U): U
fix<U extends MathCollection<Unit>>(x: U, unit: Unit): U
fix(x: Unit, unit: Unit): Unit
fix(x: Unit, n: number | BigNumber, unit: Unit): Unit
fix<U extends MathCollection<Unit>>(x: U, n: number | BigNumber, unit: Unit): U
```

### `floor`

```typescript { .api }
/**
 * Round a value towards minus infinity (floor). Evaluated element-wise
 * on matrices.
 * @param x - Number to be rounded
 * @param n - Number of decimal places. Default: 0.
 * @returns Rounded-down value
 */
floor<T extends MathNumericType | MathCollection>(
  x: T,
  n?: number | BigNumber
): NoLiteralType<T>
floor<U extends MathCollection>(x: MathNumericType, n: U): U
floor<U extends MathCollection<Unit>>(x: U, unit: Unit): U
floor(x: Unit, unit: Unit): Unit
floor(x: Unit, n: number | BigNumber, unit: Unit): Unit
floor<U extends MathCollection<Unit>>(x: U, n: number | BigNumber, unit: Unit): U
```

### `gcd`

```typescript { .api }
/**
 * Calculate the greatest common divisor for two or more integer values.
 * Evaluated element-wise on matrices. Variadic.
 * @param args - Two or more integer numbers
 * @returns Greatest common divisor
 */
gcd<T extends number | BigNumber | Fraction | MathCollection>(...args: T[]): T
gcd<T extends number | BigNumber | Fraction | Matrix>(args: T[]): T
```

### `hypot`

```typescript { .api }
/**
 * Calculate the hypotenuse of a list of values:
 * hypot(a, b, c, ...) = sqrt(a^2 + b^2 + c^2 + ...)
 * Matrix and Array input is flattened to a single number.
 * @param args - Numeric values or an array/matrix of values
 * @returns The hypotenuse
 */
hypot<T extends number | BigNumber>(...args: T[]): T
hypot<T extends number | BigNumber>(args: T[]): T
```

### `lcm`

```typescript { .api }
/**
 * Calculate the least common multiple for two values or arrays.
 * lcm(a, b) = abs(a * b) / gcd(a, b)
 * Evaluated element-wise on matrices.
 * @param a - An integer number
 * @param b - An integer number
 * @returns Least common multiple
 */
lcm<T extends number | BigNumber | MathCollection>(a: T, b: T): T
```

### `log`

```typescript { .api }
/**
 * Calculate the logarithm of a value.
 * With no base, calculates the natural logarithm (base e).
 * @param x - Value for which to calculate the logarithm
 * @param base - Optional logarithm base. Defaults to e.
 * @returns Logarithm of x
 */
log<T extends number | BigNumber | Complex>(
  x: T,
  base?: number | BigNumber | Complex
): NoLiteralType<T>
```

### `log10`

```typescript { .api }
/**
 * Calculate the base-10 logarithm of a value.
 * Equivalent to log(x, 10). Evaluated element-wise on matrices.
 * @param x - Value for which to calculate the logarithm
 * @returns Base-10 logarithm of x
 */
log10<T extends number | BigNumber | Complex | MathCollection>(x: T): T
```

### `log1p`

```typescript { .api }
/**
 * Calculate the natural logarithm of (x + 1).
 * More accurate than log(x + 1) for values near x = 0.
 * Evaluated element-wise on matrices.
 * @param x - Value for which to calculate log(x + 1)
 * @param base - Optional base for the logarithm
 * @returns log(1 + x)
 */
log1p<T extends number | BigNumber | Complex | MathCollection>(
  x: T,
  base?: number | BigNumber | Complex
): T
```

### `log2`

```typescript { .api }
/**
 * Calculate the base-2 logarithm of a value.
 * Equivalent to log(x, 2). Evaluated element-wise on matrices.
 * @param x - Value for which to calculate the logarithm
 * @returns Base-2 logarithm of x
 */
log2<T extends number | BigNumber | Complex | MathCollection>(x: T): T
```

### `mod`

```typescript { .api }
/**
 * Calculate the modulus (remainder of integer division), defined as:
 * x - y * floor(x / y)
 * Evaluated element-wise on matrices.
 * @param x - Dividend
 * @param y - Divisor
 * @returns Remainder of x divided by y
 */
mod<T extends number | BigNumber | bigint | Fraction | MathCollection>(
  x: T,
  y: number | BigNumber | bigint | Fraction | MathCollection
): NoLiteralType<T>
```

### `multiply`

```typescript { .api }
/**
 * Multiply two or more values. For matrices, computes the matrix product.
 * The result is squeezed. Variadic.
 * @param x - First value
 * @param y - Second value
 * @param values - Additional values (variadic)
 * @returns Product of x and y
 */
multiply<T extends Matrix>(x: T, y: MathType): Matrix
multiply<T extends Matrix>(x: MathType, y: T): Matrix
multiply<T extends MathArray>(x: T, y: T[]): T
multiply<T extends MathArray>(x: T[], y: T): T
multiply<T extends MathArray>(x: T[], y: T[]): T[]
multiply<T extends MathArray>(x: T, y: T): MathScalarType
multiply(x: Unit, y: Unit): Unit
multiply(x: number, y: number): number
multiply(x: MathType, y: MathType, ...values: MathType[]): MathType
multiply<T extends MathType>(x: T, y: T, ...values: T[]): T
```

### `norm`

```typescript { .api }
/**
 * Calculate the norm of a number, vector, or matrix.
 * @param x - Value for which to calculate the norm
 * @param p - The p-norm to compute. Supported: any number, Infinity, -Infinity,
 *            'inf', '-inf', 'fro' (Frobenius). Default: 2.
 * @returns The p-norm
 */
norm(
  x: number | BigNumber | Complex | MathCollection,
  p?: number | BigNumber | string
): number | BigNumber
```

### `nthRoot`

```typescript { .api }
/**
 * Calculate the principal nth root of a value.
 * Solves: x^root = a for the positive real solution.
 * Evaluated element-wise on matrices.
 * @param a - Value for which to calculate the nth root
 * @param root - The root degree. Default: 2.
 * @returns The nth root of a
 */
nthRoot(
  a: number | BigNumber | Complex,
  root?: number | BigNumber
): number | Complex
nthRoot(M: MathCollection, root?: number | BigNumber): MathCollection
```

### `nthRoots`

```typescript { .api }
/**
 * Calculate all n complex nth roots of a value.
 * @param a - Value for which to calculate all nth roots
 * @param n - The root degree. Default: 2.
 * @returns An array of all n complex roots
 */
nthRoots(a: number | BigNumber | Complex, n?: number): Array<Complex>
```

**Example:**

```typescript { .api }
math.nthRoots(1, 3)
// [Complex(1, 0), Complex(-0.5, 0.866), Complex(-0.5, -0.866)]
```

### `pow`

```typescript { .api }
/**
 * Calculate x raised to the power y: x ^ y.
 * Supports matrix exponentiation for square matrices with positive integer y.
 * @param x - The base
 * @param y - The exponent
 * @returns x to the power y
 */
pow(x: MathType, y: number | BigNumber | bigint | Complex): MathType
```

### `round`

```typescript { .api }
/**
 * Round a value to the nearest integer or to n decimal places.
 * Evaluated element-wise on matrices.
 * @param x - Number to be rounded
 * @param n - Number of decimal places. Default: 0.
 * @returns Rounded value
 */
round<T extends MathNumericType | MathCollection>(
  x: T,
  n?: number | BigNumber
): NoLiteralType<T>
round<U extends MathCollection>(x: MathNumericType, n: U): U
round<U extends MathCollection<Unit>>(x: U, unit: Unit): U
round(x: Unit, unit: Unit): Unit
round(x: Unit, n: number | BigNumber, unit: Unit): Unit
round<U extends MathCollection<Unit>>(x: U, n: number | BigNumber, unit: Unit): U
```

### `sign`

```typescript { .api }
/**
 * Compute the sign of a value.
 * Returns: 1 when x > 0, -1 when x < 0, 0 when x == 0.
 * Evaluated element-wise on matrices.
 * @param x - The number for which to determine the sign
 * @returns Sign of x: -1, 0, or 1
 */
sign<T extends MathType>(x: T): T
```

### `sqrt`

```typescript { .api }
/**
 * Calculate the square root of a value.
 * Returns a Complex number for negative inputs when predictable=false.
 * For matrix square roots, use sqrtm. For element-wise, use map(M, sqrt).
 * @param x - Value for which to calculate the square root
 * @returns Square root of x
 */
sqrt(x: number): number | Complex
sqrt<T extends BigNumber | Complex | Unit>(x: T): T
```

### `square`

```typescript { .api }
/**
 * Compute the square of a value: x * x.
 * Evaluated element-wise on matrices.
 * @param x - Number for which to calculate the square
 * @returns x squared
 */
square<T extends MathNumericType | Unit>(x: T): T
```

### `subtract`

```typescript { .api }
/**
 * Subtract two values: x - y.
 * Evaluated element-wise on matrices.
 * @param x - Initial value
 * @param y - Value to subtract from x
 * @returns Difference x - y
 */
subtract<T extends MathType>(x: T, y: T): T
subtract(x: MathType, y: MathType): MathType
```

### `unaryMinus`

```typescript { .api }
/**
 * Invert the sign of a value: -x.
 * For complex numbers, both real and imaginary parts are negated.
 * For matrices, evaluated element-wise. Strings and booleans are
 * converted to number first.
 * @param x - Number to negate
 * @returns Value with inverted sign
 */
unaryMinus<T extends MathType>(x: T): T
```

### `unaryPlus`

```typescript { .api }
/**
 * Apply unary plus: +x.
 * Numeric values are returned as-is. Strings and booleans are converted
 * to a number. Evaluated element-wise on matrices.
 * @param x - Input value
 * @returns The numeric value of x
 */
unaryPlus<T extends string | MathType>(x: T): T
```

### `xgcd`

```typescript { .api }
/**
 * Calculate the extended greatest common divisor using the Extended
 * Euclidean algorithm.
 * @param a - An integer number
 * @param b - An integer number
 * @returns A DenseMatrix [div, m, n] where div = gcd(a, b) and a*m + b*n = div.
 *          Use .toArray() to get a plain array; e.g. xgcd(a, b).toArray()[0] for the gcd.
 */
xgcd(a: number | BigNumber, b: number | BigNumber): MathArray
```

**Example:**

```typescript { .api }
math.xgcd(8, 12)
// DenseMatrix [4, -1, 1]  because gcd(8,12)=4 and 8*(-1) + 12*(1) = 4
// Use .toArray() to get a plain array: math.xgcd(8, 12).toArray() => [4, -1, 1]
```

### `invmod`

```typescript { .api }
/**
 * Calculate the modular multiplicative inverse of a modulo b.
 * Finds integer x such that: a * x ≡ 1 (mod b).
 * Returns NaN when no inverse exists (i.e., gcd(a, b) != 1).
 * @param a - An integer number
 * @param b - An integer number (the modulus, must be non-zero)
 * @returns An integer x where invmod(a,b)*a ≡ 1 (mod b), or NaN if no inverse exists
 */
invmod(a: number | BigNumber, b: number | BigNumber): number | BigNumber
```

**Example:**

```typescript { .api }
math.invmod(7, 13)    // returns 2 (because 7*2 = 14 ≡ 1 mod 13)
math.invmod(8, 12)    // returns NaN (gcd(8,12) = 4 != 1)
```
