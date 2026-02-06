# Arithmetic Operations

Comprehensive arithmetic functions including basic operations, logarithms, roots, rounding functions, and number theory operations. All functions support element-wise operations on arrays and matrices.

## Capabilities

### Absolute Value

Returns the absolute value of a number.

```javascript { .api }
/**
 * Calculate the absolute value
 * @param x - Input value
 * @returns Absolute value
 */
function abs(x: number): number
function abs(x: BigNumber): BigNumber
function abs(x: Complex): number
function abs(x: Fraction): Fraction
function abs(x: Unit): Unit
function abs(x: MathCollection): MathCollection
```

### Addition

Add two or more values.

```javascript { .api }
/**
 * Add two or more values
 * @param x - First value
 * @param y - Second value
 * @param values - Additional values to add
 * @returns Sum of all values
 */
function add(x: MathType, y: MathType, ...values: MathType[]): MathType
```

**Usage Examples:**

```javascript
import { add } from 'mathjs'

add(2, 3)              // 5
add(2, 3, 4)           // 9
add(complex(2, 3), complex(4, 1))  // Complex{6, 4}
add(unit('5 cm'), unit('2.5 cm')) // 7.5 cm
add([1, 2], [3, 4])    // [4, 6] (element-wise)
```

### Subtraction

Subtract one value from another.

```javascript { .api }
/**
 * Subtract two values
 * @param x - First value
 * @param y - Value to subtract
 * @returns Difference
 */
function subtract(x: MathType, y: MathType): MathType
```

### Multiplication

Multiply two or more values.

```javascript { .api }
/**
 * Multiply two or more values
 * @param x - First value
 * @param y - Second value
 * @param values - Additional values to multiply
 * @returns Product of all values
 */
function multiply(x: MathType, y: MathType, ...values: MathType[]): MathType
```

**Usage Examples:**

```javascript
import { multiply } from 'mathjs'

multiply(2, 3)         // 6
multiply(2, 3, 4)      // 24
multiply(complex(2, 3), complex(2, 1))  // Complex{1, 8}
multiply([[1, 2], [3, 4]], [[5, 6], [7, 8]])  // Matrix multiplication
```

### Division

Divide two values.

```javascript { .api }
/**
 * Divide two values
 * @param x - Numerator
 * @param y - Denominator
 * @returns Quotient
 */
function divide(x: MathType, y: MathType): MathType
```

### Power and Exponentiation

Raise a value to a power.

```javascript { .api }
/**
 * Calculate the power of x to y: x^y
 * @param x - Base
 * @param y - Exponent
 * @returns x raised to power y
 */
function pow(x: MathType, y: MathNumericType): MathType

/**
 * Calculate square: x^2
 * @param x - Input value
 * @returns x squared
 */
function square(x: MathNumericType | Unit): MathNumericType | Unit

/**
 * Calculate cube: x^3
 * @param x - Input value
 * @returns x cubed
 */
function cube(x: MathNumericType | Unit): MathNumericType | Unit
```

### Roots

Calculate square roots, cube roots, and nth roots.

```javascript { .api }
/**
 * Calculate the square root
 * @param x - Input value
 * @returns Square root of x
 */
function sqrt(x: number): number | Complex
function sqrt(x: BigNumber): BigNumber
function sqrt(x: Complex): Complex
function sqrt(x: Unit): Unit
function sqrt(x: MathCollection): MathCollection

/**
 * Calculate the cube root
 * @param x - Input value
 * @param allRoots - If true, return all three roots (for Complex)
 * @returns Cube root of x
 */
function cbrt(x: number): number
function cbrt(x: BigNumber): BigNumber
function cbrt(x: Complex, allRoots?: boolean): Complex | Complex[]
function cbrt(x: Unit): Unit
function cbrt(x: MathCollection): MathCollection

/**
 * Calculate the nth root
 * @param x - Input value
 * @param root - The root (default: 2)
 * @returns Nth root of x
 */
function nthRoot(x: MathNumericType, root?: MathNumericType): MathNumericType
function nthRoot(x: MathCollection, root?: MathNumericType): MathCollection

/**
 * Calculate all nth roots of a value (complex)
 * @param x - Input value
 * @param n - The root (default: 2)
 * @returns Array of all nth roots
 */
function nthRoots(x: number | Complex, n?: number): Complex[]
```

