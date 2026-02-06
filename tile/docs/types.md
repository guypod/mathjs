# Type Constructors and Conversions

Create and convert between different numeric types including arbitrary precision decimals (BigNumber), rational fractions, big integers, and standard JavaScript numbers. Also includes index and range constructors.

## Capabilities

### Numeric Type Constructors

Create specific numeric types for precision control.

```javascript { .api }
/**
 * Create an arbitrary precision decimal (BigNumber)
 * @param x - Value to convert (number, string, or BigNumber)
 * @returns BigNumber with configured precision
 */
function bignumber(x?: number | string | BigNumber): BigNumber

/**
 * Create a big integer (ES2020 bigint)
 * @param x - Value to convert
 * @returns ES2020 bigint
 */
function bigint(x?: number | string | bigint): bigint

/**
 * Create a rational fraction
 * @param value - Decimal value or string
 * @returns Fraction
 */
function fraction(value: number | string): Fraction
/**
 * Create a fraction from numerator and denominator
 * @param numerator - Numerator
 * @param denominator - Denominator
 * @returns Fraction
 */
function fraction(numerator: number, denominator: number): Fraction

/**
 * Convert to JavaScript number
 * @param value - Value to convert
 * @param valuelessUnit - Optional unit for valueless units
 * @returns JavaScript number
 */
function number(value: any, valuelessUnit?: Unit | string): number

/**
 * Convert to specific numeric type
 * @param value - Value to convert
 * @param outputType - Target type: 'number', 'BigNumber', 'bigint', or 'Fraction'
 *                     NOTE: 'Complex' is NOT supported as output type
 * @returns Converted value (number | BigNumber | bigint | Fraction)
 * @throws Error if outputType is 'Complex' or other unsupported type
 */
function numeric(value: any, outputType: 'number' | 'BigNumber' | 'bigint' | 'Fraction'): number | BigNumber | bigint | Fraction
```

**Usage Examples:**

```javascript
import { bignumber, bigint, fraction, number, config } from 'mathjs'

// BigNumber (arbitrary precision)
bignumber(0.1)                    // 0.1 (exact)
bignumber('1.23e+500')            // Very large number
bignumber(1).div(3)               // 0.333... (to configured precision)

// Configure BigNumber precision
config({ precision: 128 })
bignumber(1).div(3)               // 128 digits of precision

// Bigint (ES2020 big integers)
bigint(123)                       // 123n
bigint('9007199254740991')        // Beyond Number.MAX_SAFE_INTEGER

// Fraction (rational numbers)
fraction(0.25)                    // 1/4
fraction(1, 3)                    // 1/3 (exact)
fraction('0.333...')              // 1/3
fraction(0.1)                     // 1/10 (exact, unlike Number)

// Convert to number
number(fraction(1, 3))            // 0.333...
number(bignumber('123.456'))      // 123.456
number('50%')                     // 0.5
```

### Boolean and String Conversion

Type conversion utilities.

```javascript { .api }
/**
 * Convert to boolean
 * Supports: numbers (0 = false, non-zero = true), strings ('true' = true, 'false' = false)
 * @param x - Value to convert (number or specific strings like 'true'/'false')
 * @returns Boolean value
 * @throws Error if string cannot be converted (e.g., empty string '')
 */
function boolean(x: number | string | boolean): boolean

/**
 * Convert to string
 * @param value - Value to convert
 * @returns String representation
 */
function string(value: any): string
```

**Usage Examples:**

```javascript
import { boolean, string, complex, fraction } from 'mathjs'

// Boolean conversion
boolean(1)                        // true
boolean(0)                        // false
boolean('true')                   // true
boolean('false')                  // false
// boolean('') would throw error - empty strings not supported

// String conversion
string(123)                       // '123'
string(complex(2, 3))             // '2 + 3i'
string(fraction(1, 3))            // '1/3'
```

### Index and Range

Create index ranges for matrix operations.

```javascript { .api }
/**
 * Create a matrix index for subsetting
 * @param ranges - Index ranges (numbers, arrays, or range objects)
 * @returns Index object
 */
function index(...ranges: any[]): Index

/**
 * Create a numeric range
 * @param start - Start value
 * @param end - End value (inclusive if includeEnd is true)
 * @param step - Step size (default: 1)
 * @param includeEnd - Include end value (default: false)
 * @returns Range object
 */
function range(
  start: number,
  end: number,
  step?: number,
  includeEnd?: boolean
): Range
```

**Usage Examples:**

```javascript
import { matrix, index, range, subset } from 'mathjs'

const A = matrix([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

// Index for subsetting
const idx = index(range(0, 2), 1)
subset(A, idx)                    // [2, 5] (column 1, rows 0-1)

// Range examples
range(1, 5)                       // 1:5 (1, 2, 3, 4)
range(1, 5, 1, true)              // 1:5 (1, 2, 3, 4, 5) - inclusive
range(0, 10, 2)                   // 0:2:10 (0, 2, 4, 6, 8)
range(5, 1, -1)                   // 5:-1:1 (5, 4, 3, 2)

// Use in matrix operations
const B = matrix([[1, 2], [3, 4], [5, 6]])
subset(B, index(range(1, 3), 0))  // [3, 5] (column 0, rows 1-2)
```

## Type System Architecture

### Type Hierarchy

Math.js supports multiple numeric types with automatic conversion:

```javascript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex
type MathScalarType = MathNumericType | Unit
type MathArray<T> = T[] | Array<MathArray<T>>
type MathCollection<T> = MathArray<T> | Matrix<T>
type MathType = MathScalarType | MathCollection
type MathExpression = string | string[] | MathCollection
```

### Type Interfaces

