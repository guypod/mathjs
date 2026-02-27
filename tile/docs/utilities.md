# mathjs Utilities and Comparison

mathjs v15.1.0 utilities, comparison functions, set operations, string formatting, unit conversion, type-check predicates, serialization helpers, and error classes.

---

## Section 1 — Comparison / Relational Functions

All numeric comparisons respect the configured `relTol` and `absTol` tolerances. Comparisons on collections are evaluated element-wise.

```typescript { .api }
// Compare two values numerically.
// Returns 1 when x > y, -1 when x < y, 0 when x == y.
// Uses relTol / absTol tolerances. Element-wise on collections.
function compare(
  x: MathType | string,
  y: MathType | string
): number | BigNumber | Fraction | MathCollection

// Compare two values of any type in a deterministic, natural order.
// Works the same as compare() for numeric types; for non-numeric types
// it falls back to a consistent natural ordering.
function compareNatural(x: any, y: any): number

// Compare two strings lexically (case-sensitive).
// Returns 1 when x > y, -1 when x < y, 0 when x == y.
// Element-wise on collections.
function compareText(
  x: string | MathCollection,
  y: string | MathCollection
): number | MathCollection

// Test element-wise whether two matrices are equal.
// Returns true when the inputs have the same size and every element is equal.
function deepEqual(x: MathType, y: MathType): MathType

// Test whether two values are equal using configured tolerances.
// null is only equal to null; undefined is only equal to undefined.
// Element-wise on collections.
function equal(
  x: MathType | string,
  y: MathType | string
): boolean | MathCollection

// Check string equality (case-sensitive). Returns true/false for scalar inputs.
// Element-wise on collections (returns a collection of booleans).
// Note: unlike compareText() which returns -1/0/1, equalText() returns a boolean.
function equalText(
  x: string | MathCollection,
  y: string | MathCollection
): boolean | MathCollection

// Test whether x > y (with tolerance). Element-wise on collections.
function larger(
  x: MathType | string,
  y: MathType | string
): boolean | MathCollection

// Test whether x >= y (with tolerance). Element-wise on collections.
function largerEq(
  x: MathType | string,
  y: MathType | string
): boolean | MathCollection

// Test whether x < y (with tolerance). Element-wise on collections.
function smaller(
  x: MathType | string,
  y: MathType | string
): boolean | MathCollection

// Test whether x <= y (with tolerance). Element-wise on collections.
function smallerEq(
  x: MathType | string,
  y: MathType | string
): boolean | MathCollection

// Test whether x != y (with tolerance). Element-wise on collections.
function unequal(
  x: MathType | string,
  y: MathType | string
): boolean | MathCollection
```

---

## Section 2 — Set Functions

Set functions treat arrays and matrices as multisets. Multi-dimensional inputs are flattened before the operation.

```typescript { .api }
// Cartesian product of two multisets.
function setCartesian<T extends MathCollection>(a1: T, a2: MathCollection): T

// Set difference: elements in a1 that are not in a2.
function setDifference<T extends MathCollection>(a1: T, a2: MathCollection): T

// Collect the distinct (unique) elements of a multiset.
function setDistinct<T extends MathCollection>(a: T): T

// Intersection of two multisets.
function setIntersect<T extends MathCollection>(a1: T, a2: MathCollection): T

// Test whether a1 is a subset of a2.
function setIsSubset(a1: MathCollection, a2: MathCollection): boolean

// Count occurrences of element e in multiset a.
function setMultiplicity(e: MathNumericType, a: MathCollection): number

// Power set — all possible subsets of a multiset.
function setPowerset<T extends MathCollection>(a: T): T

// Count the total number of elements in a multiset.
function setSize(a: MathCollection): number

// Symmetric difference of two multisets.
function setSymDifference<T extends MathCollection>(a1: T, a2: MathCollection): T

// Union of two multisets.
function setUnion<T extends MathCollection>(a1: T, a2: MathCollection): T
```

---

## Section 3 — Signal Processing