### Exponential and Logarithmic

Exponential and logarithmic functions.

```javascript { .api }
/**
 * Calculate the exponential: e^x
 * @param x - Exponent
 * @returns e raised to power x
 */
function exp(x: number): number
function exp(x: BigNumber): BigNumber
function exp(x: Complex): Complex
function exp(x: MathCollection): MathCollection

/**
 * Calculate e^x - 1 (more accurate for small x)
 * @param x - Exponent
 * @returns e^x - 1
 */
function expm1(x: number): number
function expm1(x: BigNumber): BigNumber
function expm1(x: Complex): Complex
function expm1(x: MathCollection): MathCollection

/**
 * Calculate the natural logarithm
 * @param x - Input value
 * @param base - Optional logarithm base (default: e)
 * @returns Logarithm of x
 */
function log(x: MathNumericType, base?: MathNumericType): MathNumericType
function log(x: MathCollection, base?: MathNumericType): MathCollection

/**
 * Calculate the base-10 logarithm
 * @param x - Input value
 * @returns Base-10 logarithm of x
 */
function log10(x: MathNumericType): MathNumericType
function log10(x: MathCollection): MathCollection

/**
 * Calculate the base-2 logarithm
 * @param x - Input value
 * @returns Base-2 logarithm of x
 */
function log2(x: MathNumericType): MathNumericType
function log2(x: MathCollection): MathCollection

/**
 * Calculate log(1 + x) (more accurate for small x)
 * @param x - Input value
 * @returns log(1 + x)
 */
function log1p(x: MathNumericType): MathNumericType
function log1p(x: MathCollection): MathCollection
```

### Rounding Functions

Various rounding operations.

```javascript { .api }
/**
 * Round to nearest integer
 * @param x - Value to round
 * @param n - Number of digits (default: 0)
 * @returns Rounded value
 */
function round(x: MathNumericType, n?: number | BigNumber): MathNumericType
function round(x: MathCollection, n?: number | BigNumber): MathCollection
function round(x: Unit, unit: Unit): Unit
function round(x: Unit, n: number | BigNumber, unit: Unit): Unit

/**
 * Round up (ceiling)
 * @param x - Value to round
 * @param n - Number of digits (default: 0)
 * @returns Rounded up value
 */
function ceil(x: MathNumericType, n?: number | BigNumber): MathNumericType
function ceil(x: MathCollection, n?: number | BigNumber): MathCollection
function ceil(x: Unit, unit: Unit): Unit
function ceil(x: Unit, n: number | BigNumber, unit: Unit): Unit

/**
 * Round down (floor)
 * @param x - Value to round
 * @param n - Number of digits (default: 0)
 * @returns Rounded down value
 */
function floor(x: MathNumericType, n?: number | BigNumber): MathNumericType
function floor(x: MathCollection, n?: number | BigNumber): MathCollection
function floor(x: Unit, unit: Unit): Unit
function floor(x: Unit, n: number | BigNumber, unit: Unit): Unit

/**
 * Round towards zero
 * @param x - Value to round
 * @param n - Number of digits (default: 0)
 * @returns Value rounded towards zero
 */
function fix(x: MathNumericType, n?: number | BigNumber): MathNumericType
function fix(x: MathCollection, n?: number | BigNumber): MathCollection
function fix(x: Unit, unit: Unit): Unit
function fix(x: Unit, n: number | BigNumber, unit: Unit): Unit
```

### Sign and Modulo

