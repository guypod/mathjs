# Arithmetic Operations

Math.js provides 39 comprehensive arithmetic functions supporting all numeric types including numbers, BigNumber, Complex, Fraction, Unit, and matrices. These operations form the foundation of mathematical computation in the library.

## Core Imports

```javascript { .api }
import {
  abs, add, subtract, multiply, divide, pow, sqrt,
  mod, gcd, lcm, log, exp, round, ceil, floor
} from 'mathjs';
```

## Basic Arithmetic

### add

Adds two values. Supports element-wise addition for matrices.

```typescript { .api }
function add(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: First value (number, BigNumber, Complex, Fraction, Unit, Matrix, or Array)
- `y`: Second value (must be compatible type)

**Returns:** Sum of x and y

**Usage Examples:**

```javascript
import { add, bignumber, complex, unit, matrix } from 'mathjs';

// Numbers
add(2, 3);                        // 5
add(2.5, 3.8);                    // 6.3

// BigNumber for arbitrary precision
add(bignumber('0.1'), bignumber('0.2'));
// BigNumber 0.3 (exact)

// Complex numbers
add(complex(2, 3), complex(1, 4));
// Complex {re: 3, im: 7}

// Units
add(unit('5 cm'), unit('2 cm'));
// Unit 7 cm

// Matrices - element-wise
add([[1, 2], [3, 4]], [[5, 6], [7, 8]]);
// [[6, 8], [10, 12]]
```

### addScalar

Adds a scalar value to all entries in a matrix or array.

```typescript { .api }
function addScalar(x: MathType, y: number | BigNumber | Fraction | Complex): MathType;
```

**Parameters:**
- `x`: Value or matrix
- `y`: Scalar to add

**Returns:** Result with scalar added to each element

**Usage Examples:**

```javascript
import { addScalar, matrix } from 'mathjs';

// Add scalar to all matrix elements
addScalar([[1, 2], [3, 4]], 10);
// [[11, 12], [13, 14]]