```typescript { .api }
// N-dimensional Fast Fourier Transform.
// Input arr must contain complex or real numeric values.
// Returns a collection of the same type as the input.
function fft<T extends MathCollection>(arr: T): T

// N-dimensional Inverse Fast Fourier Transform.
// Returns a collection of the same type as the input.
function ifft<T extends MathCollection>(arr: T): T

// Compute the transfer function of a zero-pole-gain model.
// Returns the transfer function as an array of numerator and denominator coefficients.
function zpk2tf<T extends MathCollection>(z: T, p: T, k?: number): T

// Calculate the frequency response of a filter given its numerator and denominator coefficients.
// w is an optional range of frequencies (number of points or array of frequencies).
// Returns { w: T, h: T } where w is the frequency vector and h is the complex frequency response.
function freqz<T extends MathCollection>(b: T, a: T, w?: number | T): { w: T; h: T }
```

---

## Section 4 — String Functions

```typescript { .api }
// Format a value of any type into a string.
// options  — a FormatOptions object, a precision number, a BigNumber precision,
//            or a custom format function applied to every numeric leaf.
// callback — override the built-in numeric notation for every numeric element
//            (e.g. all matrix elements, real/imaginary parts of Complex).
function format(
  value: any,
  options?: FormatOptions | number | BigNumber | ((item: any) => string),
  callback?: (value: any) => string
): string

// Interpolate values into a string template.
// Template placeholders use the syntax $varname (no braces, no spaces) where varname
// is a key in values. Example: print('x = $x', { x: 3 }) → 'x = 3'.
// Using ${ x } (with braces/spaces) does NOT substitute the variable.
// precision — number of significant digits; omit to leave values unrounded.
// options   — additional FormatOptions or a precision number.
function print(
  template: string,
  values: any,
  precision?: number,
  options?: number | object
): void

// FormatOptions — controls numeric output of format() and related functions
interface FormatOptions {
  // Number notation style. Default: 'auto'
  // 'fixed'       — e.g. '123.40', '14000000'
  // 'exponential' — e.g. '1.234e+2'
  // 'engineering' — exponent is always a multiple of 3
  // 'auto'        — fixed when |exp| is between lowerExp and upperExp, else exponential
  // 'hex' / 'bin' / 'oct' — integer bases; optionally padded to wordSize bits
  notation?: 'fixed' | 'exponential' | 'engineering' | 'auto' | 'hex' | 'bin' | 'oct'

  // Significant digits (auto/exponential) or decimal places (fixed).
  precision?: number | BigNumber

  // Exponent threshold below which auto mode switches to exponential. Default: -3
  lowerExp?: number | BigNumber

  // Exponent threshold above which auto mode switches to exponential. Default: 5
  upperExp?: number | BigNumber

  // Fraction display style: 'ratio' (default, e.g. '1/3') or 'decimal' (e.g. '0.(3)')
  fraction?: string

  // Word size in bits for bin/oct/hex notation. Appended as a size suffix.
  wordSize?: number | BigNumber
}

// Format a number as a binary string (prefix '0b').
// wordSize — optional bit width for signed two's-complement representation.
//            When provided, appends an 'i{wordSize}' suffix (e.g. bin(10, 8) → '0b1010i8').
//            The value must fit within the signed range for the given word size.
function bin(value: number | BigNumber, wordSize?: number | BigNumber): string

// Format a number as an octal string (prefix '0o').
// wordSize — optional bit width for signed two's-complement representation.
//            When provided, appends an 'i{wordSize}' suffix (e.g. oct(8, 8) → '0o10i8').
function oct(value: number | BigNumber, wordSize?: number | BigNumber): string

// Format a number as a hexadecimal string (prefix '0x').
// wordSize — optional bit width for signed two's-complement representation.
//            When provided, appends an 'i{wordSize}' suffix (e.g. hex(127, 8) → '0x7fi8').
//            The value must be within the signed range: e.g. for wordSize=8 the max is 127.
//            hex(255, 8) throws because 255 exceeds the maximum signed 8-bit value.
function hex(value: number | BigNumber, wordSize?: number | BigNumber): string
```

**Examples:**

```typescript { .api }
math.bin(2)         // '0b10'
math.oct(56)        // '0o70'
math.hex(240)       // '0xf0'
math.hex(127, 8)    // '0x7fi8'  (8-bit signed; note the i8 suffix)
math.bin(10, 8)     // '0b1010i8'
math.oct(8, 8)      // '0o10i8'
math.hex(-1, 8)     // '0xffi8'
```