Sign function and modulo operations.

```javascript { .api }
/**
 * Compute the sign of a value: -1, 0, or 1
 * @param x - Input value
 * @returns Sign of x
 */
function sign(x: MathType): MathType

/**
 * Calculate the modulo (remainder after division)
 * @param x - Dividend
 * @param y - Divisor
 * @returns Remainder of x / y
 */
function mod(x: MathType, y: MathType): MathType

/**
 * Negate a value
 * @param x - Value to negate
 * @returns -x
 */
function unaryMinus(x: MathType): MathType

/**
 * Unary plus (convert to number)
 * @param x - Value to convert
 * @returns +x
 */
function unaryPlus(x: string | MathType): MathType
```

### Number Theory

Greatest common divisor, least common multiple, and extended GCD.

```javascript { .api }
/**
 * Calculate the greatest common divisor
 * @param args - Two or more values
 * @returns Greatest common divisor
 */
function gcd(...args: MathType[]): MathType
function gcd(args: MathType[]): MathType

/**
 * Calculate the least common multiple
 * @param a - First value
 * @param b - Second value
 * @returns Least common multiple
 */
function lcm(a: MathType, b: MathType): MathType

/**
 * Calculate the extended greatest common divisor
 * Returns [gcd, x, y] where gcd(a,b) = a*x + b*y
 * @param a - First value
 * @param b - Second value
 * @returns Array [gcd, x, y]
 */
function xgcd(a: number | BigNumber, b: number | BigNumber): MathArray
```

**Usage Examples:**

```javascript
import { gcd, lcm, xgcd } from 'mathjs'

gcd(12, 8)        // 4
gcd(12, 8, 6)     // 2
lcm(4, 6)         // 12
xgcd(36, 24)      // [12, -1, 2] since 36*(-1) + 24*2 = 12
```

### Vector Operations

Norm and hypot for vector calculations.

```javascript { .api }
/**
 * Calculate the norm of a vector or matrix
 * @param x - Vector or matrix
 * @param p - Norm type (default: 2 for Euclidean)
 *            1 = sum of absolute values
 *            2 = Euclidean (default)
 *            Infinity = max absolute value
 * @returns Norm of x
 */
function norm(x: MathType, p?: number | string): number | BigNumber

/**
 * Calculate the hypotenuse: sqrt(x1^2 + x2^2 + ...)
 * @param args - Values
 * @returns Hypotenuse
 */
function hypot(...args: MathNumericType[]): MathNumericType
function hypot(args: MathNumericType[]): MathNumericType
```

**Usage Examples:**

```javascript
import { norm, hypot } from 'mathjs'

norm([3, 4])          // 5 (Euclidean norm)
norm([3, 4], 1)       // 7 (Manhattan norm)
norm([3, 4], Infinity) // 4 (Max norm)
hypot(3, 4)           // 5
hypot(3, 4, 5)        // 7.07...
```

### Element-wise Matrix Operations

Operations that apply element-wise to matrices (with dot notation).

```javascript { .api }
/**
 * Multiply matrices element-wise
 * @param x - First matrix/array
 * @param y - Second matrix/array
 * @returns Element-wise product
 */
function dotMultiply(x: MathCollection, y: MathCollection): MathCollection
function dotMultiply(x: Unit, y: MathType): Unit
function dotMultiply(x: MathType, y: Unit): Unit

/**
 * Divide matrices element-wise
 * @param x - Numerator matrix/array
 * @param y - Denominator matrix/array
 * @returns Element-wise quotient
 */
function dotDivide(x: MathCollection, y: MathCollection): MathCollection
function dotDivide(x: Unit, y: MathType): Unit
function dotDivide(x: MathType, y: Unit): Unit

/**
 * Calculate power element-wise
 * @param x - Base matrix/array
 * @param y - Exponent
 * @returns Element-wise power
 */
function dotPow(x: MathType, y: MathType): MathType
```
