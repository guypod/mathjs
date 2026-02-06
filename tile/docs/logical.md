# Logical and Comparison Operations

Boolean logic, comparison operators, and relational operations. These functions work with numbers, strings, and support element-wise operations on arrays and matrices.

## Capabilities

### Logical Operations

Boolean logic operations supporting AND, OR, XOR, and NOT.

```javascript { .api }
/**
 * Logical AND operation
 * @param x - First value
 * @param y - Second value
 * @returns true if both are true/truthy
 */
function and(x: MathType, y: MathType): boolean | MathCollection

/**
 * Logical OR operation
 * @param x - First value
 * @param y - Second value
 * @returns true if either is true/truthy
 */
function or(x: MathType, y: MathType): boolean | MathCollection

/**
 * Logical XOR (exclusive OR) operation
 * @param x - First value
 * @param y - Second value
 * @returns true if exactly one is true/truthy
 */
function xor(x: MathType, y: MathType): boolean | MathCollection

/**
 * Logical NOT operation
 * @param x - Value to negate
 * @returns Logical negation
 */
function not(x: MathType): boolean | MathCollection
```

**Usage Examples:**

```javascript
import { and, or, xor, not } from 'mathjs'

// Basic logical operations
and(true, true)               // true
and(true, false)              // false
or(true, false)               // true
or(false, false)              // false
xor(true, false)              // true
xor(true, true)               // false
not(true)                     // false
not(false)                    // true

// With numbers (truthy/falsy)
and(1, 1)                     // true
and(1, 0)                     // false
or(0, 1)                      // true
not(0)                        // true
not(1)                        // false

// Element-wise on arrays
and([true, true, false], [true, false, false])  // [true, false, false]
or([true, false], [false, false])               // [true, false]
not([true, false, true])                        // [false, true, false]
```

### Equality Comparisons

Test equality between values.

**IMPORTANT**: `equal()`, `unequal()` work with numbers, BigNumber, Complex, Units, and matrices, but do **NOT** support arbitrary string comparisons. Strings will be attempted to convert to numbers. For string comparisons, use `equalText()`.

```javascript { .api }
/**
 * Test equality (with tolerance for numbers)
 * Attempts to convert strings to numbers before comparison
 * @param x - First value (number, BigNumber, Complex, Unit, Matrix)
 * @param y - Second value (number, BigNumber, Complex, Unit, Matrix)
 * @returns true if equal
 * @throws Error if strings cannot be converted to numbers
 */
function equal(x: MathType, y: MathType): boolean | MathCollection

/**
 * Test inequality
 * Attempts to convert strings to numbers before comparison
 * @param x - First value (number, BigNumber, Complex, Unit, Matrix)
 * @param y - Second value (number, BigNumber, Complex, Unit, Matrix)
 * @returns true if not equal
 * @throws Error if strings cannot be converted to numbers
 */
function unequal(x: MathType, y: MathType): boolean | MathCollection

/**
 * Deep equality check (exact, no tolerance)
 * @param x - First value
 * @param y - Second value
 * @returns true if deeply equal
 */
function deepEqual(x: any, y: any): boolean

/**
 * Test string equality
 * @param x - First string
 * @param y - Second string
 * @returns true if strings are equal
 */
function equalText(x: string, y: string): boolean
```

**Usage Examples:**

```javascript
import { equal, unequal, deepEqual, equalText } from 'mathjs'

// Number equality (with tolerance)
equal(2, 2)                   // true
equal(2, 3)                   // false
equal(2.000001, 2)            // true (within tolerance)
unequal(2, 3)                 // true

// String equality - use equalText() for strings
equalText('hello', 'hello')   // true
equalText('Hello', 'hello')   // false (case-sensitive)
// equal('hello', 'hello') would throw error - use equalText() instead!

// Deep equality (exact, no tolerance)
deepEqual(2, 2)               // true
deepEqual(2.000001, 2)        // false (exact)
deepEqual([1, 2], [1, 2])     // true
deepEqual({a: 1}, {a: 1})     // true

// Element-wise on arrays
equal([1, 2, 3], [1, 2, 4])   // [true, true, false]
```

### Relational Comparisons

Compare magnitude of values.

