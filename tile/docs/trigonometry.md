# Trigonometric Functions

Complete trigonometric, inverse trigonometric, hyperbolic, and inverse hyperbolic functions. All functions support angles in radians by default and work with units (e.g., degrees).

## Capabilities

### Standard Trigonometric Functions

Basic trigonometric functions for angle calculations.

```javascript { .api }
/**
 * Calculate the sine
 * @param x - Angle in radians (or with unit)
 * @returns Sine of x
 */
function sin(x: number | Unit): number
function sin(x: BigNumber): BigNumber
function sin(x: Complex): Complex
function sin(x: MathCollection): MathCollection

/**
 * Calculate the cosine
 * @param x - Angle in radians (or with unit)
 * @returns Cosine of x
 */
function cos(x: number | Unit): number
function cos(x: BigNumber): BigNumber
function cos(x: Complex): Complex
function cos(x: MathCollection): MathCollection

/**
 * Calculate the tangent
 * @param x - Angle in radians (or with unit)
 * @returns Tangent of x
 */
function tan(x: number | Unit): number
function tan(x: BigNumber): BigNumber
function tan(x: Complex): Complex
function tan(x: MathCollection): MathCollection

/**
 * Calculate the cotangent
 * @param x - Angle in radians (or with unit)
 * @returns Cotangent of x
 */
function cot(x: number | Unit): number
function cot(x: BigNumber): BigNumber
function cot(x: Complex): Complex
function cot(x: MathCollection): MathCollection

/**
 * Calculate the secant
 * @param x - Angle in radians (or with unit)
 * @returns Secant of x
 */
function sec(x: number | Unit): number
function sec(x: BigNumber): BigNumber
function sec(x: Complex): Complex
function sec(x: MathCollection): MathCollection

/**
 * Calculate the cosecant
 * @param x - Angle in radians (or with unit)
 * @returns Cosecant of x
 */
function csc(x: number | Unit): number
function csc(x: BigNumber): BigNumber
function csc(x: Complex): Complex
function csc(x: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { sin, cos, tan, unit, evaluate } from 'mathjs'

sin(0)               // 0
sin(Math.PI / 2)     // 1
cos(Math.PI)         // -1
tan(Math.PI / 4)     // 1

// With units
sin(unit('90 deg'))  // 1
cos(unit('180 deg')) // -1

// Via evaluate
evaluate('sin(45 deg)') // 0.707...
evaluate('cos(pi)')     // -1
```

### Inverse Trigonometric Functions

Inverse functions returning angles.

```javascript { .api }
/**
 * Calculate the arcsine (inverse sine)
 * @param x - Input value [-1, 1]
 * @returns Angle in radians
 */
function asin(x: number): number | Complex
function asin(x: BigNumber): BigNumber
function asin(x: Complex): Complex
function asin(x: MathCollection): MathCollection

/**
 * Calculate the arccosine (inverse cosine)
 * @param x - Input value [-1, 1]
 * @returns Angle in radians
 */
function acos(x: number): number | Complex
function acos(x: BigNumber): BigNumber
function acos(x: Complex): Complex
function acos(x: MathCollection): MathCollection

/**
 * Calculate the arctangent (inverse tangent)
 * @param x - Input value
 * @returns Angle in radians
 */
function atan(x: number): number
function atan(x: BigNumber): BigNumber
function atan(x: Complex): Complex
function atan(x: MathCollection): MathCollection

/**
 * Calculate the two-argument arctangent (atan2)
 * @param y - Y coordinate
 * @param x - X coordinate
 * @returns Angle in radians from -π to π
 */
function atan2(y: number, x: number): number
function atan2(y: MathCollection, x: MathCollection): MathCollection

/**
 * Calculate the arccotangent (inverse cotangent)
 * @param x - Input value
 * @returns Angle in radians
 */
function acot(x: number): number
function acot(x: BigNumber): BigNumber
function acot(x: Complex): Complex
function acot(x: MathCollection): MathCollection

/**
 * Calculate the arcsecant (inverse secant)
 * @param x - Input value (|x| >= 1)
 * @returns Angle in radians
 */
function asec(x: number): number | Complex
function asec(x: BigNumber): BigNumber
function asec(x: Complex): Complex
function asec(x: MathCollection): MathCollection

/**
 * Calculate the arccosecant (inverse cosecant)
 * @param x - Input value (|x| >= 1)
 * @returns Angle in radians
 */
function acsc(x: number): number | Complex
function acsc(x: BigNumber): BigNumber
function acsc(x: Complex): Complex
function acsc(x: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { asin, acos, atan, atan2 } from 'mathjs'

asin(1)          // π/2 (1.5708...)
acos(0)          // π/2 (1.5708...)
atan(1)          // π/4 (0.7854...)
atan2(1, 1)      // π/4 (0.7854...)
atan2(-1, -1)    // -3π/4 (-2.356...)
```