---

## Section 5 — Geometry Functions

```typescript { .api }
// Euclidean distance between two points in 2D or 3D space, or distance from a point to a line.
// x  — coordinates of the first point (or point)
// y  — coordinates of the second point, OR line coefficients (3D), OR first end-point of line (2D)
// z  — (optional) second end-point of the line when computing point-to-line distance in 2D
function distance(
  x: MathCollection | object,
  y: MathCollection | object,
  z?: MathCollection | object
): number | BigNumber

// Point of intersection of two lines in 2D/3D, or of a line and a plane in 3D.
// Returns null when the lines do not meet.
// Note: fill plane coefficients as x + y + z = c (not x + y + z + c = 0).
function intersect(
  w: MathCollection,   // first endpoint of line 1
  x: MathCollection,   // second endpoint of line 1
  y: MathCollection,   // first endpoint of line 2 OR plane coefficients
  z?: MathCollection   // second endpoint of line 2 (omit for line-plane intersection)
): MathArray
```

---

## Section 6 — Unit Conversion Functions

```typescript { .api }
// Convert a unit or collection of units to a different unit.
// unit can be a string like 'cm' or a valueless Unit object.
// Element-wise on collections.
function to(x: Unit | MathCollection, unit: Unit | string): Unit | MathCollection

// Convert a unit to the most appropriate display unit.
// When no preferred units are given, the best SI prefix is chosen automatically.
// When preferred units are provided, converts to the unit giving a value closest to 1.
function toBest(): Unit
function toBest(units: string[] | Unit[], options: object): Unit
```

---

## Section 7 — Type-Check / Utility Functions

### Numeric test functions

```typescript { .api }
// Deep-clone any value, including matrices and mathjs objects.
function clone<TType>(x: TType): TType

// Returns true if x is a number, BigNumber, bigint, Fraction, boolean,
// or a string containing a numeric value.
function hasNumericValue(x: any): boolean | boolean[]

// Returns true if x is a bounded mathematical entity.
function isBounded(x: MathType): boolean

// Returns true if x is finite. Element-wise on collections.
function isFinite(x: MathScalarType): boolean
function isFinite(A: MathCollection): MathCollection

// Returns true if x is an integer value (works for number, BigNumber, Fraction).
// Element-wise on collections.
function isInteger(x: number | BigNumber | Fraction | MathCollection): boolean

// Returns true if x is NaN.
// Supports number, BigNumber, bigint, Fraction, Unit. Element-wise on collections.
function isNaN(x: number | BigNumber | bigint | Fraction | MathCollection | Unit): boolean

// Returns true if x < 0.
// Supports number, BigNumber, bigint, Fraction, Unit. Element-wise on collections.
function isNegative(x: number | BigNumber | bigint | Fraction | MathCollection | Unit): boolean

// Returns true if x is a numeric type: number, BigNumber, bigint, Fraction, or boolean.
// Acts as a type guard narrowing to those types.
function isNumeric(x: any): x is number | BigNumber | bigint | Fraction | boolean

// Returns true if x > 0.
// Supports number, BigNumber, bigint, Fraction, Unit. Element-wise on collections.
function isPositive(x: number | BigNumber | bigint | Fraction | MathCollection | Unit): boolean

// Returns true if x is a prime number (supports number, BigNumber).
// Element-wise on collections.
function isPrime(x: number | BigNumber | MathCollection): boolean

// Returns true if x equals zero.
// Supports number, BigNumber, Fraction, Complex, Unit. Element-wise on collections.
function isZero(x: MathType): boolean

// Determine the type name of any value as a string.
// Primitive types are lower-case (e.g. 'number', 'string').
// Non-primitive types are upper-camel-case (e.g. 'Array', 'Matrix', 'BigNumber').
function typeOf(x: any): string

// Convert a numeric value to a specific numeric output type.
function numeric(x: string | number | BigNumber | bigint | Fraction, outputType: 'number'): number
function numeric(x: string | number | BigNumber | bigint | Fraction, outputType: 'BigNumber'): BigNumber
function numeric(x: string | number | BigNumber | bigint | Fraction, outputType: 'bigint'): bigint
function numeric(x: string | number | BigNumber | bigint | Fraction, outputType: 'Fraction'): Fraction
```