**IMPORTANT**: `larger()`, `smaller()`, `largerEq()`, `smallerEq()` work with numbers, BigNumber, Complex, Units, and matrices, but do **NOT** support arbitrary string comparisons. Strings will be attempted to convert to numbers. For string comparisons, use `compareText()` or `compareNatural()`.

```javascript { .api }
/**
 * Test if x is larger than y (x > y)
 * Attempts to convert strings to numbers before comparison
 * @param x - First value (number, BigNumber, Complex, Unit, Matrix)
 * @param y - Second value (number, BigNumber, Complex, Unit, Matrix)
 * @returns true if x > y
 * @throws Error if strings cannot be converted to numbers
 */
function larger(x: MathType, y: MathType): boolean | MathCollection

/**
 * Test if x is larger than or equal to y (x >= y)
 * Attempts to convert strings to numbers before comparison
 * @param x - First value (number, BigNumber, Complex, Unit, Matrix)
 * @param y - Second value (number, BigNumber, Complex, Unit, Matrix)
 * @returns true if x >= y
 * @throws Error if strings cannot be converted to numbers
 */
function largerEq(x: MathType, y: MathType): boolean | MathCollection

/**
 * Test if x is smaller than y (x < y)
 * Attempts to convert strings to numbers before comparison
 * @param x - First value (number, BigNumber, Complex, Unit, Matrix)
 * @param y - Second value (number, BigNumber, Complex, Unit, Matrix)
 * @returns true if x < y
 * @throws Error if strings cannot be converted to numbers
 */
function smaller(x: MathType, y: MathType): boolean | MathCollection

/**
 * Test if x is smaller than or equal to y (x <= y)
 * Attempts to convert strings to numbers before comparison
 * @param x - First value (number, BigNumber, Complex, Unit, Matrix)
 * @param y - Second value (number, BigNumber, Complex, Unit, Matrix)
 * @returns true if x <= y
 * @throws Error if strings cannot be converted to numbers
 */
function smallerEq(x: MathType, y: MathType): boolean | MathCollection
```

**Usage Examples:**

```javascript
import { larger, largerEq, smaller, smallerEq } from 'mathjs'

// Basic comparisons
larger(5, 3)                  // true
larger(3, 5)                  // false
largerEq(5, 5)                // true
smaller(3, 5)                 // true
smallerEq(3, 3)               // true

// With units
larger(unit('5 cm'), unit('2 inch'))  // false (5cm < 2inch)
smaller(unit('1 mile'), unit('2 km')) // false (1mile > 2km)

// Element-wise on arrays
larger([1, 5, 3], [2, 3, 4])  // [false, true, false]
smaller([1, 2, 3], 2)         // [true, false, false]
```

### Comparison Utilities

Additional comparison and ordering functions.

```javascript { .api }
/**
 * Three-way comparison
 * @param x - First value
 * @param y - Second value
 * @returns -1 if x < y, 0 if x == y, 1 if x > y
 */
function compare(x: MathType, y: MathType): number | MathCollection

/**
 * Natural order comparison (for sorting)
 * @param x - First value
 * @param y - Second value
 * @returns -1, 0, or 1 for natural ordering
 */
function compareNatural(x: any, y: any): number

/**
 * Lexical string comparison
 * @param x - First string
 * @param y - Second string
 * @returns -1, 0, or 1 for lexical ordering
 */
function compareText(x: string, y: string): number
```

**Usage Examples:**

```javascript
import { compare, compareNatural, compareText, sort } from 'mathjs'

// Three-way comparison
compare(5, 3)                 // 1
compare(3, 5)                 // -1
compare(3, 3)                 // 0

// Element-wise
compare([1, 5, 3], [2, 3, 4]) // [-1, 1, -1]

// Natural ordering (for mixed types)
compareNatural(1, 2)          // -1
compareNatural('a', 'b')      // -1
compareNatural(null, 0)       // -1

// Text comparison
compareText('apple', 'banana') // -1
compareText('Zebra', 'apple')  // -1 (case-sensitive)

// Use in sorting
const arr = [5, 2, 8, 1, 9]
sort(arr, compare)            // [1, 2, 5, 8, 9]
```

## Usage in Expressions

