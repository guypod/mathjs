# Bitwise Operations

Binary operations on integers including bitwise logic and bit shifts. These functions work with JavaScript numbers, BigNumber, and bigint types.

## Capabilities

### Bitwise Logic Operations

Perform bitwise AND, OR, XOR, and NOT operations.

```javascript { .api }
/**
 * Bitwise AND operation (&)
 * @param x - First integer
 * @param y - Second integer
 * @returns Bitwise AND result
 */
function bitAnd(x: number | BigNumber | bigint, y: number | BigNumber | bigint): number | BigNumber | bigint
function bitAnd(x: MathCollection, y: MathCollection): MathCollection

/**
 * Bitwise OR operation (|)
 * @param x - First integer
 * @param y - Second integer
 * @returns Bitwise OR result
 */
function bitOr(x: number | BigNumber | bigint, y: number | BigNumber | bigint): number | BigNumber | bigint
function bitOr(x: MathCollection, y: MathCollection): MathCollection

/**
 * Bitwise XOR operation (^)
 * @param x - First integer
 * @param y - Second integer
 * @returns Bitwise XOR result
 */
function bitXor(x: number | BigNumber | bigint, y: number | BigNumber | bigint): number | BigNumber | bigint
function bitXor(x: MathCollection, y: MathCollection): MathCollection

/**
 * Bitwise NOT operation (~)
 * @param x - Integer to invert
 * @returns Bitwise NOT result (two's complement)
 */
function bitNot(x: number | BigNumber | bigint): number | BigNumber | bigint
function bitNot(x: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { bitAnd, bitOr, bitXor, bitNot } from 'mathjs'

// Bitwise AND
bitAnd(5, 3)              // 1 (0101 & 0011 = 0001)
bitAnd(12, 10)            // 8 (1100 & 1010 = 1000)
bitAnd(0xFF, 0x0F)        // 15 (11111111 & 00001111 = 00001111)

// Bitwise OR
bitOr(5, 3)               // 7 (0101 | 0011 = 0111)
bitOr(12, 10)             // 14 (1100 | 1010 = 1110)
bitOr(0xF0, 0x0F)         // 255 (11110000 | 00001111 = 11111111)

// Bitwise XOR
bitXor(5, 3)              // 6 (0101 ^ 0011 = 0110)
bitXor(12, 10)            // 6 (1100 ^ 1010 = 0110)
bitXor(0xFF, 0xAA)        // 85 (11111111 ^ 10101010 = 01010101)

// Bitwise NOT (two's complement)
bitNot(5)                 // -6 (~0101 = ...11111010)
bitNot(0)                 // -1 (~0 = ...11111111)
bitNot(-1)                // 0 (~...11111111 = 0)

// Element-wise on arrays
bitAnd([1, 2, 3], [3, 2, 1])  // [1, 2, 1]
bitOr([1, 2, 4], [8, 4, 2])   // [9, 6, 6]
```

### Bit Shift Operations

Shift bits left or right with different shift types.

```javascript { .api }
/**
 * Left shift (<<)
 * @param x - Value to shift
 * @param y - Number of positions to shift
 * @returns Left-shifted result
 */
function leftShift(x: number | BigNumber | bigint, y: number): number | BigNumber | bigint
function leftShift(x: MathCollection, y: number): MathCollection

/**
 * Arithmetic right shift (>>)
 * Sign bit is extended
 * @param x - Value to shift
 * @param y - Number of positions to shift
 * @returns Right-shifted result
 */
function rightArithShift(x: number | BigNumber | bigint, y: number): number | BigNumber | bigint
function rightArithShift(x: MathCollection, y: number): MathCollection

/**
 * Logical right shift (>>>)
 * Zeros are shifted in from the left
 * @param x - Value to shift
 * @param y - Number of positions to shift
 * @returns Right-shifted result (unsigned)
 */
function rightLogShift(x: number | BigNumber | bigint, y: number): number | BigNumber | bigint
function rightLogShift(x: MathCollection, y: number): MathCollection
```

**Usage Examples:**

