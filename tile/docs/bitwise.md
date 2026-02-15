# Bitwise Operations

Math.js provides 7 bitwise operation functions for performing bit-level manipulations on integers. These operations work with numbers, BigNumber, and support element-wise operations on matrices and arrays.

## Core Imports

```javascript { .api }
import {
  bitAnd, bitNot, bitOr, bitXor,
  leftShift, rightArithShift, rightLogShift
} from 'mathjs';
```

## Bitwise Logical Operations

### bitAnd

Performs a bitwise AND operation on two integers.

```typescript { .api }
function bitAnd(x: number | BigNumber | Array | Matrix, y: number | BigNumber | Array | Matrix): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `x`: First integer value
- `y`: Second integer value

**Returns:** Bitwise AND result (x & y)

**Usage Examples:**

```javascript
import { bitAnd } from 'mathjs';

bitAnd(5, 3);                     // 1 (0101 & 0011 = 0001)
bitAnd(12, 10);                   // 8 (1100 & 1010 = 1000)
bitAnd(0xFF, 0x0F);               // 15 (255 & 15 = 15)

// Element-wise for arrays
bitAnd([5, 6, 7], [3, 4, 5]);
// [1, 4, 5]
```

### bitOr

Performs a bitwise OR operation on two integers.

```typescript { .api }
function bitOr(x: number | BigNumber | Array | Matrix, y: number | BigNumber | Array | Matrix): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `x`: First integer value
- `y`: Second integer value

**Returns:** Bitwise OR result (x | y)

**Usage Examples:**

```javascript
import { bitOr } from 'mathjs';

bitOr(5, 3);                      // 7 (0101 | 0011 = 0111)
bitOr(12, 10);                    // 14 (1100 | 1010 = 1110)
bitOr(0x0F, 0xF0);                // 255 (15 | 240 = 255)

// Element-wise for arrays
bitOr([1, 2, 4], [8, 16, 32]);
// [9, 18, 36]
```

### bitXor

Performs a bitwise XOR (exclusive OR) operation on two integers.

```typescript { .api }
function bitXor(x: number | BigNumber | Array | Matrix, y: number | BigNumber | Array | Matrix): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `x`: First integer value
- `y`: Second integer value

**Returns:** Bitwise XOR result (x ^ y)

**Usage Examples:**

```javascript
import { bitXor } from 'mathjs';

bitXor(5, 3);                     // 6 (0101 ^ 0011 = 0110)
bitXor(12, 10);                   // 6 (1100 ^ 1010 = 0110)
bitXor(0xFF, 0x0F);               // 240 (255 ^ 15 = 240)

// XOR with same value gives 0
bitXor(42, 42);                   // 0

// Element-wise for arrays
bitXor([5, 6, 7], [3, 4, 5]);
// [6, 2, 2]
```

### bitNot

Performs a bitwise NOT operation (one's complement) on an integer.

```typescript { .api }
function bitNot(x: number | BigNumber | Array | Matrix): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `x`: Integer value to invert

**Returns:** Bitwise NOT result (~x)

**Usage Examples:**

```javascript
import { bitNot } from 'mathjs';

bitNot(5);                        // -6 (~0101 = ...11111010)
bitNot(0);                        // -1
bitNot(-1);                       // 0

// Element-wise for arrays
bitNot([1, 2, 3]);
// [-2, -3, -4]

// Note: JavaScript uses 32-bit signed integers for bitwise ops
bitNot(0xFF);                     // -256
```

## Bit Shift Operations

### leftShift

Performs a bitwise left shift operation.

```typescript { .api }
function leftShift(x: number | BigNumber | Array | Matrix, y: number | BigNumber): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `x`: Value to shift
- `y`: Number of bits to shift left

**Returns:** Left shift result (x << y)

**Usage Examples:**

```javascript
import { leftShift } from 'mathjs';

leftShift(1, 2);                  // 4 (0001 << 2 = 0100)
leftShift(5, 3);                  // 40 (0101 << 3 = 101000)
leftShift(1, 10);                 // 1024

// Equivalent to multiplying by 2^n
leftShift(3, 4);                  // 48 (3 * 2^4)

// Element-wise for arrays
leftShift([1, 2, 3], 2);
// [4, 8, 12]
```

### rightArithShift

Performs a bitwise arithmetic right shift (sign-extending).

```typescript { .api }
function rightArithShift(x: number | BigNumber | Array | Matrix, y: number | BigNumber): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `x`: Value to shift
- `y`: Number of bits to shift right

**Returns:** Arithmetic right shift result (x >> y)

**Usage Examples:**

```javascript
import { rightArithShift } from 'mathjs';

rightArithShift(8, 2);            // 2 (1000 >> 2 = 0010)
rightArithShift(40, 3);           // 5 (101000 >> 3 = 101)
rightArithShift(1024, 10);        // 1

// Preserves sign for negative numbers
rightArithShift(-8, 2);           // -2 (sign bit extended)
rightArithShift(-40, 3);          // -5

// Equivalent to dividing by 2^n (floored)
rightArithShift(48, 4);           // 3 (48 / 2^4)

// Element-wise for arrays
rightArithShift([16, 32, 64], 2);
// [4, 8, 16]
```

### rightLogShift

Performs a bitwise logical right shift (zero-filling).

```typescript { .api }
function rightLogShift(x: number | BigNumber | Array | Matrix, y: number | BigNumber): number | BigNumber | Array | Matrix;
```

**Parameters:**
- `x`: Value to shift
- `y`: Number of bits to shift right

**Returns:** Logical right shift result (x >>> y)

**Usage Examples:**

```javascript
import { rightLogShift } from 'mathjs';

rightLogShift(8, 2);              // 2 (1000 >>> 2 = 0010)
rightLogShift(40, 3);             // 5 (101000 >>> 3 = 101)

// Does NOT preserve sign (fills with zeros)
rightLogShift(-8, 2);             // 1073741822
rightLogShift(-1, 1);             // 2147483647

// Used for unsigned integer operations
rightLogShift(0xFF, 4);           // 15

// Element-wise for arrays
rightLogShift([16, 32, 64], 2);
// [4, 8, 16]
```

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathArray<T> = T[] | Array<MathArray<T>>;
type MathCollection = MathArray<any> | Matrix;
```
