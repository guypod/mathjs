# Relational Operations

Math.js provides 13 relational and comparison functions for comparing values. These operations support various data types including numbers, strings, units, and complex numbers, with element-wise operations on arrays and matrices.

## Core Imports

```javascript { .api }
import {
  equal, unequal, smaller, smallerEq, larger, largerEq,
  compare, compareNatural, compareText, compareUnits,
  deepEqual, equalScalar, equalText
} from 'mathjs';
```

## Basic Comparison Operations

### equal

Tests equality between two values.

```typescript { .api }
function equal(x: MathType, y: MathType): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x equals y, false otherwise

**Usage Examples:**

```javascript
import { equal, complex, unit } from 'mathjs';

// Numbers
equal(5, 5);                      // true
equal(5, 6);                      // false

// Complex numbers
equal(complex(2, 3), complex(2, 3));
// true

// Units (converted for comparison)
equal(unit('100 cm'), unit('1 m'));
// true

// Element-wise for arrays
equal([1, 2, 3], [1, 2, 4]);
// [true, true, false]
```

### unequal

Tests inequality between two values.

```typescript { .api }
function unequal(x: MathType, y: MathType): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x does not equal y, false otherwise

**Usage Examples:**

```javascript
import { unequal } from 'mathjs';

unequal(5, 6);                    // true
unequal(5, 5);                    // false

unequal([1, 2, 3], [1, 2, 4]);
// [false, false, true]
```

### smaller

Tests if first value is smaller than second value (x < y).

```typescript { .api }
function smaller(x: MathType, y: MathType): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x < y, false otherwise

**Usage Examples:**

```javascript
import { smaller, bignumber } from 'mathjs';

smaller(3, 5);                    // true
smaller(5, 3);                    // false
smaller(5, 5);                    // false

// BigNumber for precise comparisons
smaller(bignumber(0.1), bignumber(0.2));
// true

// Element-wise for arrays
smaller([1, 2, 3], [2, 2, 2]);
// [true, false, false]
```

### smallerEq

Tests if first value is smaller than or equal to second value (x <= y).

```typescript { .api }
function smallerEq(x: MathType, y: MathType): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x <= y, false otherwise

**Usage Examples:**

```javascript
import { smallerEq } from 'mathjs';

smallerEq(3, 5);                  // true
smallerEq(5, 5);                  // true
smallerEq(7, 5);                  // false

smallerEq([1, 2, 3], [2, 2, 2]);
// [true, true, false]
```

### larger

Tests if first value is larger than second value (x > y).

```typescript { .api }
function larger(x: MathType, y: MathType): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x > y, false otherwise

**Usage Examples:**

```javascript
import { larger } from 'mathjs';

larger(5, 3);                     // true
larger(3, 5);                     // false
larger(5, 5);                     // false

larger([3, 2, 1], [2, 2, 2]);
// [true, false, false]
```

### largerEq

Tests if first value is larger than or equal to second value (x >= y).

```typescript { .api }
function largerEq(x: MathType, y: MathType): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x >= y, false otherwise

**Usage Examples:**

```javascript
import { largerEq } from 'mathjs';

largerEq(5, 3);                   // true
largerEq(5, 5);                   // true
largerEq(3, 5);                   // false

largerEq([3, 2, 1], [2, 2, 2]);
// [true, true, false]
```

## Advanced Comparison Functions

### compare

Compares two values and returns -1, 0, or 1.

```typescript { .api }
function compare(x: MathType, y: MathType): number | BigNumber | Fraction | Array | Matrix;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:**
- `-1` if x < y
- `0` if x == y
- `1` if x > y

**Usage Examples:**

```javascript
import { compare } from 'mathjs';

compare(3, 5);                    // -1
compare(5, 5);                    // 0
compare(7, 5);                    // 1

// Useful for sorting
[3, 1, 2].sort((a, b) => compare(a, b));
// [1, 2, 3]

compare([1, 2, 3], [2, 2, 2]);
// [-1, 0, 1]
```

### compareNatural

Compares two values using natural ordering.

```typescript { .api }
function compareNatural(x: any, y: any): number;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:**
- `-1` if x < y
- `0` if x == y
- `1` if x > y

**Usage Examples:**