```javascript
import { leftShift, rightArithShift, rightLogShift } from 'mathjs'

// Left shift (multiply by 2^n)
leftShift(1, 2)           // 4 (0001 << 2 = 0100)
leftShift(5, 1)           // 10 (0101 << 1 = 1010)
leftShift(3, 4)           // 48 (0011 << 4 = 110000)

// Arithmetic right shift (divide by 2^n, preserve sign)
rightArithShift(8, 2)     // 2 (1000 >> 2 = 0010)
rightArithShift(5, 1)     // 2 (0101 >> 1 = 0010)
rightArithShift(-8, 2)    // -2 (sign preserved)

// Logical right shift (divide by 2^n, unsigned)
rightLogShift(8, 2)       // 2 (1000 >>> 2 = 0010)
rightLogShift(5, 1)       // 2 (0101 >>> 1 = 0010)
rightLogShift(-8, 2)      // 1073741822 (treats as unsigned 32-bit)

// Element-wise on arrays
leftShift([1, 2, 4], 2)   // [4, 8, 16]
rightArithShift([8, 16, 32], 2)  // [2, 4, 8]
```

## Usage in Expressions

Bitwise operators can be used in string expressions:

```javascript
import { evaluate } from 'mathjs'

// Bitwise logic
evaluate('5 & 3')             // 1
evaluate('5 | 3')             // 7
evaluate('5 ^| 3')            // 6 (XOR uses ^| in expressions)
evaluate('~5')                // -6

// Bit shifts
evaluate('4 << 2')            // 16
evaluate('16 >> 2')           // 4
evaluate('-8 >>> 2')          // 1073741822
```

## Common Bitwise Patterns

### Bit Manipulation

```javascript
import { bitAnd, bitOr, bitXor, leftShift, rightArithShift } from 'mathjs'

// Set bit at position n
function setBit(value, n) {
  return bitOr(value, leftShift(1, n))
}
setBit(0, 3)                  // 8 (00000 -> 01000)

// Clear bit at position n
function clearBit(value, n) {
  return bitAnd(value, bitNot(leftShift(1, n)))
}
clearBit(15, 2)               // 11 (01111 -> 01011)

// Toggle bit at position n
function toggleBit(value, n) {
  return bitXor(value, leftShift(1, n))
}
toggleBit(10, 1)              // 8 (01010 -> 01000)

// Test if bit at position n is set
function testBit(value, n) {
  return bitAnd(value, leftShift(1, n)) !== 0
}
testBit(10, 1)                // true (01010, bit 1 is set)
testBit(10, 2)                // false (01010, bit 2 is clear)
```

### Masking Operations

```javascript
import { bitAnd, bitOr, bitXor } from 'mathjs'

// Extract lower byte
function getLowByte(value) {
  return bitAnd(value, 0xFF)
}
getLowByte(0x1234)            // 0x34 (52)

// Extract upper byte
function getHighByte(value) {
  return rightArithShift(bitAnd(value, 0xFF00), 8)
}
getHighByte(0x1234)           // 0x12 (18)

// Combine bytes
function combineBytes(high, low) {
  return bitOr(leftShift(high, 8), bitAnd(low, 0xFF))
}
combineBytes(0x12, 0x34)      // 0x1234 (4660)

// Apply mask
const mask = 0x0F
bitAnd(0x5A, mask)            // 0x0A (extract lower nibble)
```

### Flags and Bit Fields

```javascript
import { bitAnd, bitOr, bitXor, leftShift } from 'mathjs'

// Define flags
const FLAG_READ = leftShift(1, 0)    // 0001
const FLAG_WRITE = leftShift(1, 1)   // 0010
const FLAG_EXECUTE = leftShift(1, 2) // 0100
const FLAG_DELETE = leftShift(1, 3)  // 1000

// Set flags
let permissions = 0
permissions = bitOr(permissions, FLAG_READ)
permissions = bitOr(permissions, FLAG_WRITE)
// permissions = 0011 (READ | WRITE)

// Check if flag is set
function hasFlag(value, flag) {
  return bitAnd(value, flag) === flag
}
hasFlag(permissions, FLAG_READ)      // true
hasFlag(permissions, FLAG_EXECUTE)   // false

// Toggle flag
permissions = bitXor(permissions, FLAG_WRITE)
// permissions = 0001 (WRITE toggled off)

// Clear flag
permissions = bitAnd(permissions, bitNot(FLAG_READ))
// permissions = 0000 (READ cleared)
```