### Primitive and collection type guards

```typescript { .api }
function isNumber(x: unknown): x is number
function isBigNumber(x: unknown): x is BigNumber
function isBigInt(x: unknown): x is bigint
function isComplex(x: unknown): x is Complex
function isFraction(x: unknown): x is Fraction
function isUnit(x: unknown): x is Unit
function isString(x: unknown): x is string
const isArray: ArrayConstructor['isArray']   // delegates to Array.isArray
function isMatrix(x: unknown): x is Matrix
function isCollection(x: unknown): x is Matrix | any[]
function isDenseMatrix(x: unknown): x is Matrix
function isSparseMatrix(x: unknown): x is Matrix
function isRange(x: unknown): boolean
function isIndex(x: unknown): x is Index
function isBoolean(x: unknown): x is boolean
function isResultSet(x: unknown): x is ResultSet
function isHelp(x: unknown): boolean
function isChain(x: unknown): x is MathJsChain<unknown>
function isNull(x: unknown): x is null
function isUndefined(x: unknown): x is undefined
function isInteger(x: unknown): boolean
// JavaScript built-in type guards
function isFunction(x: unknown): boolean
function isDate(x: unknown): boolean
function isRegExp(x: unknown): boolean
function isObject(x: unknown): boolean
function isMap(x: unknown): boolean
function isPartitionedMap(x: unknown): boolean
function isObjectWrappingMap(x: unknown): boolean
```

### AST node type guards

```typescript { .api }
function isNode(x: unknown): x is MathNode
function isAccessorNode(x: unknown): x is AccessorNode
function isArrayNode(x: unknown): x is ArrayNode
function isAssignmentNode(x: unknown): x is AssignmentNode
function isBlockNode(x: unknown): x is BlockNode
function isConditionalNode(x: unknown): x is ConditionalNode
function isConstantNode(x: unknown): x is ConstantNode
function isFunctionAssignmentNode(x: unknown): x is FunctionAssignmentNode
function isFunctionNode(x: unknown): x is FunctionNode
function isIndexNode(x: unknown): x is IndexNode
function isObjectNode(x: unknown): x is ObjectNode
function isOperatorNode(x: unknown): x is OperatorNode<OperatorNodeOp, OperatorNodeFn>
function isParenthesisNode(x: unknown): x is ParenthesisNode
function isRangeNode(x: unknown): x is RangeNode
function isRelationalNode(x: unknown): x is RelationalNode
function isSymbolNode(x: unknown): x is SymbolNode
```

---

## Section 8 — Serialization

mathjs objects (BigNumber, Complex, Fraction, Matrix, Unit, ResultSet, etc.) can be round-tripped through JSON using the `reviver` and `replacer` helpers.

```typescript { .api }
// Returns a reviver function for JSON.parse() that restores mathjs objects
// from their plain JSON representation.
// The mathjs?.type field in the JSON drives dispatch to the correct constructor.
function reviver(): (key: string, value: any) => any

// Returns a replacer function for JSON.stringify() that serializes mathjs
// objects to plain JSON-compatible values.
function replacer(): (key: string, value: any) => any

// Usage pattern — pass as function REFERENCES (do NOT call them with parentheses):
//   const { replacer, reviver } = math
//   const json = JSON.stringify(value, replacer)   // correct: reference, not replacer()
//   const restored = JSON.parse(json, reviver)      // correct: reference, not reviver()
//
// Calling math.replacer() or math.reviver() returns undefined, which silently breaks
// serialization. Always destructure or reference the function directly.

// On-wire JSON format for Unit values (and other mathjs objects that
// support toJSON / fromJSON).
interface MathJSON {
  mathjs?: string    // type identifier used by reviver() for dispatch
  value: number      // the numeric magnitude
  unit: string       // unit string, e.g. 'kg m / s^2'
  fixPrefix?: boolean // whether the SI prefix is fixed (not auto-scaled)
}
```

---

## Section 9 — Error Classes

All error classes are exported from the `mathjs` package. `IndexError` and `DimensionError` extend `RangeError`; `ArgumentsError` extends `Error`.

