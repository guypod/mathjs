# mathjs Types and Classes

**Package:** `mathjs` v15.1.0
**Source:** `types/index.d.ts`

---

## Core Type Aliases

```typescript { .api }
// Converts literal types to their wider base types.
// number literals -> number, string literals -> string, boolean literals -> boolean
type NoLiteralType<T> = T extends number
  ? number
  : T extends string
    ? string
    : T extends boolean
      ? boolean
      : T

// Union of all numeric types supported by mathjs
type MathNumericType = number | BigNumber | bigint | Fraction | Complex

// All scalar types (numeric + unit)
type MathScalarType = MathNumericType | Unit

// Generic wrapper defaulting to MathNumericType
type MathGeneric<T extends MathScalarType = MathNumericType> = T

// Recursive array type: a flat array or nested array
type MathArray<T = MathGeneric> = T[] | Array<MathArray<T>>

// Either a MathArray or a Matrix
type MathCollection<T = MathGeneric> = MathArray<T> | Matrix<T>

// Any valid math value: scalar or collection
type MathType = MathScalarType | MathCollection

// Anything that can be parsed as an expression
type MathExpression = string | string[] | MathCollection

// Storage format for Matrix
type MatrixStorageFormat = 'dense' | 'sparse'

// Callback used by matrixFromFunction; receives the index array and returns a value
type MatrixFromFunctionCallback<T extends MathScalarType> = (index: number[]) => T

// Factory function signature for dependency injection
type FactoryFunction<T> = (scope: MathScope) => T

// Scope for expression evaluation: plain object or MapLike
type MathScope<TValue = any> = Record<string, TValue> | MapLike<string, TValue>

// Every name that is a function on the MathJsInstance
type MathJsFunctionName = keyof MathJsInstance
```

---

## FactoryFunctionMap

Nested map of factory functions. All nested objects are flattened on `create()`.

```typescript { .api }
interface FactoryFunctionMap {
  [key: string]: FactoryFunction<any> | FactoryFunctionMap
}
```

---

## BigNumber class