### Hyperbolic Functions

Hyperbolic trigonometric functions.

```javascript { .api }
/**
 * Calculate the hyperbolic sine
 * @param x - Input value
 * @returns Hyperbolic sine of x
 */
function sinh(x: number | Unit): number
function sinh(x: BigNumber): BigNumber
function sinh(x: Complex): Complex
function sinh(x: MathCollection): MathCollection

/**
 * Calculate the hyperbolic cosine
 * @param x - Input value
 * @returns Hyperbolic cosine of x
 */
function cosh(x: number | Unit): number
function cosh(x: BigNumber): BigNumber
function cosh(x: Complex): Complex
function cosh(x: MathCollection): MathCollection

/**
 * Calculate the hyperbolic tangent
 * @param x - Input value
 * @returns Hyperbolic tangent of x
 */
function tanh(x: number | Unit): number
function tanh(x: BigNumber): BigNumber
function tanh(x: Complex): Complex
function tanh(x: MathCollection): MathCollection

/**
 * Calculate the hyperbolic cotangent
 * @param x - Input value
 * @returns Hyperbolic cotangent of x
 */
function coth(x: number | Unit): number
function coth(x: BigNumber): BigNumber
function coth(x: Complex): Complex
function coth(x: MathCollection): MathCollection

/**
 * Calculate the hyperbolic secant
 * @param x - Input value
 * @returns Hyperbolic secant of x
 */
function sech(x: number | Unit): number
function sech(x: BigNumber): BigNumber
function sech(x: Complex): Complex
function sech(x: MathCollection): MathCollection

/**
 * Calculate the hyperbolic cosecant
 * @param x - Input value
 * @returns Hyperbolic cosecant of x
 */
function csch(x: number | Unit): number
function csch(x: BigNumber): BigNumber
function csch(x: Complex): Complex
function csch(x: MathCollection): MathCollection
```

### Inverse Hyperbolic Functions

Inverse hyperbolic functions.

```javascript { .api }
/**
 * Calculate the inverse hyperbolic sine
 * @param x - Input value
 * @returns Inverse hyperbolic sine of x
 */
function asinh(x: number): number
function asinh(x: BigNumber): BigNumber
function asinh(x: Complex): Complex
function asinh(x: MathCollection): MathCollection

/**
 * Calculate the inverse hyperbolic cosine
 * @param x - Input value (x >= 1)
 * @returns Inverse hyperbolic cosine of x
 */
function acosh(x: number): number | Complex
function acosh(x: BigNumber): BigNumber
function acosh(x: Complex): Complex
function acosh(x: MathCollection): MathCollection

/**
 * Calculate the inverse hyperbolic tangent
 * @param x - Input value (|x| < 1)
 * @returns Inverse hyperbolic tangent of x
 */
function atanh(x: number): number | Complex
function atanh(x: BigNumber): BigNumber
function atanh(x: Complex): Complex
function atanh(x: MathCollection): MathCollection

/**
 * Calculate the inverse hyperbolic cotangent
 * @param x - Input value (|x| > 1)
 * @returns Inverse hyperbolic cotangent of x
 */
function acoth(x: number): number
function acoth(x: BigNumber): BigNumber
function acoth(x: Complex): Complex
function acoth(x: MathCollection): MathCollection

/**
 * Calculate the inverse hyperbolic secant
 * @param x - Input value (0 < x <= 1)
 * @returns Inverse hyperbolic secant of x
 */
function asech(x: number): number | Complex
function asech(x: BigNumber): BigNumber
function asech(x: Complex): Complex
function asech(x: MathCollection): MathCollection

/**
 * Calculate the inverse hyperbolic cosecant
 * @param x - Input value (x != 0)
 * @returns Inverse hyperbolic cosecant of x
 */
function acsch(x: number): number
function acsch(x: BigNumber): BigNumber
function acsch(x: Complex): Complex
function acsch(x: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { sinh, cosh, tanh, asinh, acosh, atanh } from 'mathjs'

sinh(0)          // 0
cosh(0)          // 1
tanh(Infinity)   // 1
asinh(0)         // 0
acosh(1)         // 0
atanh(0)         // 0
```
