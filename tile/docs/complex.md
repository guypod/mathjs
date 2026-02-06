# Complex Numbers

Operations for complex number arithmetic including creation, property extraction, and arithmetic operations. All standard math functions support complex numbers.

## Capabilities

### Complex Number Creation

Create complex numbers from real and imaginary parts or polar form.

```javascript { .api }
/**
 * Create a complex number from real and imaginary parts
 * @param re - Real part (default: 0)
 * @param im - Imaginary part (default: 0)
 * @returns Complex number
 */
function complex(re?: number, im?: number): Complex

/**
 * Create a complex number from polar coordinates
 * @param arg - Object with r (magnitude) and phi (angle in radians)
 * @returns Complex number
 */
function complex(arg: { r: number; phi: number }): Complex
```

**Usage Examples:**

```javascript
import { complex, evaluate } from 'mathjs'

// Cartesian form (a + bi)
complex(3, 4)              // 3 + 4i
complex(2)                 // 2 + 0i
complex(0, 1)              // i

// Polar form (r * e^(iφ))
complex({ r: 5, phi: Math.PI / 4 })  // 3.536 + 3.536i

// Via evaluate
evaluate('complex(3, 4)')  // 3 + 4i
evaluate('i')              // 0 + 1i
```

### Complex Number Properties

Extract real part, imaginary part, magnitude, and phase.

```javascript { .api }
/**
 * Get the real part of a complex number
 * @param x - Complex number or real value
 * @returns Real part
 */
function re(x: Complex): number
function re(x: number | BigNumber): number | BigNumber
function re(x: MathCollection): MathCollection

/**
 * Get the imaginary part of a complex number
 * @param x - Complex number or real value
 * @returns Imaginary part
 */
function im(x: Complex): number
function im(x: number | BigNumber): number | BigNumber
function im(x: MathCollection): MathCollection

/**
 * Get the argument (phase angle) of a complex number
 * @param x - Complex number
 * @returns Angle in radians [-π, π]
 */
function arg(x: Complex): number
function arg(x: number | BigNumber): number | BigNumber
function arg(x: MathCollection): MathCollection

/**
 * Get the complex conjugate
 * @param x - Complex number
 * @returns Complex conjugate (real - imag*i)
 */
function conj(x: Complex): Complex
function conj(x: number | BigNumber): number | BigNumber
function conj(x: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { complex, re, im, arg, conj, abs } from 'mathjs'

const z = complex(3, 4)

re(z)                  // 3
im(z)                  // 4
abs(z)                 // 5 (magnitude: √(3² + 4²))
arg(z)                 // 0.927... (atan2(4, 3))
conj(z)                // 3 - 4i

// For real numbers
re(5)                  // 5
im(5)                  // 0
arg(-1)                // π (Math.PI)
```

### Complex Arithmetic

All standard arithmetic operations support complex numbers.

```javascript { .api }
/**
 * Complex addition (use add function)
 * @param x - First complex number
 * @param y - Second complex number
 * @returns Sum
 */
function add(x: Complex, y: Complex): Complex

/**
 * Complex subtraction (use subtract function)
 * @param x - First complex number
 * @param y - Second complex number
 * @returns Difference
 */
function subtract(x: Complex, y: Complex): Complex

/**
 * Complex multiplication (use multiply function)
 * @param x - First complex number
 * @param y - Second complex number
 * @returns Product
 */
function multiply(x: Complex, y: Complex): Complex

/**
 * Complex division (use divide function)
 * @param x - Numerator
 * @param y - Denominator
 * @returns Quotient
 */
function divide(x: Complex, y: Complex): Complex

/**
 * Complex power (use pow function)
 * @param x - Base
 * @param y - Exponent
 * @returns x raised to power y
 */
function pow(x: Complex, y: number | Complex): Complex

/**
 * Complex square root (use sqrt function)
 * @param x - Complex number
 * @returns Principal square root
 */
function sqrt(x: Complex): Complex
```

**Usage Examples:**

```javascript
import { complex, add, subtract, multiply, divide, pow, sqrt } from 'mathjs'

const z1 = complex(3, 4)   // 3 + 4i
const z2 = complex(1, 2)   // 1 + 2i

add(z1, z2)                // 4 + 6i
subtract(z1, z2)           // 2 + 2i
multiply(z1, z2)           // -5 + 10i
divide(z1, z2)             // 2.2 + 0.4i
pow(z1, 2)                 // -7 + 24i
sqrt(complex(-1, 0))       // i

// Operations with real numbers
add(z1, 5)                 // 8 + 4i
multiply(z1, 2)            // 6 + 8i
```

### Complex Trigonometry

Trigonometric functions with complex arguments.