Extends `Decimal` from the [`decimal.js`](https://mikemcl.github.io/decimal.js/) library. Provides arbitrary-precision decimal arithmetic. All methods and properties of `Decimal` are available.

```typescript { .api }
// BigNumber extends Decimal from decimal.js
interface BigNumber extends Decimal {}
```

**Constructor** (via `math.bignumber()`):

```typescript { .api }
// Create a BigNumber from a number, string, Fraction, BigNumber, bigint, Unit, boolean, or null
math.bignumber(
  x?: number | string | Fraction | BigNumber | bigint | Unit | boolean | null
): BigNumber

// Convert all elements of a matrix/array to BigNumber
math.bignumber<T extends MathCollection>(x: T): T
```

**Selected Decimal.js arithmetic methods** (inherited):

| Method | Signature | Description |
|--------|-----------|-------------|
| `abs` | `() => BigNumber` | Absolute value |
| `ceil` | `() => BigNumber` | Round toward +Infinity |
| `floor` | `() => BigNumber` | Round toward -Infinity |
| `round` | `() => BigNumber` | Round to nearest integer |
| `truncated` | `() => BigNumber` | Truncate toward zero |
| `plus` | `(n: Decimal.Value) => BigNumber` | Addition |
| `minus` | `(n: Decimal.Value) => BigNumber` | Subtraction |
| `times` | `(n: Decimal.Value) => BigNumber` | Multiplication |
| `dividedBy` | `(n: Decimal.Value) => BigNumber` | Division |
| `modulo` | `(n: Decimal.Value) => BigNumber` | Modulo |
| `pow` | `(n: Decimal.Value) => BigNumber` | Exponentiation |
| `sqrt` | `() => BigNumber` | Square root |
| `log` | `(base?: Decimal.Value) => BigNumber` | Base-10 logarithm when called with no argument (e.g. `bignumber(100).log()` → `2`); pass a base for arbitrary-base log |
| `ln` | `() => BigNumber` | Natural logarithm (base e); use instead of `log()` for ln |
| `exp` | `() => BigNumber` | e^x |
| `negated` | `() => BigNumber` | Unary negation |
| `isNaN` | `() => boolean` | Test for NaN |
| `isFinite` | `() => boolean` | Test for finiteness |
| `isZero` | `() => boolean` | Test for zero |
| `isPositive` | `() => boolean` | Test for positive |
| `isNegative` | `() => boolean` | Test for negative |
| `isInteger` | `() => boolean` | Test for integer |
| `equals` / `eq` | `(n: Decimal.Value) => boolean` | Equality |
| `greaterThan` / `gt` | `(n: Decimal.Value) => boolean` | Greater than |
| `greaterThanOrEqualTo` / `gte` | `(n: Decimal.Value) => boolean` | Greater than or equal |
| `lessThan` / `lt` | `(n: Decimal.Value) => boolean` | Less than |
| `lessThanOrEqualTo` / `lte` | `(n: Decimal.Value) => boolean` | Less than or equal |
| `comparedTo` / `cmp` | `(n: Decimal.Value) => number` | Comparison (-1, 0, 1) |
| `toFixed` | `(dp?: number) => string` | Fixed-point string |
| `toExponential` | `(dp?: number) => string` | Exponential string |
| `toPrecision` | `(sd?: number) => string` | Significant-digits string |
| `toNumber` | `() => number` | Convert to JS number |
| `toJSON` | `() => string` | JSON serialization |
| `toString` | `() => string` | String representation |
| `valueOf` | `() => string` | Primitive string value |

**Selected Decimal.js properties** (inherited):

| Property | Type | Description |
|----------|------|-------------|
| `d` | `number[]` | Coefficient digits array |
| `e` | `number` | Exponent |
| `s` | `number` | Sign: 1 or -1 |

---

## Complex class

Wraps the [`complex.js`](https://github.com/infusion/complex.js) library. Represents a complex number `a + bi`.

```typescript { .api }
interface Complex {
  // Instance properties
  re: number   // Real part
  im: number   // Imaginary part

  // Instance methods
  abs(): number
  arg(): number
  acos(): Complex
  acosh(): Complex
  acot(): Complex
  acoth(): Complex
  acsc(): Complex
  acsch(): Complex
  asec(): Complex
  asech(): Complex
  asin(): Complex
  asinh(): Complex
  atan(): Complex
  atanh(): Complex
  ceil(places?: number): Complex
  clone(): Complex
  conjugate(): Complex
  cos(): Complex
  cosh(): Complex
  cot(): Complex
  coth(): Complex
  csc(): Complex
  csch(): Complex
  div(other: Complex | number | string): Complex
  equals(other: Complex): boolean
  exp(): Complex
  floor(places?: number): Complex
  inverse(): Complex
  isNaN(): boolean
  isFinite(): boolean
  isZero(): boolean
  log(): Complex
  mod(other: Complex | number): Complex
  mul(other: Complex | number | string): Complex
  neg(): Complex
  pow(other: Complex | number): Complex
  round(places?: number): Complex
  sec(): Complex
  sech(): Complex
  sign(): Complex
  sin(): Complex
  sinh(): Complex
  sqrt(): Complex
  sub(other: Complex | number | string): Complex
  tan(): Complex
  tanh(): Complex
  toFixed(places?: number): string
  toJSON(): object
  toPolar(): PolarCoordinates
  toString(): string
  valueOf(): string

  // Methods available on the Complex interface (as defined in mathjs)
  format(precision?: number): string
  fromJSON(json: object): Complex
  fromPolar(polar: object): Complex
  fromPolar(r: number, phi: number): Complex
  compare(a: Complex, b: Complex): number
}
```

**Constructor** (via `math.complex()`):

```typescript { .api }
// From real and imaginary parts
math.complex(re: number, im: number): Complex

// From a string like '2 + 3i'
math.complex(arg: string): Complex

// From polar coordinates {r, phi}
math.complex(arg: PolarCoordinates): Complex

// From an existing Complex number (clone)
math.complex(arg: Complex): Complex

// No argument: returns 0 + 0i
math.complex(): Complex

// Convert all elements of a collection
math.complex(arg: MathCollection): MathCollection
```

**Static methods:**

```typescript { .api }
// Create a Complex from polar form {r, phi}
Complex.fromPolar(polar: { r: number; phi: number }): Complex
Complex.fromPolar(r: number, phi: number): Complex

// Parse a string into a Complex; returns null on failure
Complex.parseString(str: string): Complex | null
```

---

## Fraction class

Re-exported from [`fraction.js`](https://github.com/infusion/Fraction.js). Represents an exact rational number as a numerator/denominator pair.

```typescript { .api }
// Fraction is re-exported from 'fraction.js'
export { Fraction } from 'fraction.js'
```

**Constructor** (via `math.fraction()`):

```typescript { .api }
// From a number, string, BigNumber, bigint, Unit, Fraction, or FractionDefinition
math.fraction(
  x: number | string | BigNumber | bigint | Unit | Fraction | FractionDefinition
): Fraction

// From separate numerator and denominator (number)
math.fraction(numerator: number, denominator: number): Fraction

// From separate numerator and denominator (bigint)
math.fraction(numerator: bigint, denominator: bigint): Fraction

// Convert all elements of a matrix/array
math.fraction(values: MathCollection): MathCollection
```

**Key instance properties and methods** (from fraction.js):

| Member | Type / Signature | Description |
|--------|-----------------|-------------|
| `n` | `bigint` | Numerator (always non-negative; compare with `===` using bigint literals, e.g. `1n`) |
| `d` | `bigint` | Denominator (always positive; wrap with `Number()` to use as a JS number) |
| `s` | `bigint` | Sign: 1n or -1n |
| `abs()` | `() => Fraction` | Absolute value |
| `add(n)` | `(Fraction \| number \| string) => Fraction` | Addition |
| `sub(n)` | `(Fraction \| number \| string) => Fraction` | Subtraction |
| `mul(n)` | `(Fraction \| number \| string) => Fraction` | Multiplication |
| `div(n)` | `(Fraction \| number \| string) => Fraction` | Division |
| `pow(n)` | `(number) => Fraction` | Exponentiation |
| `mod(n)` | `(Fraction \| number \| string) => Fraction` | Modulo |
| `gcd(n)` | `(Fraction \| number \| string) => Fraction` | Greatest common divisor |
| `lcm(n)` | `(Fraction \| number \| string) => Fraction` | Least common multiple |
| `ceil(places?)` | `(number?) => Fraction` | Round toward +Infinity |
| `floor(places?)` | `(number?) => Fraction` | Round toward -Infinity |
| `round(places?)` | `(number?) => Fraction` | Round to nearest |
| `inverse()` | `() => Fraction` | Reciprocal |
| `neg()` | `() => Fraction` | Negation |
| `equals(n)` | `(Fraction \| number \| string) => boolean` | Equality |
| `compare(n)` | `(Fraction \| number \| string) => number` | Comparison (-1, 0, 1) |
| `simplify(eps?)` | `(number?) => Fraction` | Simplify within tolerance |
| `toFraction(bool?)` | `(boolean?) => string` | String like `'1/3'` |
| `toLatex(bool?)` | `(boolean?) => string` | LaTeX string |
| `toString()` | `() => string` | String representation |
| `valueOf()` | `() => number` | Decimal approximation |
| `toJSON()` | `() => { n: number; d: number; s: number }` | Serialize |

---

## FractionDefinition

Plain object that can be passed to `math.fraction()` to construct a `Fraction`.

```typescript { .api }
interface FractionDefinition {
  n: number  // Numerator
  d: number  // Denominator
}
```

---

## Matrix interface

Represents a mathjs matrix. Two concrete storage implementations exist: `DenseMatrix` and `SparseMatrix`.

```typescript { .api }
interface Matrix<T = MathGeneric> {
  // Storage type: 'DenseMatrix' or 'SparseMatrix'
  type: string

  // Returns the storage format string ('dense' or 'sparse')
  storage(): string

  // Returns the element data type string
  datatype(): string

  // Re-initialize the matrix from an array with an optional data type
  create(data: MathArray, datatype?: string): void

  // Returns the ratio of non-zero elements to total elements.
  // NOTE: density() is only available on SparseMatrix. Calling it on a DenseMatrix
  // throws TypeError: m.density is not a function.
  density(): number

  // Get or set a subset using an Index
  subset(index: Index, replacement?: any, defaultValue?: any): Matrix

  // Get the element at a given multi-dimensional index
  get(index: number[]): any

  // Set an element at a given multi-dimensional index
  set(index: number[], value: any, defaultValue?: number | string): Matrix

  // Resize the matrix to a new size, filling new entries with defaultValue
  resize(size: MathCollection, defaultValue?: number | string): Matrix

  // Reshape the matrix to new dimensions (does not copy data)
  reshape?(size: number[]): Matrix<T>

  // Deep clone the matrix
  clone(): Matrix<T>

  // Array of dimension sizes, e.g. [3, 4] for a 3x4 matrix
  size(): number[]

  // Map a callback over each element; skipZeros skips zero entries (sparse)
  map(
    callback: (a: any, b: number[], c: Matrix) => any,
    skipZeros?: boolean
  ): Matrix

  // Iterate over each element; skipZeros skips zero entries (sparse)
  forEach(
    callback: (a: any, b: number[], c: Matrix) => void,
    skipZeros?: boolean
  ): void

  // Convert to a nested JavaScript array
  toArray(): MathArray<T>

  // Same as toArray()
  valueOf(): MathArray<T>

  // Format the matrix as a string, optionally with FormatOptions
  format(
    options?: FormatOptions | number | BigNumber | ((value: any) => string)
  ): string

  // String representation
  toString(): string

  // Return a JSON-serializable representation
  toJSON(): any

  // Extract the k-th diagonal (k=0 is main diagonal, positive is above, negative below)
  diagonal(k?: number | BigNumber): any[]

  // Swap two rows in-place, returns the mutated matrix
  swapRows(i: number, j: number): Matrix<T>
}
```

### MatrixCtor

Constructor interface for the `Matrix` type.

```typescript { .api }
interface MatrixCtor {
  new (): Matrix
}
```

### MatrixStorageFormat

```typescript { .api }
type MatrixStorageFormat = 'dense' | 'sparse'
```

---

## Unit interface

Represents a value with a physical unit, e.g. `5 kg` or `9.8 m/s^2`.

```typescript { .api }
interface Unit {
  // Instance properties
  units: UnitComponent[]       // Array of unit components making up this unit
  dimensions: number[]         // Dimensional vector (SI base dimensions)
  value: number                // The numeric value
  fixPrefix: boolean           // Whether the prefix is fixed (not auto-adjusted)
  skipAutomaticSimplification: true  // Prevents automatic simplification

  // Convert to another unit; accepts unit string or Unit instance
  to(unit: string | Unit): Unit

  // Convert to the "best" human-readable unit
  toBest(): Unit
  toBest(units?: string[] | Unit[], options?: object): Unit

  // Extract the numeric value in the given unit
  toNumber(unit?: string): number

  // Extract the numeric value as number, Fraction, or BigNumber
  toNumeric(unit?: string): number | Fraction | BigNumber

  // Convert to SI base units
  toSI(): Unit

  // Serialize to MathJSON
  toJSON(): MathJSON

  // Format only the unit symbols (no numeric value)
  formatUnits(): string

  // Format the full unit expression with options
  format(options: FormatOptions): string

  // Simplify combined units to a single canonical unit
  simplify(): Unit

  // Split this unit into an array of units whose sum equals the original
  splitUnit(parts: ReadonlyArray<string | Unit>): Unit[]

  // Clone this unit
  clone(): Unit

  // Returns string representation of the value and unit
  valueOf(): string

  // String representation
  toString(): string

  // Check whether this unit has the given base dimension
  hasBase(base: BaseUnit | string | undefined): boolean

  // Check whether two units share the same base dimensions
  equalBase(unit: Unit): boolean

  // Full equality check (value and unit)
  equals(unit: Unit): boolean

  // Multiply two units
  multiply(unit: Unit): Unit

  // Divide two units; returns a dimensionless number if dimensions cancel
  divide(unit: Unit): Unit | number

  // Raise a unit to a power
  pow(unit: Unit): Unit

  // Absolute value of the numeric part
  abs(unit: Unit): Unit
}
```

### UnitSystemName

The name of a supported unit system. Used with `getUnitSystem()` and `setUnitSystem()`.

```typescript { .api }
type UnitSystemName = 'si' | 'cgs' | 'us' | 'auto'
```

### UnitStatic

Static methods and properties available on the `Unit` constructor.

```typescript { .api }
interface UnitStatic {
  PREFIXES: Record<string, UnitPrefix>
  BASE_DIMENSIONS: string[]
  BASE_UNITS: Record<string, BaseUnit>
  UNIT_SYSTEMS: Record<
    UnitSystemName,
    Record<string, { unit: Unit; prefix: UnitPrefix }>
  >
  UNITS: Record<string, Unit>

  // Parse a unit string into a Unit object
  parse(str: string): Unit

  // Returns true if the unit name has no associated value (valueless unit)
  isValuelessUnit(name: string): boolean

  // Deserialize a Unit from MathJSON
  fromJSON(json: MathJSON): Unit

  // Returns true if character c is valid in a unit name
  isValidAlpha(c: string): boolean

  // Create one or more new user-defined units
  createUnit(
    obj: Record<string, string | Unit | UnitDefinition>,
    options?: { override: boolean }
  ): Unit

  // Create a single new user-defined unit
  createUnitSingle(
    name: string,
    definition: string | Unit | UnitDefinition
  ): Unit

  // Get the current unit system name
  getUnitSystem(): UnitSystemName

  // Set the active unit system
  setUnitSystem(name: UnitSystemName): void
}
```

### UnitCtor

Constructor for Unit instances. Also extends `UnitStatic`.

```typescript { .api }
interface UnitCtor extends UnitStatic {
  new (
    value: number | BigNumber | Fraction | Complex | boolean,
    name: string
  ): Unit
}
```

### UnitDefinition

Describes how to define a new unit in terms of existing units.

```typescript { .api }
interface UnitDefinition {
  definition?: string | Unit  // Definition in terms of existing units, e.g. '0.514444 m/s'
  prefixes?: string           // Prefix set: 'none' | 'short' | 'long' | 'binary_short' | 'binary_long'
  offset?: number             // Additive offset for unit conversions (e.g. 273.15 for Celsius)
  aliases?: string[]          // Alternative names for this unit
  baseName?: string           // Name of the base dimension this unit introduces
}
```

### CreateUnitOptions

```typescript { .api }
interface CreateUnitOptions {
  prefixes?: 'none' | 'short' | 'long' | 'binary_short' | 'binary_long'
  aliases?: string[]
  offset?: number
  override?: boolean
}
```

### UnitComponent

One component in a compound unit expression.

```typescript { .api }
interface UnitComponent {
  power: number    // Exponent of this unit component
  prefix: string   // SI prefix string (e.g. 'k', 'm', 'M')
  unit: {
    name: string
    base: BaseUnit
    prefixes: Record<string, UnitPrefix>
    value: number
    offset: number
    dimensions: number[]
  }
}
```

### UnitPrefix

```typescript { .api }
interface UnitPrefix {
  name: string        // Prefix string, e.g. 'kilo'
  value: number       // Multiplier, e.g. 1000
  scientific: boolean // Whether this prefix is used in scientific notation
}
```

### BaseUnit

```typescript { .api }
interface BaseUnit {
  dimensions: number[]  // Dimensional exponents vector over SI base quantities
  key: string           // Identifier string for this base dimension
}
```

---

## Index interface

Represents an index used to select subsets of a `Matrix` or array. Created via `math.index(...ranges)`.

```typescript { .api }
// Opaque interface — Index objects are created with math.index()
interface Index {}
```

**Constructor** (via `math.index()`):

```typescript { .api }
// Create an Index from zero or more ranges, arrays, or numbers.
// Each argument corresponds to one dimension.
math.index(...ranges: any[]): Index
```

**Runtime methods** (available on Index instances at runtime):

| Method | Signature | Description |
|--------|-----------|-------------|
| `isScalar()` | `() => boolean` | Returns true if every dimension selects exactly one element |
| `size()` | `() => number[]` | Returns the size of the selection in each dimension |
| `max()` | `() => number[]` | Maximum index per dimension |
| `min()` | `() => number[]` | Minimum index per dimension |
| `toArray()` | `() => any[]` | Convert the index ranges to arrays |

---

## ResultSet interface

Returned by `math.evaluate()` when evaluating a block expression (multiple semicolon-separated statements). Each statement's result is stored in `entries`.

```typescript { .api }
interface ResultSet {
  entries: unknown[]   // Array of results, one per evaluated statement

  // Returns the entries array
  valueOf(): unknown[]

  // String representation
  toString(): string

  // Serialize to MathJSON format
  toJSON(): MathJSON
}
```

---

## MathJSON interface

The JSON serialization format used by mathjs objects (primarily `Unit` and `ResultSet`).

```typescript { .api }
interface MathJSON {
  mathjs?: string    // Type identifier used during deserialization (e.g. 'Unit', 'ResultSet')
  value: number      // Numeric value
  unit: string       // Unit string
  fixPrefix?: boolean // Whether the unit prefix is fixed
}
```

---

## PolarCoordinates interface

Represents a complex number in polar form.

```typescript { .api }
interface PolarCoordinates {
  r: number    // Radius (magnitude), always >= 0
  phi: number  // Angle (argument) in radians
}
```

---

## Decomposition result interfaces

### LUDecomposition

Result of `math.lup()` — LU decomposition with partial pivoting such that `A[p,:] = L * U`.

```typescript { .api }
interface LUDecomposition {
  L: MathCollection  // Lower triangular matrix
  U: MathCollection  // Upper triangular matrix
  p: number[]        // Row permutation vector
}
```

### SLUDecomposition

Result of `math.slu()` — Sparse LU decomposition with full pivoting such that `P * A * Q = L * U`. Extends `LUDecomposition`.

```typescript { .api }
interface SLUDecomposition extends LUDecomposition {
  L: MathCollection  // Lower triangular factor (inherited)
  U: MathCollection  // Upper triangular factor (inherited)
  p: number[]        // Row permutation vector (inherited)
  q: number[]        // Column permutation vector
}
```

### QRDecomposition

Result of `math.qr()` — QR decomposition where `Q` is orthogonal and `R` is upper triangular.

```typescript { .api }
interface QRDecomposition {
  Q: MathCollection  // Orthogonal matrix
  R: MathCollection  // Upper triangular matrix
}
```

### SchurDecomposition

Result of `math.schur()` — Schur decomposition where `A = U * T * U^*`, `U` is unitary and `T` is upper quasi-triangular.

```typescript { .api }
interface SchurDecomposition {
  U: MathCollection  // Unitary matrix
  T: MathCollection  // Upper quasi-triangular (Schur form) matrix
}
```

---

## Map-like interfaces

### MapLike

A minimal Map-compatible interface used as an alternative to plain objects for `MathScope`.

```typescript { .api }
interface MapLike<TKey = string, TValue = unknown> {
  get(key: TKey): TValue
  set(key: TKey, value: TValue): MapLike<TKey, TValue>
  has(key: TKey): boolean
  keys(): IterableIterator<TKey> | TKey[]
}
```

### PartitionedMap

A map partitioned into two sub-maps `a` and `b`. Used internally in scoping.

```typescript { .api }
interface PartitionedMap<T, U> {
  a: Map<T, U>
  b: Map<T, U>
}
```

### ObjectWrappingMap

A map backed by a plain JavaScript object. Used to wrap objects as map-like scopes.

```typescript { .api }
interface ObjectWrappingMap<T extends string | number | symbol, U> {
  wrappedObject: Record<T, U>
}
```

---

## MathJsFunctionName

Type alias that resolves to the name of every function defined on the `MathJsInstance`. Useful for typed dependency injection via `math.factory()`.

```typescript { .api }
// Union of every string key on MathJsInstance that corresponds to a callable function
type MathJsFunctionName = keyof MathJsInstance
```

**Example usage:**

```typescript { .api }
// factory() uses MathJsFunctionName to type-check dependency lists
math.factory(
  'myFunction',
  ['add', 'multiply'] satisfies MathJsFunctionName[],
  ({ add, multiply }) => (x: number, y: number) => add(multiply(x, x), multiply(y, y))
)
```