```typescript { .api }
// Thrown when a matrix or array index is out of its valid bounds.
// Extends RangeError.
class IndexError extends RangeError {
  constructor(
    index: number,       // The actual index that was used
    min?: number,        // Minimum valid index (inclusive). Default: 0
    max?: number         // Maximum valid index (exclusive)
  )

  index: number          // The out-of-range index value
  min: number            // Minimum allowed index
  max: number | undefined // Maximum allowed index (exclusive), if provided
  message: string        // e.g. 'Index out of range (5 > 4)'
  isIndexError: true
}

// Thrown when matrix dimensions are incompatible for an operation.
// Extends RangeError.
class DimensionError extends RangeError {
  constructor(
    actual: number | number[],    // The actual size or dimension vector
    expected: number | number[],  // The expected size or dimension vector
    relation?: string             // Relational operator in the message. Default: '!='
  )

  actual: number | number[]       // The actual dimension(s)
  expected: number | number[]     // The expected dimension(s)
  relation: string                // e.g. '!=', '<'
  message: string                 // e.g. 'Dimension mismatch (3 != 2)'
  isDimensionError: true
}

// Thrown when a function receives the wrong number of arguments.
// Extends Error.
class ArgumentsError extends Error {
  constructor(
    fn: string,          // Name of the function that threw
    count: number,       // The number of arguments actually provided
    min: number,         // Minimum required number of arguments
    max?: number         // Maximum allowed number of arguments
  )

  fn: string             // Function name
  count: number          // Actual argument count provided
  min: number            // Minimum required
  max: number | undefined // Maximum allowed, if bounded
  message: string        // e.g. 'Wrong number of arguments in function add (1 provided, 2-3 expected)'
  isArgumentsError: true
}
```

---

## Section 10 — Numeric Integration (ODE Solver)

### solveODE

Numerically solve an ordinary differential equation (ODE) initial-value problem using an adaptive Runge-Kutta method.

Two variable-step methods are available:
- `'RK23'` — Bogacki–Shampine method (3rd-order with 2nd-order error estimate)
- `'RK45'` — Dormand-Prince RK5(4)7M method (default)

```typescript { .api }
/**
 * Numerically solve an ODE initial-value problem: dy/dt = func(t, y), y(t0) = y0.
 * @param func    - Forcing function f(t, y). Receives the current independent variable
 *                  and state; must return a value of the same shape as y0.
 * @param tspan   - Two-element array [tStart, tEnd] (numbers or Units of the same type).
 * @param y0      - Initial state: a scalar, BigNumber, Unit, or flat array/Matrix.
 * @param options - Optional solver configuration.
 * @returns       Object { t, y } where t is the time array and y is the state array.
 *                When y0 is scalar: y has shape [n].
 *                When y0 is an array of size [m]: y has shape [n, m].
 */
function solveODE(
  func:    (t: number | BigNumber | Unit, y: any) => any,
  tspan:   [number | BigNumber | Unit, number | BigNumber | Unit],
  y0:      number | BigNumber | Unit | MathCollection,
  options?: {
    method?:    'RK23' | 'RK45'   // Default: 'RK45'
    tol?:       number             // Numeric tolerance. Default: 1e-3
    firstStep?: number             // Initial step size
    minStep?:   number             // Minimum step size
    maxStep?:   number             // Maximum step size
    minDelta?:  number             // Minimum ratio of step change. Default: 0.2
    maxDelta?:  number             // Maximum ratio of step change. Default: 5
    maxIter?:   number             // Maximum iterations. Default: 1e4
  }
): { t: number[], y: number[] | number[][] }
```

**Example:**

```typescript { .api }
import { solveODE } from 'mathjs'

// Solve dy/dt = y, y(0) = 1  =>  solution: y(t) = e^t
const result = solveODE(
  (t, y) => y,          // f(t, y)
  [0, 4],               // tspan
  1                     // y0
)
// result.t — array of time points
// result.y — array of corresponding y values (approx e^t)

// System of ODEs: y0' = -y1, y1' = y0  (harmonic oscillator)
const result2 = solveODE(
  (t, y) => [-y[1], y[0]],
  [0, 2 * Math.PI],
  [1, 0],
  { method: 'RK23', tol: 1e-6 }
)
```