```javascript { .api }
/**
 * Complex sine (use sin function)
 */
function sin(x: Complex): Complex

/**
 * Complex cosine (use cos function)
 */
function cos(x: Complex): Complex

/**
 * Complex tangent (use tan function)
 */
function tan(x: Complex): Complex

/**
 * Complex hyperbolic sine (use sinh function)
 */
function sinh(x: Complex): Complex

/**
 * Complex hyperbolic cosine (use cosh function)
 */
function cosh(x: Complex): Complex

/**
 * Complex hyperbolic tangent (use tanh function)
 */
function tanh(x: Complex): Complex
```

**Usage Examples:**

```javascript
import { complex, sin, cos, exp, log } from 'mathjs'

const z = complex(0, 1)    // i

sin(z)                     // 0 + 1.175i (sinh(1)i)
cos(z)                     // 1.543 + 0i (cosh(1))
exp(z)                     // 0.540 + 0.841i (e^i)

// Euler's formula: e^(iπ) = -1
exp(complex(0, Math.PI))   // -1 + 0i

// Complex logarithm
log(complex(-1, 0))        // 0 + πi
```

### Roots and Powers

Calculate nth roots and complex powers.

```javascript { .api }
/**
 * Calculate all nth roots of a complex number
 * @param x - Complex number or real number
 * @param n - Root degree (default: 2)
 * @returns Array of all nth roots
 */
function nthRoots(x: number | Complex, n?: number): Complex[]

/**
 * Calculate cube root with all roots option
 * @param x - Complex number
 * @param allRoots - If true, return all three roots
 * @returns Cube root or array of all roots
 */
function cbrt(x: Complex, allRoots?: boolean): Complex | Complex[]
```

**Usage Examples:**

```javascript
import { nthRoots, cbrt, complex } from 'mathjs'

// All square roots of -1
nthRoots(-1, 2)            // [i, -i]

// All cube roots of 1
nthRoots(1, 3)             // [1, -0.5+0.866i, -0.5-0.866i]

// All cube roots of 8
nthRoots(8, 3)             // [2, -1+1.732i, -1-1.732i]

// Cube root with all roots
cbrt(complex(1, 0), true)  // [1, -0.5+0.866i, -0.5-0.866i]
```

### Complex Number Interface

The Complex type structure.

```javascript { .api }
interface Complex {
  /**
   * Real part
   */
  re: number

  /**
   * Imaginary part
   */
  im: number
}
```

**Usage Examples:**

```javascript
import { complex } from 'mathjs'

const z = complex(3, 4)

// Access properties directly
console.log(z.re)          // 3
console.log(z.im)          // 4

// Type checking
import { isComplex } from 'mathjs'
isComplex(z)               // true
isComplex(5)               // false
```

## Working with Complex Numbers

### Automatic Complex Results

Many functions automatically return complex results when appropriate:

```javascript
import { sqrt, log, asin } from 'mathjs'

sqrt(-4)                   // 2i
log(-1)                    // πi
asin(2)                    // 1.571 - 1.317i
```

### Complex Numbers in Expressions

Complex numbers can be used in string expressions:

```javascript
import { evaluate } from 'mathjs'

evaluate('i')                        // i
evaluate('2 + 3i')                   // 2 + 3i
evaluate('(2 + 3i) * (1 - i)')       // 5 + i
evaluate('sqrt(-1)')                 // i
evaluate('e^(i * pi)')               // -1 (Euler's formula)
```

### Magnitude and Phase

Convert between Cartesian and polar forms:

```javascript
import { complex, abs, arg, evaluate } from 'mathjs'

const z = complex(3, 4)

// Get polar form
const magnitude = abs(z)         // 5
const phase = arg(z)             // 0.927 radians

// Create from polar
const w = complex({ r: 5, phi: 0.927 })  // ≈ 3 + 4i

// Via evaluate
evaluate('abs(3 + 4i)')          // 5
evaluate('arg(3 + 4i)')          // 0.927...
```

### Complex Matrices

Complex numbers work seamlessly with matrices:

```javascript
import { matrix, complex, multiply, det } from 'mathjs'

const A = matrix([
  [complex(1, 0), complex(0, 1)],
  [complex(0, -1), complex(1, 0)]
])

det(A)                           // 1 + 0i
multiply(A, A)                   // Matrix multiplication
```

## Mathematical Identities

Common complex number identities:

```javascript
import { complex, add, multiply, conj, abs, pow, exp, evaluate } from 'mathjs'

const z = complex(3, 4)
const zConj = conj(z)

// |z|² = z × conj(z)
multiply(z, zConj)               // 25 + 0i
pow(abs(z), 2)                   // 25

// Euler's formula: e^(ix) = cos(x) + i⋅sin(x)
evaluate('e^(i * pi/2)')         // i
evaluate('cos(pi/2) + i*sin(pi/2)')  // i

// De Moivre's theorem
pow(complex({ r: 1, phi: Math.PI/4 }), 2)  // ≈ i
```