Logical and comparison operators can be used in string expressions:

```javascript
import { evaluate } from 'mathjs'

// Logical operators
evaluate('true and false')          // false
evaluate('true or false')           // true
evaluate('true xor true')           // false
evaluate('not true')                // false

// Comparison operators
evaluate('5 > 3')                   // true
evaluate('5 >= 5')                  // true
evaluate('3 < 5')                   // true
evaluate('5 == 5')                  // true
evaluate('5 != 3')                  // true

// Combined expressions
evaluate('(5 > 3) and (2 < 4)')     // true
evaluate('x > 10 or y < 5', { x: 8, y: 3 })  // true

// Chained comparisons
evaluate('1 < 2 < 3')               // true
evaluate('1 < 2 < 1.5')             // false
```

## Tolerance Configuration

Equality and comparison operations use configured tolerance for floating-point numbers:

```javascript
import { config, equal, compare } from 'mathjs'

// Default tolerance
equal(2, 2.00001)                // true (within default tolerance)

// Configure tolerance
config({
  relTol: 1e-10,  // Relative tolerance
  absTol: 1e-12   // Absolute tolerance
})

equal(2, 2.00001)                // false (outside new tolerance)

// Comparison with tolerance
compare(1.0000001, 1)            // 0 (considered equal within tolerance)
```

## Working with Arrays and Matrices

All logical and comparison operations support element-wise operations:

```javascript
import { and, or, equal, larger, smaller } from 'mathjs'

const a = [1, 2, 3, 4, 5]
const b = [5, 4, 3, 2, 1]

// Element-wise comparisons
larger(a, b)              // [false, false, false, true, true]
smaller(a, 3)             // [true, true, false, false, false]
equal(a, b)               // [false, false, true, false, false]

// Logical operations on boolean arrays
const x = [true, true, false, false]
const y = [true, false, true, false]

and(x, y)                 // [true, false, false, false]
or(x, y)                  // [true, true, true, false]
xor(x, y)                 // [false, true, true, false]
not(x)                    // [false, false, true, true]
```

## Conditional Expressions

Use comparisons with conditional (ternary) expressions:

```javascript
import { evaluate } from 'mathjs'

// Basic ternary
evaluate('5 > 3 ? "yes" : "no"')           // "yes"
evaluate('x > 0 ? 1 : -1', { x: 5 })       // 1
evaluate('x > 0 ? 1 : -1', { x: -3 })      // -1

// Nested conditionals
evaluate('x > 0 ? "positive" : x < 0 ? "negative" : "zero"', { x: 0 })  // "zero"

// With computations
evaluate('x > 10 ? x * 2 : x / 2', { x: 15 })  // 30
evaluate('x > 10 ? x * 2 : x / 2', { x: 5 })   // 2.5
```

## Boolean Context

Values are coerced to boolean in logical operations following JavaScript rules:

**Truthy values:**
- `true`
- Non-zero numbers
- Non-empty strings
- Non-null objects

**Falsy values:**
- `false`
- `0`
- Empty string `''`
- `null`
- `undefined`
- `NaN`

```javascript
import { and, or, not } from 'mathjs'

// Truthy/falsy coercion
and(1, 2)                 // true (both truthy)
and(1, 0)                 // false (0 is falsy)
or(0, 'hello')            // true ('hello' is truthy)
not(0)                    // true (0 is falsy)
not('')                   // true (empty string is falsy)
not('text')               // false (non-empty string is truthy)
```

## Type-Specific Comparisons

Different types have specific comparison behaviors:

```javascript
import { equal, larger, compare } from 'mathjs'

// Numbers: numerical comparison
equal(2, 2.0)             // true
larger(5, 3)              // true

// Strings: lexical comparison
larger('b', 'a')          // true
larger('10', '2')         // false (lexical: '1' < '2')

// Units: converted to same unit before comparison
larger(unit('2 m'), unit('100 cm'))  // true (2m > 1m)

// BigNumber: arbitrary precision comparison
equal(bignumber(0.1).plus(0.2), bignumber(0.3))  // true

// Complex: magnitude comparison (for larger/smaller)
larger(complex(3, 4), complex(2, 2))  // true (|3+4i| > |2+2i|)
```
