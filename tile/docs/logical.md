# Logical Operations

Math.js provides 5 logical operation functions for Boolean logic. These operations support standard logical operators and work with boolean values, numbers (where 0 is false and non-zero is true), and element-wise operations on arrays and matrices.

## Core Imports

```javascript { .api }
import {
  and, or, not, xor, nullish
} from 'mathjs';
```

## Logical Operators

### and

Performs a logical AND operation on two values.

```typescript { .api }
function and(x: number | BigNumber | Complex | Unit | Array | Matrix, y: number | BigNumber | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** Logical AND result (true if both values are truthy)

**Usage Examples:**

```javascript
import { and } from 'mathjs';

// Boolean values
and(true, true);                  // true
and(true, false);                 // false
and(false, false);                // false

// Numeric values (0 is false, non-zero is true)
and(1, 1);                        // true
and(1, 0);                        // false
and(5, 3);                        // true

// Element-wise for arrays
and([true, true, false], [true, false, true]);
// [true, false, false]

and([1, 0, 1], [1, 1, 0]);
// [true, false, false]
```

### or

Performs a logical OR operation on two values.

```typescript { .api }
function or(x: number | BigNumber | Complex | Unit | Array | Matrix, y: number | BigNumber | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** Logical OR result (true if at least one value is truthy)

**Usage Examples:**

```javascript
import { or } from 'mathjs';

// Boolean values
or(true, true);                   // true
or(true, false);                  // true
or(false, false);                 // false

// Numeric values
or(0, 0);                         // false
or(0, 1);                         // true
or(5, 3);                         // true

// Element-wise for arrays
or([true, false, false], [false, false, true]);
// [true, false, true]

or([1, 0, 0], [0, 0, 1]);
// [true, false, true]
```

### not

Performs a logical NOT operation (negation) on a value.

```typescript { .api }
function not(x: number | BigNumber | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to negate

**Returns:** Logical NOT result (inverts truthiness)

**Usage Examples:**

```javascript
import { not } from 'mathjs';

// Boolean values
not(true);                        // false
not(false);                       // true

// Numeric values
not(0);                           // true
not(1);                           // false
not(5);                           // false

// Element-wise for arrays
not([true, false, true]);
// [false, true, false]

not([0, 1, 2]);
// [true, false, false]
```

### xor

Performs a logical XOR (exclusive OR) operation on two values.

```typescript { .api }
function xor(x: number | BigNumber | Complex | Unit | Array | Matrix, y: number | BigNumber | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** Logical XOR result (true if exactly one value is truthy)

**Usage Examples:**

```javascript
import { xor } from 'mathjs';

// Boolean values
xor(true, true);                  // false (both true)
xor(true, false);                 // true (one true)
xor(false, true);                 // true (one true)
xor(false, false);                // false (both false)

// Numeric values
xor(1, 0);                        // true
xor(0, 1);                        // true
xor(1, 1);                        // false
xor(0, 0);                        // false

// Element-wise for arrays
xor([true, true, false], [true, false, true]);
// [false, true, true]

xor([1, 0, 1], [1, 1, 0]);
// [false, true, true]
```

### nullish

Checks if a value is null or undefined.

```typescript { .api }
function nullish(x: any): boolean;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x is null or undefined, false otherwise

**Usage Examples:**

```javascript
import { nullish } from 'mathjs';

nullish(null);                    // true
nullish(undefined);               // true

// All other values are not nullish
nullish(0);                       // false
nullish(false);                   // false
nullish('');                      // false
nullish([]);                      // false
nullish({});                      // false

// Useful for null coalescing
const value = nullish(x) ? defaultValue : x;
```

## Truthiness Rules

Math.js follows these truthiness conventions:

- **Boolean**: `true` is truthy, `false` is falsy
- **Numbers**: `0` is falsy, all non-zero values are truthy
- **BigNumber**: BigNumber zero is falsy, all other values are truthy
- **null/undefined**: Both are considered falsy
- **Complex**: Zero complex number (0 + 0i) is falsy, all other complex numbers are truthy
- **Strings**: Empty string is falsy, all non-empty strings are truthy

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathArray<T> = T[] | Array<MathArray<T>>;
type MathCollection = MathArray<any> | Matrix;
```
