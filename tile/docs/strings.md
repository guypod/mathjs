# String Operations

Math.js provides 5 string formatting and conversion functions for converting numbers to various string representations and formatting values for display.

## Core Imports

```javascript { .api }
import {
  format, print, bin, hex, oct
} from 'mathjs';
```

## Formatting Functions

### format

Formats a value as a string with configurable options.

```typescript { .api }
function format(value: any, options?: FormatOptions | number | ((item: any) => string)): string;
```

**Parameters:**
- `value`: Value to format (any type)
- `options`: Formatting options object, precision number, or custom formatter function

**Returns:** Formatted string representation

**Options:**
- `precision`: Number of significant digits (default: 5)
- `notation`: 'fixed', 'exponential', 'engineering', 'auto' (default: 'auto')
- `lowerExp`: Exponent at which to switch to exponential notation (default: -3)
- `upperExp`: Exponent at which to switch to exponential notation (default: 5)
- `fraction`: 'ratio' or 'decimal' for Fraction formatting
- `truncate`: Maximum length before truncation (for arrays/matrices)

**Usage Examples:**

```javascript
import { format, pi, bignumber, fraction, complex, unit } from 'mathjs';

// Default formatting (5 significant digits)
format(pi);                       // '3.1416'

// Custom precision
format(pi, 10);                   // '3.141592654'

// Fixed notation
format(1234.567, {notation: 'fixed', precision: 2});
// '1234.57'

// Exponential notation
format(12345, {notation: 'exponential'});
// '1.2345e+4'

// Engineering notation (powers of 1000)
format(12345, {notation: 'engineering'});
// '12.345e+3'

// Format BigNumber
format(bignumber('1.234567890123456789'));
// '1.2345678901234568'

// Format Complex numbers
format(complex(2, 3));            // '2 + 3i'
format(complex(0, 1));            // 'i'

// Format Fractions
format(fraction(1, 3));           // '1/3'
format(fraction(1, 3), {fraction: 'decimal'});
// '0.33333'

// Format Units
format(unit('5.08 cm'));          // '5.08 cm'

// Format arrays/matrices
format([1, 2, 3]);                // '[1, 2, 3]'
format([[1, 2], [3, 4]]);         // '[[1, 2], [3, 4]]'

// Truncate long arrays
format([1, 2, 3, 4, 5], {truncate: 3});
// '[1, 2, ...]'

// Custom formatter function
format(pi, x => x.toFixed(3));    // '3.142'
```

### print

Performs string interpolation using a template with placeholders.

```typescript { .api }
function print(template: string, values: object | Array, precision?: number): string;
```

**Parameters:**
- `template`: Template string with $variable or $index placeholders
- `values`: Object with named values or array with indexed values
- `precision`: Number of digits for numeric formatting (optional)

**Returns:** Interpolated string

**Usage Examples:**

```javascript
import { print, pi, sqrt } from 'mathjs';

// Named placeholders
print('pi = $pi', {pi: pi});
// 'pi = 3.14159265359'

// Multiple values
print('$a + $b = $sum', {a: 2, b: 3, sum: 5});
// '2 + 3 = 5'

// Indexed placeholders (array values)
print('x = $0, y = $1', [10, 20]);
// 'x = 10, y = 20'

// Custom precision
print('pi = $pi', {pi: pi}, 3);
// 'pi = 3.14'

// Complex template
print('sqrt($n) = $result', {n: 2, result: sqrt(2)}, 4);
// 'sqrt(2) = 1.414'

// Escaping $ with $$
print('Price: $$100', {});
// 'Price: $100'
```

## Number Base Conversion Functions

### bin

Converts a number to a binary (base-2) string representation.

```typescript { .api }
function bin(value: number | BigNumber | Array | Matrix): string | Array | Matrix;
```

**Parameters:**
- `value`: Value to convert to binary

**Returns:** Binary string representation

**Usage Examples:**

```javascript
import { bin } from 'mathjs';

bin(5);                           // '101'
bin(10);                          // '1010'
bin(255);                         // '11111111'
bin(0);                           // '0'

// Negative numbers (two's complement for standard numbers)
bin(-1);                          // '-1'

// Element-wise for arrays
bin([1, 2, 3, 4]);
// ['1', '10', '11', '100']

// Large numbers with BigNumber
bin(bignumber('1234567890'));
// '1001001100101100000001011010010'
```

### hex

Converts a number to a hexadecimal (base-16) string representation.

```typescript { .api }
function hex(value: number | BigNumber | Array | Matrix): string | Array | Matrix;
```

**Parameters:**
- `value`: Value to convert to hexadecimal

**Returns:** Hexadecimal string representation (lowercase)

**Usage Examples:**

```javascript
import { hex } from 'mathjs';

hex(15);                          // 'f'
hex(255);                         // 'ff'
hex(256);                         // '100'
hex(4096);                        // '1000'

// Element-wise for arrays
hex([10, 11, 12, 13, 14, 15]);
// ['a', 'b', 'c', 'd', 'e', 'f']

// Large numbers
hex(1234567890);                  // '499602d2'

// BigNumber support
hex(bignumber('0xDEADBEEF'));
// 'deadbeef'
```

### oct

Converts a number to an octal (base-8) string representation.

```typescript { .api }
function oct(value: number | BigNumber | Array | Matrix): string | Array | Matrix;
```

**Parameters:**
- `value`: Value to convert to octal

**Returns:** Octal string representation

**Usage Examples:**

```javascript
import { oct } from 'mathjs';

oct(8);                           // '10'
oct(64);                          // '100'
oct(255);                         // '377'
oct(512);                         // '1000'

// Element-wise for arrays
oct([8, 16, 32, 64]);
// ['10', '20', '40', '100']

// Large numbers
oct(1234567);                     // '4553207'

// BigNumber support
oct(bignumber('1000000'));
// '3641100'
```

## Types

```typescript { .api }
interface FormatOptions {
  precision?: number;
  notation?: 'fixed' | 'exponential' | 'engineering' | 'auto';
  lowerExp?: number;
  upperExp?: number;
  fraction?: 'ratio' | 'decimal';
  truncate?: number;
}
```

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathArray<T> = T[] | Array<MathArray<T>>;
```