```javascript { .api }
/**
 * BigNumber - Arbitrary precision decimal
 * From decimal.js library
 */
interface BigNumber {
  // Arithmetic methods
  plus(y: BigNumber | number | string): BigNumber
  minus(y: BigNumber | number | string): BigNumber
  times(y: BigNumber | number | string): BigNumber
  div(y: BigNumber | number | string): BigNumber
  pow(y: BigNumber | number | string): BigNumber
  sqrt(): BigNumber
  abs(): BigNumber
  // Comparison methods
  eq(y: BigNumber | number | string): boolean
  lt(y: BigNumber | number | string): boolean
  gt(y: BigNumber | number | string): boolean
  // Conversion
  toNumber(): number
  toString(): string
  toFixed(dp?: number): string
  toPrecision(sd?: number): string
}

/**
 * Fraction - Rational number
 */
interface Fraction {
  /** Numerator */
  n: number
  /** Denominator */
  d: number
}

/**
 * Complex - Complex number
 */
interface Complex {
  /** Real part */
  re: number
  /** Imaginary part */
  im: number
}

/**
 * Index - Matrix index/subscript
 */
interface Index {
  // Internal representation of index ranges
}

/**
 * Range - Numeric range
 */
interface Range {
  start: number
  end: number
  step: number
}
```

### Automatic Type Detection

Math.js automatically chooses appropriate types based on configuration:

```javascript
import { config, evaluate } from 'mathjs'

// Default: JavaScript numbers
config({ number: 'number' })
evaluate('1/3')                   // 0.333...

// Use BigNumber for all operations
config({ number: 'BigNumber', precision: 64 })
evaluate('1/3')                   // 0.333... (64 digits)

// Use Fraction for exact rational arithmetic
config({ number: 'Fraction' })
evaluate('1/3')                   // 1/3 (exact)
```

## Working with Different Types

### BigNumber (Arbitrary Precision)

Use BigNumber for calculations requiring high precision or very large/small numbers.

```javascript
import { bignumber, add, multiply, divide, config } from 'mathjs'

// Configure precision (default: 64)
config({ precision: 128 })

// Exact decimal arithmetic
const x = bignumber('0.1')
const y = bignumber('0.2')
add(x, y)                         // 0.3 (exact)

// Very large numbers
const large = bignumber('1e+500')
multiply(large, large)            // 1e+1000

// Precise division
divide(bignumber(1), bignumber(3)) // 0.333... (128 digits)

// Avoid floating point errors
bignumber(0.1)
  .plus(0.2)
  .equals(0.3)                    // true
```

### Fraction (Rational Numbers)

Use Fraction for exact rational arithmetic.

```javascript
import { fraction, add, multiply, divide } from 'mathjs'

// Exact fractions
const a = fraction(1, 3)          // 1/3
const b = fraction(1, 6)          // 1/6

add(a, b)                         // 1/2 (exact)
multiply(a, 3)                    // 1 (exact)
divide(fraction(1, 2), fraction(3, 4))  // 2/3

// Convert decimals to fractions
fraction(0.125)                   // 1/8
fraction(0.333333)                // 1/3 (approximate)

// Access numerator and denominator
const f = fraction(3, 4)
console.log(f.n, f.d)             // 3, 4
```

### Bigint (ES2020 Big Integers)

Use bigint for exact integer arithmetic beyond Number.MAX_SAFE_INTEGER.

```javascript
import { bigint, add, multiply, pow } from 'mathjs'

// Large integers
const a = bigint('9007199254740991')  // Number.MAX_SAFE_INTEGER
const b = bigint(1)

add(a, b)                         // 9007199254740992n (exact)

// Factorial of large numbers
pow(bigint(100), bigint(2))       // 10000n

// Integer-only operations
multiply(bigint(123), bigint(456)) // 56088n
```

### Type Mixing

Math.js handles mixed-type operations intelligently:

```javascript
import { add, multiply, fraction, bignumber } from 'mathjs'

// Fraction + number -> Fraction
add(fraction(1, 2), 0.25)         // 3/4

// BigNumber + number -> BigNumber
add(bignumber(1), 2)              // BigNumber(3)

// Complex + real -> Complex
add(complex(1, 2), 3)             // 4 + 2i

// Matrix operations preserve element types
const A = matrix([fraction(1, 2), fraction(1, 3)])
multiply(A, 2)                    // [1, 2/3]
```

## Type Conversion Best Practices

### When to Use Each Type

**JavaScript number:**
- Default for most operations
- Fast performance
- 15-17 decimal digits precision
- Limited range (±10^308)

**BigNumber:**
- Financial calculations requiring precision
- Scientific computations with very large/small numbers
- When you need more than 15 digits of precision
- Configurable precision

**Fraction:**
- Exact rational arithmetic
- When you need to preserve exact ratios (e.g., 1/3)
- Music theory, cooking recipes, etc.
- Slower than number or BigNumber

**Bigint:**
- Large integer calculations
- Cryptography
- Combinatorics with large numbers
- Integer-only operations

**Complex:**
- Electrical engineering
- Quantum mechanics
- Signal processing
- Automatic for roots of negative numbers

### Conversion Examples

```javascript
import { bignumber, fraction, number, string, format } from 'mathjs'

// Number -> BigNumber -> Number
const n = 123.456
const bn = bignumber(n)
number(bn)                        // 123.456

// Number -> Fraction -> Number
const f = fraction(0.75)          // 3/4
number(f)                         // 0.75

// Format for display
format(fraction(1, 3))            // '1/3'
format(bignumber('1.23e+500'))    // '1.23e+500'
format(fraction(1, 3), { fraction: 'decimal' })  // '0.333...'
```
