# Trigonometry Functions

Math.js provides 24 comprehensive trigonometric functions supporting all numeric types including numbers, BigNumber, Complex, and matrices. These functions handle angles in radians by default and support complex number calculations for extended domains.

## Core Imports

```javascript { .api }
import {
  sin, cos, tan, asin, acos, atan, atan2,
  sinh, cosh, tanh, asinh, acosh, atanh,
  cot, csc, sec, acot, acsc, asec,
  coth, csch, sech, acoth, acsch, asech
} from 'mathjs';
```

## Basic Trigonometric Functions

### sin

Calculates the sine of a value.

```typescript { .api }
function sin(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Angle in radians (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Sine of x

**Usage Examples:**

```javascript
import { sin, pi, complex, unit, matrix } from 'mathjs';

// Numbers (radians)
sin(0);                    // 0
sin(pi / 2);              // 1
sin(pi);                  // ~0 (1.2e-16)

// Angle units
sin(unit('90 deg'));      // 1
sin(unit('45 deg'));      // 0.7071... (√2/2)

// Complex numbers
sin(complex(2, 3));
// Complex {re: 9.154..., im: -4.168...}

// Matrices - element-wise
sin([[0, pi/2], [pi, 3*pi/2]]);
// [[0, 1], [~0, -1]]
```

### cos

Calculates the cosine of a value.

```typescript { .api }
function cos(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Angle in radians (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Cosine of x

**Usage Examples:**

```javascript
import { cos, pi, complex, unit } from 'mathjs';

// Numbers (radians)
cos(0);                   // 1
cos(pi / 2);             // ~0 (6.1e-17)
cos(pi);                 // -1

// Angle units
cos(unit('0 deg'));      // 1
cos(unit('60 deg'));     // 0.5

// Complex numbers
cos(complex(0, 1));      // Complex {re: 1.543..., im: 0}
```

### tan

Calculates the tangent of a value.

```typescript { .api }
function tan(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Angle in radians (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Tangent of x

**Usage Examples:**

```javascript
import { tan, pi, complex, unit } from 'mathjs';

// Numbers (radians)
tan(0);                   // 0
tan(pi / 4);             // 1
tan(pi / 3);             // 1.732... (√3)

// Angle units
tan(unit('45 deg'));     // 1
tan(unit('30 deg'));     // 0.577... (1/√3)

// Complex numbers
tan(complex(1, 1));      // Complex {re: 0.271..., im: 1.083...}

// Matrices
tan([0, pi/4, pi/6]);    // [0, 1, 0.577...]
```

## Inverse Trigonometric Functions

### asin

Calculates the inverse sine (arcsine) of a value.

```typescript { .api }
function asin(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value in range [-1, 1] for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Angle in radians, in range [-π/2, π/2] for real inputs

**Usage Examples:**

```javascript
import { asin, pi, complex } from 'mathjs';

// Numbers
asin(0);                  // 0
asin(1);                  // π/2 (1.5707...)
asin(0.5);                // π/6 (0.5235...)
asin(-1);                 // -π/2 (-1.5707...)

// Complex numbers (extends domain beyond [-1, 1])
asin(2);                  // Complex {re: 1.5707..., im: 1.316...}
asin(complex(2, 3));      // Complex {re: 0.570..., im: 1.983...}

// Matrices
asin([0, 0.5, 1]);        // [0, 0.5235..., 1.5707...]
```

### acos

Calculates the inverse cosine (arccosine) of a value.

```typescript { .api }
function acos(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value in range [-1, 1] for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Angle in radians, in range [0, π] for real inputs

**Usage Examples:**

```javascript
import { acos, pi, complex } from 'mathjs';

// Numbers
acos(1);                  // 0
acos(0);                  // π/2 (1.5707...)
acos(-1);                 // π (3.1415...)
acos(0.5);                // π/3 (1.0471...)

// Complex numbers (extends domain)
acos(2);                  // Complex {re: 0, im: -1.316...}
acos(complex(1, 1));      // Complex {re: 0.904..., im: -1.061...}

// Matrices
acos([1, 0.5, 0]);        // [0, 1.0471..., 1.5707...]
```

### atan

Calculates the inverse tangent (arctangent) of a value.

```typescript { .api }
function atan(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any value (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Angle in radians, in range (-π/2, π/2) for real inputs

**Usage Examples:**

```javascript
import { atan, pi, complex } from 'mathjs';

// Numbers
atan(0);                  // 0
atan(1);                  // π/4 (0.7853...)
atan(-1);                 // -π/4 (-0.7853...)
atan(Infinity);           // π/2 (1.5707...)

// Complex numbers
atan(complex(1, 1));      // Complex {re: 1.017..., im: 0.402...}

// Matrices
atan([0, 1, -1]);         // [0, 0.7853..., -0.7853...]
```

### atan2

Calculates the two-argument arctangent, determining the quadrant correctly.

```typescript { .api }
function atan2(y: number | BigNumber | Array | Matrix, x: number | BigNumber | Array | Matrix): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `y`: Y-coordinate (number, BigNumber, Matrix, or Array)
- `x`: X-coordinate (number, BigNumber, Matrix, or Array)

**Returns:** Angle in radians from the positive x-axis to the point (x, y), in range (-π, π]

**Usage Examples:**

```javascript
import { atan2, pi } from 'mathjs';

// Cartesian to polar angle conversion
atan2(0, 1);              // 0 (positive x-axis)
atan2(1, 0);              // π/2 (positive y-axis)
atan2(0, -1);             // π (negative x-axis)
atan2(-1, 0);             // -π/2 (negative y-axis)

// Quadrant detection
atan2(1, 1);              // π/4 (Q1: 0.7853...)
atan2(1, -1);             // 3π/4 (Q2: 2.3561...)
atan2(-1, -1);            // -3π/4 (Q3: -2.3561...)
atan2(-1, 1);             // -π/4 (Q4: -0.7853...)

// Matrices - element-wise
atan2([1, 1, -1], [1, -1, 1]);
// [0.7853..., 2.3561..., -0.7853...]
```

## Hyperbolic Functions

### sinh

Calculates the hyperbolic sine of a value.

```typescript { .api }
function sinh(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any value (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Hyperbolic sine of x: (e^x - e^(-x)) / 2

**Usage Examples:**

```javascript
import { sinh, complex } from 'mathjs';

// Numbers
sinh(0);                  // 0
sinh(1);                  // 1.1752...
sinh(-1);                 // -1.1752...

// Complex numbers
sinh(complex(1, 1));      // Complex {re: 0.634..., im: 1.298...}

// Matrices
sinh([0, 1, 2]);          // [0, 1.1752..., 3.6268...]
```

### cosh

Calculates the hyperbolic cosine of a value.

```typescript { .api }
function cosh(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any value (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Hyperbolic cosine of x: (e^x + e^(-x)) / 2

**Usage Examples:**

```javascript
import { cosh, complex } from 'mathjs';

// Numbers
cosh(0);                  // 1
cosh(1);                  // 1.5430...
cosh(-1);                 // 1.5430... (even function)

// Complex numbers
cosh(complex(0, pi));     // -1

// Matrices
cosh([0, 1, 2]);          // [1, 1.5430..., 3.7621...]
```

### tanh

Calculates the hyperbolic tangent of a value.

```typescript { .api }
function tanh(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any value (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Hyperbolic tangent of x: sinh(x) / cosh(x)

**Usage Examples:**

```javascript
import { tanh, complex } from 'mathjs';

// Numbers
tanh(0);                  // 0
tanh(1);                  // 0.7615...
tanh(Infinity);           // 1
tanh(-Infinity);          // -1

// Complex numbers
tanh(complex(1, 1));      // Complex {re: 1.083..., im: 0.271...}

// Matrices
tanh([0, 1, 2]);          // [0, 0.7615..., 0.9640...]
```

## Inverse Hyperbolic Functions

### asinh

Calculates the inverse hyperbolic sine (area hyperbolic sine) of a value.

```typescript { .api }
function asinh(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any value (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Inverse hyperbolic sine of x

**Usage Examples:**

```javascript
import { asinh, complex } from 'mathjs';

// Numbers
asinh(0);                 // 0
asinh(1);                 // 0.8813...
asinh(-1);                // -0.8813...

// Complex numbers
asinh(complex(1, 1));     // Complex {re: 1.061..., im: 0.666...}

// Matrices
asinh([0, 1, 2]);         // [0, 0.8813..., 1.4436...]
```

### acosh

Calculates the inverse hyperbolic cosine (area hyperbolic cosine) of a value.

```typescript { .api }
function acosh(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value >= 1 for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Inverse hyperbolic cosine of x (non-negative for real inputs)

**Usage Examples:**

```javascript
import { acosh, complex } from 'mathjs';

// Numbers
acosh(1);                 // 0
acosh(2);                 // 1.3169...
acosh(10);                // 2.9932...

// Complex numbers (extends domain to x < 1)
acosh(0.5);               // Complex {re: 0, im: 1.0471...}
acosh(complex(1, 1));     // Complex {re: 1.061..., im: 0.904...}

// Matrices
acosh([1, 2, 3]);         // [0, 1.3169..., 1.7627...]
```

### atanh

Calculates the inverse hyperbolic tangent (area hyperbolic tangent) of a value.

```typescript { .api }
function atanh(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value in range (-1, 1) for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Inverse hyperbolic tangent of x

**Usage Examples:**

```javascript
import { atanh, complex } from 'mathjs';

// Numbers
atanh(0);                 // 0
atanh(0.5);               // 0.5493...
atanh(-0.5);              // -0.5493...

// Complex numbers (extends domain)
atanh(2);                 // Complex {re: 0.5493..., im: -1.5707...}
atanh(complex(1, 1));     // Complex {re: 0.402..., im: 1.017...}

// Matrices
atanh([0, 0.5, -0.5]);    // [0, 0.5493..., -0.5493...]
```

## Hyperbolic Reciprocal Functions

### coth

Calculates the hyperbolic cotangent of a value.

```typescript { .api }
function coth(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any non-zero value (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Hyperbolic cotangent of x: cosh(x) / sinh(x)

**Usage Examples:**

```javascript
import { coth, complex } from 'mathjs';

// Numbers
coth(1);                  // 1.3130...
coth(-1);                 // -1.3130...
coth(2);                  // 1.0373...

// Complex numbers
coth(complex(1, 1));      // Complex {re: 0.868..., im: -0.217...}

// Matrices
coth([1, 2, 3]);          // [1.3130..., 1.0373..., 1.0049...]
```

### csch

Calculates the hyperbolic cosecant of a value.

```typescript { .api }
function csch(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any non-zero value (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Hyperbolic cosecant of x: 1 / sinh(x)

**Usage Examples:**

```javascript
import { csch, complex } from 'mathjs';

// Numbers
csch(1);                  // 0.8509...
csch(-1);                 // -0.8509...
csch(2);                  // 0.2757...

// Complex numbers
csch(complex(1, 1));      // Complex {re: 0.303..., im: -0.621...}

// Matrices
csch([1, 2, 0.5]);        // [0.8509..., 0.2757..., 1.9190...]
```

### sech

Calculates the hyperbolic secant of a value.

```typescript { .api }
function sech(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any value (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Hyperbolic secant of x: 1 / cosh(x)

**Usage Examples:**

```javascript
import { sech, complex } from 'mathjs';

// Numbers
sech(0);                  // 1
sech(1);                  // 0.6480...
sech(-1);                 // 0.6480... (even function)

// Complex numbers
sech(complex(1, 1));      // Complex {re: 0.498..., im: -0.591...}

// Matrices
sech([0, 1, 2]);          // [1, 0.6480..., 0.2658...]
```

## Reciprocal Trigonometric Functions

### cot

Calculates the cotangent of a value.

```typescript { .api }
function cot(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Angle in radians (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Cotangent of x: 1 / tan(x)

**Usage Examples:**

```javascript
import { cot, pi, complex, unit } from 'mathjs';

// Numbers (radians)
cot(pi / 4);              // 1
cot(pi / 6);              // 1.732... (√3)
cot(pi / 3);              // 0.577... (1/√3)

// Angle units
cot(unit('45 deg'));      // 1
cot(unit('30 deg'));      // 1.732...

// Complex numbers
cot(complex(1, 1));       // Complex {re: 0.217..., im: -0.868...}

// Matrices
cot([pi/4, pi/3, pi/6]);  // [1, 0.577..., 1.732...]
```

### csc

Calculates the cosecant of a value.

```typescript { .api }
function csc(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Angle in radians (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Cosecant of x: 1 / sin(x)

**Usage Examples:**

```javascript
import { csc, pi, complex, unit } from 'mathjs';

// Numbers (radians)
csc(pi / 2);              // 1
csc(pi / 6);              // 2
csc(pi / 4);              // 1.414... (√2)

// Angle units
csc(unit('90 deg'));      // 1
csc(unit('30 deg'));      // 2

// Complex numbers
csc(complex(1, 1));       // Complex {re: 0.621..., im: -0.303...}

// Matrices
csc([pi/6, pi/4, pi/2]);  // [2, 1.414..., 1]
```

### sec

Calculates the secant of a value.

```typescript { .api }
function sec(x: number | BigNumber | Complex | Unit | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Angle in radians (number, BigNumber, Complex, Unit, Matrix, or Array)

**Returns:** Secant of x: 1 / cos(x)

**Usage Examples:**

```javascript
import { sec, pi, complex, unit } from 'mathjs';

// Numbers (radians)
sec(0);                   // 1
sec(pi / 3);              // 2
sec(pi / 4);              // 1.414... (√2)

// Angle units
sec(unit('0 deg'));       // 1
sec(unit('60 deg'));      // 2

// Complex numbers
sec(complex(1, 1));       // Complex {re: 0.498..., im: 0.591...}

// Matrices
sec([0, pi/4, pi/3]);     // [1, 1.414..., 2]
```

## Inverse Reciprocal Trigonometric Functions

### acot

Calculates the inverse cotangent (arccotangent) of a value.

```typescript { .api }
function acot(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any value (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Angle in radians, in range (0, π) for real inputs

**Usage Examples:**

```javascript
import { acot, pi, complex } from 'mathjs';

// Numbers
acot(0);                  // π/2 (1.5707...)
acot(1);                  // π/4 (0.7853...)
acot(-1);                 // 3π/4 (2.3561...)
acot(Infinity);           // 0

// Complex numbers
acot(complex(1, 1));      // Complex {re: 0.553..., im: -0.402...}

// Matrices
acot([0, 1, -1]);         // [1.5707..., 0.7853..., 2.3561...]
```

### acsc

Calculates the inverse cosecant (arccosecant) of a value.

```typescript { .api }
function acsc(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value where |x| >= 1 for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Angle in radians, in range [-π/2, π/2] excluding 0 for real inputs

**Usage Examples:**

```javascript
import { acsc, pi, complex } from 'mathjs';

// Numbers
acsc(1);                  // π/2 (1.5707...)
acsc(-1);                 // -π/2 (-1.5707...)
acsc(2);                  // π/6 (0.5235...)
acsc(-2);                 // -π/6 (-0.5235...)

// Complex numbers (extends domain to |x| < 1)
acsc(0.5);                // Complex {re: 1.5707..., im: -1.316...}
acsc(complex(1, 1));      // Complex {re: 0.452..., im: -0.530...}

// Matrices
acsc([1, 2, -2]);         // [1.5707..., 0.5235..., -0.5235...]
```

### asec

Calculates the inverse secant (arcsecant) of a value.

```typescript { .api }
function asec(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value where |x| >= 1 for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Angle in radians, in range [0, π] excluding π/2 for real inputs

**Usage Examples:**

```javascript
import { asec, pi, complex } from 'mathjs';

// Numbers
asec(1);                  // 0
asec(-1);                 // π (3.1415...)
asec(2);                  // π/3 (1.0471...)
asec(-2);                 // 2π/3 (2.0943...)

// Complex numbers (extends domain to |x| < 1)
asec(0.5);                // Complex {re: 0, im: 1.316...}
asec(complex(1, 1));      // Complex {re: 1.118..., im: 0.530...}

// Matrices
asec([1, 2, -1]);         // [0, 1.0471..., 3.1415...]
```

## Inverse Reciprocal Hyperbolic Functions

### acoth

Calculates the inverse hyperbolic cotangent (area hyperbolic cotangent) of a value.

```typescript { .api }
function acoth(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value where |x| > 1 for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Inverse hyperbolic cotangent of x

**Usage Examples:**

```javascript
import { acoth, complex } from 'mathjs';

// Numbers
acoth(2);                 // 0.5493...
acoth(-2);                // -0.5493...
acoth(10);                // 0.1003...

// Complex numbers (extends domain to |x| <= 1)
acoth(0.5);               // Complex {re: 0.5493..., im: -1.5707...}
acoth(complex(1, 1));     // Complex {re: 0.402..., im: -0.553...}

// Matrices
acoth([2, 3, -2]);        // [0.5493..., 0.3465..., -0.5493...]
```

### acsch

Calculates the inverse hyperbolic cosecant (area hyperbolic cosecant) of a value.

```typescript { .api }
function acsch(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Any non-zero value (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Inverse hyperbolic cosecant of x

**Usage Examples:**

```javascript
import { acsch, complex } from 'mathjs';

// Numbers
acsch(1);                 // 0.8813...
acsch(-1);                // -0.8813...
acsch(2);                 // 0.4812...
acsch(0.5);               // 1.4436...

// Complex numbers
acsch(complex(1, 1));     // Complex {re: 0.530..., im: -0.452...}

// Matrices
acsch([1, 2, 0.5]);       // [0.8813..., 0.4812..., 1.4436...]
```

### asech

Calculates the inverse hyperbolic secant (area hyperbolic secant) of a value.

```typescript { .api }
function asech(x: number | BigNumber | Complex | Array | Matrix): number | BigNumber | Complex | Array | Matrix;
```

**Parameters:**
- `x`: Value in range (0, 1] for real results (number, BigNumber, Complex, Matrix, or Array)

**Returns:** Inverse hyperbolic secant of x (non-negative for real inputs)

**Usage Examples:**

```javascript
import { asech, complex } from 'mathjs';

// Numbers
asech(1);                 // 0
asech(0.5);               // 1.3169...
asech(0.1);               // 2.9932...

// Complex numbers (extends domain)
asech(2);                 // Complex {re: 0, im: 1.0471...}
asech(complex(1, 1));     // Complex {re: 0.530..., im: 1.118...}

// Matrices
asech([1, 0.5, 0.1]);     // [0, 1.3169..., 2.9932...]
```

## Common Type Definitions

```typescript { .api }
// Supported input types
type MathType = number | BigNumber | Complex | Fraction | Unit | Array | Matrix;

// Complex number type
interface Complex {
  re: number;  // Real part
  im: number;  // Imaginary part
}

// BigNumber for arbitrary precision
class BigNumber {
  constructor(value: string | number);
  // Arbitrary precision decimal number
}

// Unit for angle conversion
class Unit {
  constructor(value: number | string, unit?: string);
  // Supports 'deg', 'rad', 'grad', 'cycle', 'arcsec', 'arcmin'
}

// Matrix types
type Matrix = DenseMatrix | SparseMatrix;
```

## Angle Units

All trigonometric functions accept angles in radians by default. Use the Unit type for automatic conversion:

```javascript
import { sin, cos, tan, unit } from 'mathjs';

// Degrees
sin(unit('90 deg'));      // 1
cos(unit('180 deg'));     // -1
tan(unit('45 deg'));      // 1

// Gradians
sin(unit('100 grad'));    // 1

// Other units
sin(unit('0.25 cycle')); // 1 (1 cycle = 2π rad)
```

## Complex Number Support

All trigonometric functions support complex number inputs, extending their domains beyond real numbers:

```javascript
import { sin, cos, asin, acos, complex } from 'mathjs';

// Trigonometric functions with complex inputs
sin(complex(1, 2));
// Complex {re: 3.165..., im: 1.959...}

cos(complex(1, 2));
// Complex {re: 2.032..., im: -3.051...}

// Inverse functions can return complex results for out-of-domain real inputs
asin(2);                  // Complex (extends beyond [-1, 1])
acos(2);                  // Complex (extends beyond [-1, 1])
```

## Matrix Operations

All trigonometric functions support element-wise operations on matrices and arrays:

```javascript
import { sin, cos, tan, matrix, pi } from 'mathjs';

// Arrays
sin([0, pi/6, pi/4, pi/3, pi/2]);
// [0, 0.5, 0.7071..., 0.8660..., 1]

// Dense matrices
const angles = matrix([[0, pi/4], [pi/2, pi]]);
sin(angles);
// Matrix [[0, 0.7071...], [1, ~0]]

// Works with all 24 trigonometric functions
cos([0, pi/3, pi/2]);
tan([0, pi/4, pi/3]);
sinh([0, 1, 2]);
acosh([1, 2, 3]);
```