addScalar(matrix([1, 2, 3]), 5);
// Matrix [6, 7, 8]
```

### subtract

Subtracts two values.

```typescript { .api }
function subtract(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: First value (minuend)
- `y`: Second value (subtrahend)

**Returns:** Difference x - y

**Usage Examples:**

```javascript
import { subtract, complex, unit } from 'mathjs';

subtract(10, 3);                  // 7
subtract(complex(5, 6), complex(2, 1));
// Complex {re: 3, im: 5}

subtract(unit('10 kg'), unit('3 kg'));
// Unit 7 kg
```

### subtractScalar

Subtracts a scalar from all entries in a matrix or array.

```typescript { .api }
function subtractScalar(x: MathType, y: number | BigNumber | Fraction | Complex): MathType;
```

**Parameters:**
- `x`: Value or matrix
- `y`: Scalar to subtract

**Returns:** Result with scalar subtracted from each element

### multiply

Multiplies two values. For matrices, performs matrix multiplication.

```typescript { .api }
function multiply(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** Product of x and y

**Usage Examples:**

```javascript
import { multiply, complex, matrix } from 'mathjs';

multiply(4, 5);                   // 20

// Complex multiplication
multiply(complex(2, 3), complex(1, 4));
// Complex {re: -10, im: 11}

// Matrix multiplication
multiply([[1, 2], [3, 4]], [[5], [6]]);
// [[17], [39]]

// Scalar-matrix multiplication
multiply(2, [[1, 2], [3, 4]]);
// [[2, 4], [6, 8]]
```

### multiplyScalar

Multiplies all entries in a matrix or array by a scalar.

```typescript { .api }
function multiplyScalar(x: MathType, y: number | BigNumber | Fraction | Complex): MathType;
```

**Parameters:**
- `x`: Value or matrix
- `y`: Scalar multiplier

**Returns:** Result with each element multiplied by scalar

### divide

Divides two values. For matrices, multiplies by the inverse.

```typescript { .api }
function divide(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: Numerator
- `y`: Denominator

**Returns:** Quotient x / y

**Usage Examples:**

```javascript
import { divide, fraction, complex, unit } from 'mathjs';

divide(10, 4);                    // 2.5

// Fraction for exact rational arithmetic
divide(fraction(1, 3), fraction(1, 2));
// Fraction 2/3

// Complex division
divide(complex(10, 0), complex(0, 1));
// Complex {re: 0, im: -10}

// Unit conversion during division
divide(unit('1 m'), unit('1 s'));
// Unit 1 m/s
```

### divideScalar

Divides all entries in a matrix or array by a scalar.

```typescript { .api }
function divideScalar(x: MathType, y: number | BigNumber | Fraction | Complex): MathType;
```

**Parameters:**
- `x`: Value or matrix
- `y`: Scalar divisor

**Returns:** Result with each element divided by scalar

### dotDivide

Element-wise division of two matrices or arrays.

```typescript { .api }
function dotDivide(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: Numerator matrix/array
- `y`: Denominator matrix/array (must have compatible dimensions)

**Returns:** Element-wise quotient

**Usage Examples:**

```javascript
import { dotDivide } from 'mathjs';

dotDivide([10, 20, 30], [2, 4, 5]);
// [5, 5, 6]

dotDivide([[10, 20], [30, 40]], [[2, 4], [5, 8]]);
// [[5, 5], [6, 5]]
```

### dotMultiply

Element-wise multiplication of two matrices or arrays.

```typescript { .api }
function dotMultiply(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: First matrix/array
- `y`: Second matrix/array (must have compatible dimensions)

**Returns:** Element-wise product

**Usage Examples:**

```javascript
import { dotMultiply } from 'mathjs';

dotMultiply([2, 3, 4], [5, 6, 7]);
// [10, 18, 28]

dotMultiply([[1, 2], [3, 4]], [[5, 6], [7, 8]]);
// [[5, 12], [21, 32]]
```

### dotPow

Element-wise power operation on matrices or arrays.

```typescript { .api }
function dotPow(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: Base matrix/array
- `y`: Exponent matrix/array or scalar

**Returns:** Element-wise power result

**Usage Examples:**

```javascript
import { dotPow } from 'mathjs';

// Element-wise power
dotPow([2, 3, 4], [2, 3, 2]);
// [4, 27, 16]

// Single exponent for all elements
dotPow([2, 3, 4], 2);
// [4, 9, 16]
```

## Powers and Roots

### pow

Calculates the power of x to y (x^y).

```typescript { .api }
function pow(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: Base value
- `y`: Exponent value

**Returns:** x raised to the power y

**Usage Examples:**

```javascript
import { pow, complex, bignumber } from 'mathjs';

pow(2, 3);                        // 8
pow(4, 0.5);                      // 2 (square root)
pow(2, -1);                       // 0.5

// Complex exponentiation
pow(complex(-1, 0), 0.5);
// Complex {re: 0, im: 1}  (square root of -1)

// Arbitrary precision
pow(bignumber(2), bignumber(100));
// BigNumber 1.2676506002282294e+30
```

### sqrt

Calculates the square root of a value.

```typescript { .api }
function sqrt(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to take square root of

**Returns:** Square root of x

**Usage Examples:**

```javascript
import { sqrt, complex } from 'mathjs';

sqrt(16);                         // 4
sqrt(2);                          // 1.4142135623730951

// Complex result for negative numbers
sqrt(-4);                         // Complex {re: 0, im: 2}

// Matrix element-wise
sqrt([[4, 9], [16, 25]]);
// [[2, 3], [4, 5]]
```

### square

Calculates the square of a value (x^2).

```typescript { .api }
function square(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to square

**Returns:** x squared

**Usage Examples:**

```javascript
import { square } from 'mathjs';

square(5);                        // 25
square(complex(2, 3));            // Complex {re: -5, im: 12}
square([1, 2, 3]);                // [1, 4, 9]
```

### cube

Calculates the cube of a value (x^3).

```typescript { .api }
function cube(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to cube

**Returns:** x cubed

**Usage Examples:**

```javascript
import { cube } from 'mathjs';

cube(3);                          // 27
cube(-2);                         // -8
cube([1, 2, 3]);                  // [1, 8, 27]
```

### cbrt

Calculates the cubic root (cube root) of a value.

```typescript { .api }
function cbrt(x: MathType, allRoots?: boolean): MathType | MathType[];
```

**Parameters:**
- `x`: Value to take cube root of
- `allRoots`: If true, returns all three complex roots (default: false)

**Returns:** Principal cube root, or array of all roots if allRoots is true

**Usage Examples:**

```javascript
import { cbrt } from 'mathjs';

cbrt(27);                         // 3
cbrt(-8);                         // -2

// All three cube roots (includes complex roots)
cbrt(27, true);
// [3, Complex {re: -1.5, im: 2.598...}, Complex {re: -1.5, im: -2.598...}]
```

### nthRoot

Calculates the nth root of a value (principal root).

```typescript { .api }
function nthRoot(x: MathType, n?: number | BigNumber): MathType;
```

**Parameters:**
- `x`: Value to take root of
- `n`: Root degree (default: 2 for square root)

**Returns:** Principal nth root of x

**Usage Examples:**

```javascript
import { nthRoot } from 'mathjs';

nthRoot(16, 4);                   // 2 (fourth root)
nthRoot(32, 5);                   // 2 (fifth root)
nthRoot(9);                       // 3 (square root, default)

// Works with complex numbers
nthRoot(-8, 3);                   // -2
```

### nthRoots

Calculates all nth complex roots of a value.

```typescript { .api }
function nthRoots(x: MathType, n?: number | BigNumber): Complex[];
```

**Parameters:**
- `x`: Value to find roots of
- `n`: Root degree (must be positive integer)

**Returns:** Array of all n complex roots

**Usage Examples:**

```javascript
import { nthRoots } from 'mathjs';

// All square roots of -1
nthRoots(-1, 2);
// [Complex {re: 0, im: 1}, Complex {re: 0, im: -1}]

// All cube roots of 1
nthRoots(1, 3);
// [
//   Complex {re: 1, im: 0},
//   Complex {re: -0.5, im: 0.866...},
//   Complex {re: -0.5, im: -0.866...}
// ]

// Fourth roots of 16
nthRoots(16, 4);
// [2, 2i, -2, -2i] (as Complex objects)
```

## Exponential and Logarithmic

### exp

Calculates the exponential of a value (e^x).

```typescript { .api }
function exp(x: MathType): MathType;
```

**Parameters:**
- `x`: Exponent value

**Returns:** e raised to the power x

**Usage Examples:**

```javascript
import { exp, complex } from 'mathjs';

exp(1);                           // 2.718281828459045 (e)
exp(0);                           // 1
exp(2);                           // 7.389056098930650

// Complex exponential (Euler's formula)
exp(complex(0, Math.PI));         // Complex {re: -1, im: 0}

// Matrix element-wise
exp([0, 1, 2]);                   // [1, 2.718..., 7.389...]
```

### expm1

Calculates e^x - 1 with high precision for small values of x.

```typescript { .api }
function expm1(x: MathType): MathType;
```

**Parameters:**
- `x`: Exponent value

**Returns:** e^x - 1 calculated accurately

**Usage Examples:**

```javascript
import { expm1 } from 'mathjs';

// More accurate than exp(x) - 1 for small x
expm1(1e-10);                     // 1.00000000005e-10
expm1(0);                         // 0
expm1(1);                         // 1.718281828459045
```

### log

Calculates the logarithm of a value.

```typescript { .api }
function log(x: MathType, base?: MathType): MathType;
```

**Parameters:**
- `x`: Value to take logarithm of
- `base`: Base of logarithm (default: e for natural log)

**Returns:** Logarithm of x to the specified base

**Usage Examples:**

```javascript
import { log, e, complex } from 'mathjs';

// Natural logarithm (ln)
log(e);                           // 1
log(10);                          // 2.302585092994046

// Logarithm with custom base
log(100, 10);                     // 2 (log base 10)
log(8, 2);                        // 3 (log base 2)

// Complex logarithm
log(complex(-1, 0));              // Complex {re: 0, im: 3.14159...}
```

### log10

Calculates the base-10 logarithm of a value.

```typescript { .api }
function log10(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to take logarithm of

**Returns:** Base-10 logarithm of x

**Usage Examples:**

```javascript
import { log10 } from 'mathjs';

log10(100);                       // 2
log10(1000);                      // 3
log10(1);                         // 0
```

### log2

Calculates the base-2 logarithm of a value.

```typescript { .api }
function log2(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to take logarithm of

**Returns:** Base-2 logarithm of x

**Usage Examples:**

```javascript
import { log2 } from 'mathjs';

log2(8);                          // 3
log2(1024);                       // 10
log2(1);                          // 0
```

### log1p

Calculates ln(1 + x) with high precision for small values of x.

```typescript { .api }
function log1p(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to add to 1 before taking logarithm

**Returns:** ln(1 + x) calculated accurately

**Usage Examples:**

```javascript
import { log1p } from 'mathjs';

// More accurate than log(1 + x) for small x
log1p(1e-10);                     // 9.999999999950000e-11
log1p(0);                         // 0
log1p(1);                         // 0.6931471805599453 (ln(2))
```

## Rounding and Sign

### abs

Calculates the absolute value of a number.

```typescript { .api }
function abs(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to take absolute value of

**Returns:** Absolute value of x

**Usage Examples:**

```javascript
import { abs, complex } from 'mathjs';

abs(-5);                          // 5
abs(3.5);                         // 3.5

// Complex magnitude
abs(complex(3, 4));               // 5 (sqrt(3^2 + 4^2))

// Element-wise for matrices
abs([-1, -2, 3]);                 // [1, 2, 3]
```

### sign

Determines the sign of a value.

```typescript { .api }
function sign(x: MathType): number | BigNumber | Fraction | MathType;
```

**Parameters:**
- `x`: Value to determine sign of

**Returns:** -1 for negative, 0 for zero, 1 for positive

**Usage Examples:**

```javascript
import { sign } from 'mathjs';

sign(-5);                         // -1
sign(0);                          // 0
sign(10);                         // 1
sign(-0.001);                     // -1

// Works with all numeric types
sign(bignumber(-100));            // BigNumber -1
```

### round

Rounds a value to a specified number of decimals.

```typescript { .api }
function round(x: MathType, n?: number | BigNumber): MathType;
```

**Parameters:**
- `x`: Value to round
- `n`: Number of decimals (default: 0)

**Returns:** Rounded value

**Usage Examples:**

```javascript
import { round } from 'mathjs';

round(3.7);                       // 4
round(3.2);                       // 3
round(3.14159, 2);                // 3.14
round(123.456, -1);               // 120 (round to tens place)

// Works with matrices
round([3.2, 3.7, -2.4]);          // [3, 4, -2]
```

### floor

Rounds a value down to a specified number of decimals.

```typescript { .api }
function floor(x: MathType, n?: number | BigNumber): MathType;
```

**Parameters:**
- `x`: Value to round down
- `n`: Number of decimals (default: 0)

**Returns:** Value rounded down

**Usage Examples:**

```javascript
import { floor } from 'mathjs';

floor(3.7);                       // 3
floor(-3.2);                      // -4
floor(3.14159, 2);                // 3.14
floor(123.456, -1);               // 120
```

### ceil

Rounds a value up to a specified number of decimals.

```typescript { .api }
function ceil(x: MathType, n?: number | BigNumber): MathType;
```

**Parameters:**
- `x`: Value to round up
- `n`: Number of decimals (default: 0)

**Returns:** Value rounded up

**Usage Examples:**

```javascript
import { ceil } from 'mathjs';

ceil(3.2);                        // 4
ceil(-3.7);                       // -3
ceil(3.14159, 2);                 // 3.15
ceil(123.456, -1);                // 130
```

### fix

Rounds a value towards zero to a specified number of decimals.

```typescript { .api }
function fix(x: MathType, n?: number | BigNumber): MathType;
```

**Parameters:**
- `x`: Value to round
- `n`: Number of decimals (default: 0)

**Returns:** Value rounded towards zero

**Usage Examples:**

```javascript
import { fix } from 'mathjs';

fix(3.7);                         // 3
fix(-3.7);                        // -3
fix(3.14159, 2);                  // 3.14
fix(-2.98, 1);                    // -2.9
```

### unaryMinus

Negates a value (unary minus operator).

```typescript { .api }
function unaryMinus(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to negate

**Returns:** Negative of x

**Usage Examples:**

```javascript
import { unaryMinus, complex } from 'mathjs';

unaryMinus(5);                    // -5
unaryMinus(-3);                   // 3
unaryMinus(complex(3, 4));        // Complex {re: -3, im: -4}
```

### unaryPlus

Applies unary plus operator (converts to number).

```typescript { .api }
function unaryPlus(x: MathType): MathType;
```

**Parameters:**
- `x`: Value to apply unary plus to

**Returns:** Value as numeric type

**Usage Examples:**

```javascript
import { unaryPlus } from 'mathjs';

unaryPlus(5);                     // 5
unaryPlus('10');                  // 10 (string to number)
unaryPlus(true);                  // 1
```

## Modular Arithmetic

### mod

Calculates the modulus (remainder after division).

```typescript { .api }
function mod(x: MathType, y: MathType): MathType;
```

**Parameters:**
- `x`: Dividend
- `y`: Divisor

**Returns:** Remainder of x divided by y

**Usage Examples:**

```javascript
import { mod } from 'mathjs';

mod(10, 3);                       // 1
mod(10.5, 2);                     // 0.5
mod(-10, 3);                      // 2 (follows floored division)

// Works with BigNumber for large integers
mod(bignumber('1e10'), bignumber('7'));
// BigNumber 4
```

### gcd

Calculates the greatest common divisor of two or more values.

```typescript { .api }
function gcd(...args: Array<number | BigNumber | Fraction | Array | Matrix>): number | BigNumber | Fraction | Array | Matrix;
```

**Parameters:**
- `...args`: Two or more integer values

**Returns:** Greatest common divisor

**Usage Examples:**

```javascript
import { gcd } from 'mathjs';

gcd(12, 8);                       // 4
gcd(21, 14, 7);                   // 7
gcd(24, 36, 60);                  // 12

// Works with arrays
gcd([12, 8, 4]);                  // 4
```

### lcm

Calculates the least common multiple of two or more values.

```typescript { .api }
function lcm(...args: Array<number | BigNumber | Fraction | Array | Matrix>): number | BigNumber | Fraction | Array | Matrix;
```

**Parameters:**
- `...args`: Two or more integer values

**Returns:** Least common multiple

**Usage Examples:**

```javascript
import { lcm } from 'mathjs';

lcm(4, 6);                        // 12
lcm(3, 5, 15);                    // 15
lcm(12, 18, 24);                  // 72

// Works with arrays
lcm([4, 6, 8]);                   // 24
```

### xgcd

Calculates the extended greatest common divisor using the Extended Euclidean Algorithm.

```typescript { .api }
function xgcd(a: number | BigNumber, b: number | BigNumber): Array<number | BigNumber>;
```

**Parameters:**
- `a`: First integer value
- `b`: Second integer value

**Returns:** Array [gcd, x, y] where gcd = a*x + b*y (Bézout coefficients)

**Usage Examples:**

```javascript
import { xgcd } from 'mathjs';

// Find gcd and Bézout coefficients
xgcd(12, 8);                      // [4, -1, 2]
// Verification: 12*(-1) + 8*2 = 4

xgcd(21, 14);                     // [7, 1, -1]
// Verification: 21*1 + 14*(-1) = 7

// Useful for solving linear Diophantine equations
xgcd(35, 15);                     // [5, 1, -2]
```

### invmod

Calculates the modular multiplicative inverse of a modulo b.

```typescript { .api }
function invmod(a: number | BigNumber, b: number | BigNumber): number | BigNumber;
```

**Parameters:**
- `a`: Value to find inverse of
- `b`: Modulus

**Returns:** Modular multiplicative inverse, or throws error if it doesn't exist

**Usage Examples:**

```javascript
import { invmod, mod } from 'mathjs';

// Find x such that (a * x) mod b = 1
invmod(3, 11);                    // 4
// Verification: (3 * 4) mod 11 = 1

invmod(7, 26);                    // 15
// Verification: (7 * 15) mod 26 = 1

// Useful in cryptography (RSA, etc.)
const inverse = invmod(17, 43);   // 38
mod(17 * inverse, 43);            // 1
```

## Vector and Matrix Norms

### norm

Calculates the norm (length/magnitude) of a vector or matrix.

```typescript { .api }
function norm(x: number | BigNumber | Complex | Array | Matrix, p?: number | BigNumber | string): number | BigNumber;
```

**Parameters:**
- `x`: Value, vector, or matrix
- `p`: Norm type (default: 2 for Euclidean norm)
  - For vectors: p-norm where p is a positive number, or 'inf' for infinity norm
  - For matrices: Frobenius norm (default), 'fro', or 'inf'

**Returns:** Calculated norm

**Usage Examples:**

```javascript
import { norm } from 'mathjs';

// Euclidean norm (L2 norm, default)
norm([3, 4]);                     // 5 (sqrt(3^2 + 4^2))

// Manhattan norm (L1 norm)
norm([3, 4], 1);                  // 7 (|3| + |4|)

// Infinity norm (max absolute value)
norm([3, -4], 'inf');             // 4

// p-norm for arbitrary p
norm([1, 2, 3], 3);               // 3.3019... (cube root of 1^3+2^3+3^3)

// Matrix Frobenius norm
norm([[1, 2], [3, 4]]);           // 5.477... (sqrt of sum of squares)

// Complex numbers
norm(complex(3, 4));              // 5
```

### hypot

Calculates the hypotenuse (sqrt of sum of squares) of the arguments.

```typescript { .api }
function hypot(...args: Array<number | BigNumber>): number | BigNumber;
```

**Parameters:**
- `...args`: Two or more numeric values

**Returns:** Hypotenuse sqrt(a^2 + b^2 + c^2 + ...)

**Usage Examples:**

```javascript
import { hypot } from 'mathjs';

// Pythagorean theorem
hypot(3, 4);                      // 5

// Works with more than 2 arguments
hypot(3, 4, 5);                   // 7.0710... (sqrt(50))

// Useful for calculating 3D distances
hypot(1, 2, 2);                   // 3 (distance from origin)
```

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathScalarType = MathNumericType | Unit;
type MathArray<T> = T[] | Array<MathArray<T>>;
type MathCollection = MathArray<any> | Matrix;
type MathType = MathScalarType | MathCollection;
```