### Color Manipulation

```javascript
import { bitAnd, bitOr, leftShift, rightArithShift } from 'mathjs'

// RGB color as 0xRRGGBB
const color = 0xFF8040  // Orange

// Extract components
const red = rightArithShift(bitAnd(color, 0xFF0000), 16)    // 255
const green = rightArithShift(bitAnd(color, 0x00FF00), 8)   // 128
const blue = bitAnd(color, 0x0000FF)                        // 64

// Create color from components
function rgb(r, g, b) {
  return bitOr(bitOr(leftShift(r, 16), leftShift(g, 8)), b)
}
rgb(255, 128, 64)                    // 0xFF8040

// Convert to grayscale (average)
function toGray(color) {
  const r = rightArithShift(bitAnd(color, 0xFF0000), 16)
  const g = rightArithShift(bitAnd(color, 0x00FF00), 8)
  const b = bitAnd(color, 0x0000FF)
  const gray = Math.floor((r + g + b) / 3)
  return rgb(gray, gray, gray)
}
toGray(0xFF8040)                     // 0x969696
```

## Working with Different Integer Types

### JavaScript Numbers

Standard 32-bit signed integer operations (for bitwise):

```javascript
import { bitAnd, leftShift } from 'mathjs'

// JavaScript numbers treated as 32-bit signed integers
bitAnd(0xFFFFFFFF, 0x0F)      // 15
leftShift(1, 31)              // -2147483648 (sign bit set)
```

### BigNumber

Bitwise operations on arbitrary-precision integers:

```javascript
import { bignumber, bitAnd, bitOr, leftShift } from 'mathjs'

const a = bignumber('0xFFFFFFFFFFFFFFFF')  // 64-bit
const b = bignumber('0x0F0F0F0F0F0F0F0F')

bitAnd(a, b)                  // BigNumber 0x0F0F0F0F0F0F0F0F
bitOr(a, b)                   // BigNumber 0xFFFFFFFFFFFFFFFF

// Large shifts
leftShift(bignumber(1), 100)  // 2^100 as BigNumber
```

### Bigint (ES2020)

Native JavaScript big integers:

```javascript
import { bigint, bitAnd, bitOr, leftShift } from 'mathjs'

const a = bigint('0xFFFFFFFFFFFFFFFF')
const b = bigint('0x0F0F0F0F0F0F0F0F')

bitAnd(a, b)                  // 1085102592571150095n
bitOr(a, b)                   // 18446744073709551615n
leftShift(bigint(1), 100)     // 1267650600228229401496703205376n
```

## Bitwise Operations on Arrays

All bitwise functions support element-wise operations on arrays and matrices:

```javascript
import { bitAnd, bitOr, bitXor, leftShift, rightArithShift } from 'mathjs'

const a = [0x0F, 0xF0, 0xFF, 0x00]
const b = [0x55, 0x55, 0xAA, 0xAA]

bitAnd(a, b)                  // [5, 80, 170, 0]
bitOr(a, b)                   // [95, 245, 255, 170]
bitXor(a, b)                  // [90, 165, 85, 170]

// Shift all elements
leftShift(a, 1)               // [30, 480, 510, 0]
rightArithShift([8, 16, 32], 2)  // [2, 4, 8]
```

## Performance Considerations

- Bitwise operations are very fast, operating at the CPU level
- Use bitwise operations instead of arithmetic when appropriate (e.g., `leftShift(x, 1)` vs `multiply(x, 2)`)
- For JavaScript numbers, operations are 32-bit only
- For BigNumber and bigint, operations can be slower but support arbitrary precision
- Element-wise array operations may be slower than scalar operations
