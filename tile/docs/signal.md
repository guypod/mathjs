# Signal Processing Functions

Math.js provides signal processing functions for analyzing and converting between different representations of linear time-invariant (LTI) systems. These functions are useful for control systems, filter design, and frequency analysis.

## Core Imports

```javascript { .api }
import { zpk2tf, freqz } from 'mathjs';
```

## Capabilities

### Zero-Pole-Gain to Transfer Function Conversion

Convert a system from zero-pole-gain (ZPK) representation to transfer function (TF) representation.

```typescript { .api }
/**
 * Convert zero-pole-gain representation to transfer function coefficients
 * @param z - Array of zeros (roots of numerator)
 * @param p - Array of poles (roots of denominator)
 * @param k - System gain (optional, defaults to 1)
 * @returns Object with numerator (num) and denominator (den) coefficient arrays
 */
function zpk2tf(
  z: number[] | Matrix,
  p: number[] | Matrix,
  k?: number
): { num: number[], den: number[] };
```

**Parameters:**
- `z`: Zeros of the transfer function (roots of numerator polynomial)
- `p`: Poles of the transfer function (roots of denominator polynomial)
- `k`: Gain factor (optional, defaults to 1)

**Returns:** Object containing:
- `num`: Numerator polynomial coefficients (descending powers)
- `den`: Denominator polynomial coefficients (descending powers)

**Usage Examples:**

```javascript
import { zpk2tf } from 'mathjs';

// Simple system with one zero at -1 and one pole at -2
const result1 = zpk2tf([-1], [-2], 1);
// Result: { num: [1, 1], den: [1, 2] }
// Transfer function: (s + 1) / (s + 2)

// System with gain
const result2 = zpk2tf([-1], [-2], 5);
// Result: { num: [5, 5], den: [1, 2] }
// Transfer function: 5(s + 1) / (s + 2)

// Multiple zeros and poles
const result3 = zpk2tf(
  [-1, -3],           // zeros at s=-1 and s=-3
  [-2, -4, -5],       // poles at s=-2, s=-4, s=-5
  1
);
// Result: { num: [1, 4, 3], den: [1, 11, 38, 40] }
// Transfer function: (s^2 + 4s + 3) / (s^3 + 11s^2 + 38s + 40)

// Second-order system
const result4 = zpk2tf(
  [],                 // no zeros
  [-1, -1],           // repeated pole at s=-1
  10
);
// Result: { num: [10], den: [1, 2, 1] }
// Transfer function: 10 / (s^2 + 2s + 1)

// With complex conjugate poles
import { complex } from 'mathjs';
const result5 = zpk2tf(
  [],
  [complex(-1, 2), complex(-1, -2)],  // poles at -1±2j
  1
);
// Result: { num: [1], den: [1, 2, 5] }
// Transfer function: 1 / (s^2 + 2s + 5)
```

**Practical Applications:**
- Converting from zero-pole-gain form (common in control theory)
- Designing filters by specifying desired poles and zeros
- Analyzing system stability and frequency response

### Frequency Response

Calculate the frequency response of a digital filter given its coefficients.

```typescript { .api }
/**
 * Calculate frequency response of digital filter
 * @param b - Numerator coefficients (feedforward coefficients)
 * @param a - Denominator coefficients (feedback coefficients)
 * @param w - Frequency points (optional, defaults to 512 points from 0 to π)
 * @returns Object with frequency (w), magnitude (h), and phase arrays
 */
function freqz(
  b: number[] | Matrix,
  a: number[] | Matrix,
  w?: number | number[] | Matrix
): { w: number[], h: Complex[], phase: number[] };
```

**Parameters:**
- `b`: Numerator coefficients of the transfer function
- `a`: Denominator coefficients of the transfer function  
- `w`: Frequency points to evaluate (optional)
  - If number: evaluate at w equally-spaced points from 0 to π
  - If array: evaluate at specified frequency points
  - If omitted: use 512 points from 0 to π

**Returns:** Object containing:
- `w`: Frequency points (radians/sample)
- `h`: Complex frequency response at each frequency
- `phase`: Phase response in radians at each frequency

**Usage Examples:**

```javascript
import { freqz, abs, atan2 } from 'mathjs';

// Simple first-order lowpass filter
// H(z) = (1 + z^-1) / (2)
const response1 = freqz([1, 1], [2], 128);
// Evaluates at 128 points from 0 to π
// response1.w = [0, π/128, 2π/128, ..., π]
// response1.h = [complex values of H(e^jw)]

// Access magnitude and phase
const magnitudes = response1.h.map(h => abs(h));
const phases = response1.h.map(h => atan2(h.im, h.re));

// Specific frequency points
const response2 = freqz(
  [1, 1],               // numerator
  [2],                  // denominator
  [0, Math.PI/4, Math.PI/2, Math.PI]  // specific frequencies
);
// Evaluates only at 0, π/4, π/2, and π

// Second-order IIR filter
const response3 = freqz(
  [0.05, 0.1, 0.05],    // numerator (feedforward)
  [1, -1.5, 0.55],      // denominator (feedback)
  256
);

// Bode plot data
const freq = response3.w;
const magDB = response3.h.map(h => 20 * Math.log10(abs(h)));
const phaseDeg = response3.phase.map(p => p * 180 / Math.PI);

// Simple moving average filter
const response4 = freqz(
  [1, 1, 1, 1, 1],      // 5-point average
  [5],                  // normalize by 5
  512
);
```

**Practical Applications:**
- Analyzing filter characteristics (magnitude and phase response)
- Designing digital filters to meet frequency specifications
- Verifying filter designs before implementation
- Creating Bode plots for system analysis

**Note:** The frequency response is evaluated on the unit circle in the z-plane at z = e^(jw), where w is the normalized angular frequency in radians/sample.

## Common Types

```typescript { .api }
interface Complex {
  re: number;
  im: number;
}

type MathArray<T> = T[] | Array<MathArray<T>>;
```