```javascript
import { compareNatural } from 'mathjs';

// Natural number ordering
compareNatural(2, 10);            // -1 (numeric comparison)

// Works with different types
compareNatural(null, undefined);  // -1
compareNatural(1, true);          // 0

// Useful for sorting mixed arrays
['10', '2', '1'].sort(compareNatural);
// ['1', '2', '10'] (natural order)
```

### compareText

Compares two values as text (lexicographic ordering).

```typescript { .api }
function compareText(x: string | Array | Matrix, y: string | Array | Matrix): number | Array | Matrix;
```

**Parameters:**
- `x`: First value (converted to string)
- `y`: Second value (converted to string)

**Returns:**
- `-1` if x < y lexicographically
- `0` if x == y
- `1` if x > y lexicographically

**Usage Examples:**

```javascript
import { compareText } from 'mathjs';

compareText('apple', 'banana');   // -1
compareText('10', '2');           // -1 (string comparison: '1' < '2')
compareText('abc', 'abc');        // 0

// Element-wise comparison
compareText(['a', 'b'], ['b', 'a']);
// [-1, 1]
```

### compareUnits

Compares two physical unit values after conversion.

```typescript { .api }
function compareUnits(x: Unit, y: Unit): number | BigNumber | Fraction;
```

**Parameters:**
- `x`: First unit value
- `y`: Second unit value (must be compatible unit)

**Returns:**
- `-1` if x < y
- `0` if x == y
- `1` if x > y

**Usage Examples:**

```javascript
import { compareUnits, unit } from 'mathjs';

compareUnits(unit('100 cm'), unit('1 m'));
// 0 (equal after conversion)

compareUnits(unit('1 km'), unit('100 m'));
// 1 (1 km > 100 m)

compareUnits(unit('1 hour'), unit('30 min'));
// 1 (1 hour > 30 min)

// Throws error for incompatible units
compareUnits(unit('1 m'), unit('1 kg'));
// Error: Cannot compare units with different dimensions
```

## Specialized Equality Functions

### deepEqual

Performs deep equality comparison (compares nested structures).

```typescript { .api }
function deepEqual(x: any, y: any): boolean;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x and y are deeply equal, false otherwise

**Usage Examples:**

```javascript
import { deepEqual, complex, matrix } from 'mathjs';

// Deep object comparison
deepEqual({a: 1, b: 2}, {a: 1, b: 2});
// true

// Nested arrays
deepEqual([[1, 2], [3, 4]], [[1, 2], [3, 4]]);
// true

// Complex structures
deepEqual(
  {x: complex(1, 2), y: [1, 2, 3]},
  {x: complex(1, 2), y: [1, 2, 3]}
);
// true

// Order matters for objects
deepEqual({a: 1, b: 2}, {b: 2, a: 1});
// true (object property order ignored)
```

### equalScalar

Tests scalar equality (does not perform element-wise comparison).

```typescript { .api }
function equalScalar(x: MathType, y: MathType): boolean;
```

**Parameters:**
- `x`: First value
- `y`: Second value

**Returns:** true if x equals y as scalars, false otherwise

**Usage Examples:**

```javascript
import { equalScalar, equal } from 'mathjs';

// Scalar comparison
equalScalar(5, 5);                // true
equalScalar(5, 6);                // false

// Unlike equal(), does NOT work element-wise
equalScalar([1, 2], [1, 2]);      // false (different array references)
equal([1, 2], [1, 2]);            // [true, true] (element-wise)

// Use for direct value comparison
equalScalar(2 + 3, 5);            // true
```

### equalText

Tests text equality (case-sensitive string comparison).

```typescript { .api }
function equalText(x: string | Array | Matrix, y: string | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: First value (converted to string)
- `y`: Second value (converted to string)

**Returns:** true if text representations are equal, false otherwise

**Usage Examples:**

```javascript
import { equalText } from 'mathjs';

equalText('hello', 'hello');      // true
equalText('hello', 'Hello');      // false (case-sensitive)
equalText('123', 123);            // true (converts to string)

// Element-wise for arrays
equalText(['a', 'b'], ['a', 'c']);
// [true, false]
```

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathScalarType = MathNumericType | Unit;
type MathArray<T> = T[] | Array<MathArray<T>>;
type MathCollection = MathArray<any> | Matrix;
type MathType = MathScalarType | MathCollection;
```
