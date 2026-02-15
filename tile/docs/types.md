# Type System

Math.js provides a rich type system for mathematical computations, including arbitrary-precision numbers, complex numbers, fractions, physical units, and matrices. The type system includes constructors, type classes with methods, and comprehensive type checking utilities.

## Capabilities

### Type Constructors

Functions for creating and converting to specific mathematical types.

```typescript { .api }
/**
 * Create or convert to BigNumber (arbitrary precision decimal)
 * @param x - Value to convert (number, string, or other numeric type)
 * @returns BigNumber instance
 */
function bignumber(x: number | string | BigNumber | Fraction | Complex | Unit): BigNumber;

/**
 * Create or convert to native bigint
 * @param x - Value to convert
 * @returns bigint value
 */
function bigint(x: number | string | boolean | BigNumber | Fraction | bigint): bigint;

/**
 * Create or convert to boolean
 * @param x - Value to convert
 * @returns boolean value
 */
function boolean(x: number | string | boolean | null): boolean;

/**
 * Create complex number from real and imaginary parts
 * @param re - Real part (number, BigNumber, string, or Complex)
 * @param im - Imaginary part (optional, defaults to 0)
 * @returns Complex number instance
 */
function complex(re: number | BigNumber | string | Complex, im?: number | BigNumber | string): Complex;

/**
 * Create fraction from numerator and optional denominator
 * @param numerator - Numerator value
 * @param denominator - Denominator value (optional, defaults to 1)
 * @returns Fraction instance
 */
function fraction(numerator: number | string | BigNumber | Fraction, denominator?: number | string | BigNumber): Fraction;

/**
 * Create dense matrix from array data
 * @param data - Multi-dimensional array data
 * @param format - Matrix format ('dense' or 'sparse', defaults to 'dense')
 * @returns DenseMatrix instance
 */
function matrix(data: any[][], format?: 'dense' | 'sparse'): DenseMatrix;

/**
 * Create sparse matrix from array data
 * @param data - Multi-dimensional array data
 * @param format - Matrix format (defaults to 'sparse')
 * @returns SparseMatrix instance
 */
function sparse(data: any[][], format?: string): SparseMatrix;

/**
 * Create or convert to number
 * @param x - Value to convert
 * @returns number value
 */
function number(x: number | string | boolean | BigNumber | Fraction | Complex | Unit | bigint | null): number;

/**
 * Convert value to string
 * @param x - Value to convert
 * @returns string representation
 */
function string(x: any): string;

/**
 * Create unit with value and unit string
 * @param value - Numeric value (number, BigNumber, Fraction, Complex, or null for valueless unit)
 * @param unit - Unit string (e.g., 'm', 'kg', 'm/s')
 * @returns Unit instance
 */
function unit(value: number | BigNumber | Fraction | Complex | string | null, unit?: string): Unit;
```

### Type Classes

Core type classes with their methods and properties.

#### BigNumber Class

```typescript { .api }
/**
 * Arbitrary precision decimal number class
 */
class BigNumber {
  /** Create BigNumber from value */
  constructor(value: number | string | BigNumber);

  /** Absolute value */
  abs(): BigNumber;

  /** Addition */
  add(y: BigNumber | number | string): BigNumber;

  /** Ceiling (round up) */
  ceil(): BigNumber;

  /** Comparison (-1, 0, or 1) */
  comparedTo(y: BigNumber | number | string): number;

  /** Cosine */
  cos(): BigNumber;

  /** Cube root */
  cubeRoot(): BigNumber;

  /** Division */
  div(y: BigNumber | number | string): BigNumber;

  /** Equality check */
  equals(y: BigNumber | number | string): boolean;

  /** Exponential (e^x) */
  exp(): BigNumber;

  /** Floor (round down) */
  floor(): BigNumber;

  /** Greater than */
  greaterThan(y: BigNumber | number | string): boolean;

  /** Greater than or equal */
  greaterThanOrEqualTo(y: BigNumber | number | string): boolean;

  /** Check if integer */
  isInteger(): boolean;

  /** Check if NaN */
  isNaN(): boolean;

  /** Check if negative */
  isNegative(): boolean;

  /** Check if positive */
  isPositive(): boolean;

  /** Check if zero */
  isZero(): boolean;

  /** Less than */
  lessThan(y: BigNumber | number | string): boolean;

  /** Less than or equal */
  lessThanOrEqualTo(y: BigNumber | number | string): boolean;

  /** Natural logarithm */
  ln(): BigNumber;

  /** Logarithm with specified base */
  log(base?: BigNumber | number | string): BigNumber;

  /** Subtraction */
  minus(y: BigNumber | number | string): BigNumber;

  /** Modulo */
  mod(y: BigNumber | number | string): BigNumber;

  /** Multiplication */
  mul(y: BigNumber | number | string): BigNumber;

  /** Negation */
  neg(): BigNumber;

  /** Addition (alias for add) */
  plus(y: BigNumber | number | string): BigNumber;

  /** Power */
  pow(n: BigNumber | number | string): BigNumber;

  /** Round to n decimal places */
  round(n?: number): BigNumber;

  /** Sine */
  sin(): BigNumber;

  /** Square root */
  sqrt(): BigNumber;

  /** Tangent */
  tan(): BigNumber;

  /** Division (alias for div) */
  times(y: BigNumber | number | string): BigNumber;

  /** Convert to exponential notation string */
  toExponential(decimalPlaces?: number): string;

  /** Convert to fixed-point notation string */
  toFixed(decimalPlaces?: number): string;

  /** Convert to JSON representation */
  toJSON(): string;

  /** Convert to number (may lose precision) */
  toNumber(): number;

  /** Convert to string */
  toString(): string;

  /** Convert to number for valueOf() */
  valueOf(): number;
}
```

#### Complex Class

```typescript { .api }
/**
 * Complex number with real and imaginary parts
 */
class Complex {
  /** Real part */
  re: number;

  /** Imaginary part */
  im: number;

  /** Create complex number */
  constructor(re: number, im: number);

  /** Clone this complex number */
  clone(): Complex;

  /** Check if equal to another complex number */
  equals(other: Complex): boolean;

  /** Convert to string representation */
  toString(): string;

  /** Convert to JSON representation */
  toJSON(): { mathjs: string; re: number; im: number };

  /** Convert to polar coordinates {r, phi} */
  toPolar(): { r: number; phi: number };

  /** Get format options */
  format(options?: FormatOptions): string;

  /** Create from polar coordinates */
  static fromPolar(r: number, phi: number): Complex;
}
```

#### Fraction Class

```typescript { .api }
/**
 * Rational number represented as numerator/denominator
 */
class Fraction {
  /** Numerator (sign is stored here) */
  s: number;

  /** Numerator (absolute value) */
  n: number;

  /** Denominator (always positive) */
  d: number;

  /** Create fraction */
  constructor(numerator: number | string, denominator?: number | string);

  /** Absolute value */
  abs(): Fraction;

  /** Addition */
  add(y: Fraction | number | string): Fraction;

  /** Ceiling */
  ceil(): Fraction;

  /** Clone this fraction */
  clone(): Fraction;

  /** Comparison (-1, 0, or 1) */
  compare(y: Fraction | number): number;

  /** Division */
  div(y: Fraction | number | string): Fraction;

  /** Check if equal */
  equals(y: Fraction | number): boolean;

  /** Floor */
  floor(): Fraction;

  /** Greatest common divisor */
  gcd(y: Fraction): Fraction;

  /** Inverse (1/x) */
  inverse(): Fraction;

  /** Least common multiple */
  lcm(y: Fraction): Fraction;

  /** Modulo */
  mod(y: Fraction | number | string): Fraction;

  /** Multiplication */
  mul(y: Fraction | number | string): Fraction;

  /** Negation */
  neg(): Fraction;

  /** Power */
  pow(y: Fraction | number): Fraction;

  /** Round to n decimal places */
  round(n?: number): Fraction;

  /** Subtraction */
  sub(y: Fraction | number | string): Fraction;

  /** Convert to JSON */
  toJSON(): { mathjs: string; n: number; d: number };

  /** Convert to number */
  valueOf(): number;

  /** Convert to string */
  toString(): string;

  /** Convert to LaTeX string */
  toLatex(): string;
}
```

#### Unit Class

```typescript { .api }
/**
 * Value with physical unit
 */
class Unit {
  /** Numeric value */
  value: number | BigNumber | Fraction | Complex | null;

  /** Unit dimensions and prefixes */
  units: UnitDefinition[];

  /** Create unit */
  constructor(value: number | BigNumber | Fraction | Complex | null, unit: string);

  /** Clone this unit */
  clone(): Unit;

  /** Check if equal to another unit */
  equals(other: Unit): boolean;

  /** Check if this unit has the same base as another */
  equalBase(other: Unit): boolean;

  /** Convert to another unit */
  to(unit: string | Unit): Unit;

  /** Convert to SI base units */
  toSI(): Unit;

  /** Convert to number in specified unit */
  toNumber(unit?: string | Unit): number;

  /** Convert to numeric value (without unit) */
  toNumeric(unit?: string | Unit): number | BigNumber | Fraction | Complex;

  /** Get string representation */
  toString(): string;

  /** Get LaTeX representation */
  toLatex(options?: FormatOptions): string;

  /** Get JSON representation */
  toJSON(): { mathjs: string; value: any; unit: string };

  /** Format with options */
  format(options?: FormatOptions): string;

  /** Simplify unit expression */
  simplify(): Unit;

  /** Split unit into parts */
  splitUnit(parts: string[]): Unit[];

  /** Absolute value */
  abs(): Unit;

  /** Addition */
  add(other: Unit): Unit;

  /** Subtraction */
  sub(other: Unit): Unit;

  /** Multiplication */
  mul(other: Unit | number | BigNumber | Fraction | Complex): Unit;

  /** Division */
  div(other: Unit | number | BigNumber | Fraction | Complex): Unit;

  /** Power */
  pow(p: number | BigNumber | Fraction): Unit;

  /** Square root */
  sqrt(): Unit;
}
```

#### Matrix Classes

```typescript { .api }
/**
 * Base matrix class
 */
abstract class Matrix {
  /** Matrix storage type */
  type: string;

  /** Matrix storage format */
  storage(): string;

  /** Matrix data type */
  datatype(): string | undefined;

  /** Create matrix (use matrix() or sparse() functions instead) */
  constructor();

  /** Get matrix size as array [rows, cols, ...] */
  size(): number[];

  /** Get value at index */
  get(index: number[]): any;

  /** Set value at index */
  set(index: number[], value: any, defaultValue?: any): Matrix;

  /** Get subset of matrix */
  subset(index: Index, replacement?: any, defaultValue?: any): Matrix;

  /** Resize matrix */
  resize(size: number[], defaultValue?: any): Matrix;

  /** Reshape matrix */
  reshape(size: number[]): Matrix;

  /** Clone matrix */
  clone(): Matrix;

  /** Convert to array */
  toArray(): any[];

  /** Convert to JSON */
  toJSON(): object;

  /** Iterate over all entries */
  forEach(callback: (value: any, index: number[], matrix: Matrix) => void): void;

  /** Map function over all entries */
  map(callback: (value: any, index: number[], matrix: Matrix) => any): Matrix;

  /** Get string representation */
  toString(): string;

  /** Transpose matrix */
  transpose(): Matrix;
}

/**
 * Dense matrix with 2D array storage
 */
class DenseMatrix extends Matrix {
  /** Dense matrix data storage */
  _data: any[][];

  /** Matrix size [rows, cols] */
  _size: number[];

  /** Create dense matrix */
  constructor(data: any[][], datatype?: string);

  /** All base Matrix methods plus: */

  /** Get diagonal as array */
  diagonal(k?: number): any[];

  /** Swap two rows */
  swapRows(i: number, j: number): DenseMatrix;
}

/**
 * Sparse matrix with compressed column storage
 */
class SparseMatrix extends Matrix {
  /** Column pointers */
  _ptr: number[];

  /** Row indices */
  _index: number[];

  /** Non-zero values */
  _values: any[];

  /** Matrix size [rows, cols] */
  _size: number[];

  /** Create sparse matrix */
  constructor(data: any[][], datatype?: string);

  /** All base Matrix methods plus: */

  /** Get number of non-zero elements */
  nonZeros(): number;
}
```

#### Range Class

```typescript { .api }
/**
 * Number range with start, end, and step
 */
class Range {
  /** Range start value */
  start: number;

  /** Range end value */
  end: number;

  /** Range step value */
  step: number;

  /** Create range */
  constructor(start: number, end: number, step?: number);

  /** Get size of range */
  size(): number[];

  /** Calculate range values as array */
  valueOf(): number[];

  /** Convert to string */
  toString(): string;

  /** Convert to JSON */
  toJSON(): { mathjs: string; start: number; end: number; step: number };

  /** Format range */
  format(options?: FormatOptions): string;

  /** Iterate over range values */
  forEach(callback: (value: number, index: number, range: Range) => void): void;

  /** Map function over range values */
  map(callback: (value: number, index: number, range: Range) => any): any[];
}
```

#### Index Class

```typescript { .api }
/**
 * Multi-dimensional matrix index
 */
class Index {
  /** Index dimensions */
  _dimensions: IRange[];

  /** Create index with dimensions */
  constructor(...dimensions: (number | Range | IRange)[][]);

  /** Clone this index */
  clone(): Index;

  /** Get size of each dimension */
  size(): number[];

  /** Get max value for each dimension */
  max(): number[];

  /** Get min value for each dimension */
  min(): number[];

  /** Iterate over index combinations */
  forEach(callback: (value: number[], index: number[]) => void): void;

  /** Get dimension at position */
  dimension(dim: number): IRange;

  /** Check if scalar index (single element) */
  isScalar(): boolean;

  /** Convert to array of ranges */
  toArray(): number[][];

  /** Convert to JSON */
  toJSON(): { mathjs: string; dimensions: any[] };

  /** Convert to string */
  toString(): string;
}

/** Helper function to create Index */
function index(...dimensions: (number | Range | number[])[]): Index;
```

#### ResultSet Class

```typescript { .api }
/**
 * Container for multiple expression results
 */
class ResultSet {
  /** Array of entries */
  entries: any[];

  /** Create result set */
  constructor(entries: any[]);

  /** Get value at index */
  valueOf(): any[];

  /** Convert to string */
  toString(): string;

  /** Convert to JSON */
  toJSON(): { mathjs: string; entries: any[] };

  /** Map function over entries */
  map(callback: (entry: any, index: number) => any): any[];

  /** Iterate over entries */
  forEach(callback: (entry: any, index: number) => void): void;
}
```

### Type Checker Functions

Boolean functions for runtime type checking.

#### Numeric Type Checkers

```typescript { .api }
/**
 * Check if value is a number
 * @param x - Value to check
 * @returns true if x is a number
 */
function isNumber(x: any): boolean;

/**
 * Check if value is a BigNumber
 * @param x - Value to check
 * @returns true if x is a BigNumber instance
 */
function isBigNumber(x: any): boolean;

/**
 * Check if value is a bigint
 * @param x - Value to check
 * @returns true if x is a bigint
 */
function isBigInt(x: any): boolean;

/**
 * Check if value is a Complex number
 * @param x - Value to check
 * @returns true if x is a Complex instance
 */
function isComplex(x: any): boolean;

/**
 * Check if value is a Fraction
 * @param x - Value to check
 * @returns true if x is a Fraction instance
 */
function isFraction(x: any): boolean;

/**
 * Check if value is numeric (has numeric value)
 * @param x - Value to check
 * @returns true if x has a numeric value
 */
function isNumeric(x: any): boolean;

/**
 * Check if value has a numeric value
 * @param x - Value to check
 * @returns true if value has numeric representation
 */
function hasNumericValue(x: any): boolean;

/**
 * Check if value is NaN
 * @param x - Value to check
 * @returns true if x is NaN
 */
function isNaN(x: any): boolean;

/**
 * Check if value is finite
 * @param x - Value to check
 * @returns true if x is finite number
 */
function isFinite(x: any): boolean;

/**
 * Check if value is an integer
 * @param x - Value to check
 * @returns true if x is an integer
 */
function isInteger(x: any): boolean;

/**
 * Check if value is negative
 * @param x - Value to check
 * @returns true if x is negative
 */
function isNegative(x: any): boolean;

/**
 * Check if value is positive
 * @param x - Value to check
 * @returns true if x is positive
 */
function isPositive(x: any): boolean;

/**
 * Check if value is zero
 * @param x - Value to check
 * @returns true if x equals zero
 */
function isZero(x: any): boolean;

/**
 * Check if value is a prime number
 * @param x - Value to check
 * @returns true if x is prime
 */
function isPrime(x: any): boolean;
```

#### Collection Type Checkers

```typescript { .api }
/**
 * Check if value is an array
 * @param x - Value to check
 * @returns true if x is an array
 */
function isArray(x: any): boolean;

/**
 * Check if value is a Matrix
 * @param x - Value to check
 * @returns true if x is a Matrix instance
 */
function isMatrix(x: any): boolean;

/**
 * Check if value is a DenseMatrix
 * @param x - Value to check
 * @returns true if x is a DenseMatrix instance
 */
function isDenseMatrix(x: any): boolean;

/**
 * Check if value is a SparseMatrix
 * @param x - Value to check
 * @returns true if x is a SparseMatrix instance
 */
function isSparseMatrix(x: any): boolean;

/**
 * Check if value is a collection (Array or Matrix)
 * @param x - Value to check
 * @returns true if x is array or matrix
 */
function isCollection(x: any): boolean;

/**
 * Check if value is a Range
 * @param x - Value to check
 * @returns true if x is a Range instance
 */
function isRange(x: any): boolean;

/**
 * Check if value is an Index
 * @param x - Value to check
 * @returns true if x is an Index instance
 */
function isIndex(x: any): boolean;
```

#### Basic Type Checkers

```typescript { .api }
/**
 * Check if value is a string
 * @param x - Value to check
 * @returns true if x is a string
 */
function isString(x: any): boolean;

/**
 * Check if value is a boolean
 * @param x - Value to check
 * @returns true if x is a boolean
 */
function isBoolean(x: any): boolean;

/**
 * Check if value is null
 * @param x - Value to check
 * @returns true if x is null
 */
function isNull(x: any): boolean;

/**
 * Check if value is undefined
 * @param x - Value to check
 * @returns true if x is undefined
 */
function isUndefined(x: any): boolean;

/**
 * Check if value is a function
 * @param x - Value to check
 * @returns true if x is a function
 */
function isFunction(x: any): boolean;

/**
 * Check if value is a Date
 * @param x - Value to check
 * @returns true if x is a Date instance
 */
function isDate(x: any): boolean;

/**
 * Check if value is a RegExp
 * @param x - Value to check
 * @returns true if x is a RegExp instance
 */
function isRegExp(x: any): boolean;

/**
 * Check if value is an object
 * @param x - Value to check
 * @returns true if x is an object (not array or null)
 */
function isObject(x: any): boolean;

/**
 * Check if value is a Map
 * @param x - Value to check
 * @returns true if x is a Map instance
 */
function isMap(x: any): boolean;
```

#### Math.js Specific Type Checkers

```typescript { .api }
/**
 * Check if value is a Unit
 * @param x - Value to check
 * @returns true if x is a Unit instance
 */
function isUnit(x: any): boolean;

/**
 * Check if value is a ResultSet
 * @param x - Value to check
 * @returns true if x is a ResultSet instance
 */
function isResultSet(x: any): boolean;

/**
 * Check if value is a Help object
 * @param x - Value to check
 * @returns true if x is a Help instance
 */
function isHelp(x: any): boolean;

/**
 * Check if value is a Chain
 * @param x - Value to check
 * @returns true if x is a Chain instance
 */
function isChain(x: any): boolean;
```

#### AST Node Type Checkers

```typescript { .api }
/**
 * Check if value is an AST Node
 * @param x - Value to check
 * @returns true if x is a Node instance
 */
function isNode(x: any): boolean;

/**
 * Check if value is an AccessorNode
 * @param x - Value to check
 * @returns true if x is an AccessorNode instance
 */
function isAccessorNode(x: any): boolean;

/**
 * Check if value is an ArrayNode
 * @param x - Value to check
 * @returns true if x is an ArrayNode instance
 */
function isArrayNode(x: any): boolean;

/**
 * Check if value is an AssignmentNode
 * @param x - Value to check
 * @returns true if x is an AssignmentNode instance
 */
function isAssignmentNode(x: any): boolean;

/**
 * Check if value is a BlockNode
 * @param x - Value to check
 * @returns true if x is a BlockNode instance
 */
function isBlockNode(x: any): boolean;

/**
 * Check if value is a ConditionalNode
 * @param x - Value to check
 * @returns true if x is a ConditionalNode instance
 */
function isConditionalNode(x: any): boolean;

/**
 * Check if value is a ConstantNode
 * @param x - Value to check
 * @returns true if x is a ConstantNode instance
 */
function isConstantNode(x: any): boolean;

/**
 * Check if value is a FunctionAssignmentNode
 * @param x - Value to check
 * @returns true if x is a FunctionAssignmentNode instance
 */
function isFunctionAssignmentNode(x: any): boolean;

/**
 * Check if value is a FunctionNode
 * @param x - Value to check
 * @returns true if x is a FunctionNode instance
 */
function isFunctionNode(x: any): boolean;

/**
 * Check if value is an IndexNode
 * @param x - Value to check
 * @returns true if x is an IndexNode instance
 */
function isIndexNode(x: any): boolean;

/**
 * Check if value is an ObjectNode
 * @param x - Value to check
 * @returns true if x is an ObjectNode instance
 */
function isObjectNode(x: any): boolean;

/**
 * Check if value is an OperatorNode
 * @param x - Value to check
 * @returns true if x is an OperatorNode instance
 */
function isOperatorNode(x: any): boolean;

/**
 * Check if value is a ParenthesisNode
 * @param x - Value to check
 * @returns true if x is a ParenthesisNode instance
 */
function isParenthesisNode(x: any): boolean;

/**
 * Check if value is a RangeNode
 * @param x - Value to check
 * @returns true if x is a RangeNode instance
 */
function isRangeNode(x: any): boolean;

/**
 * Check if value is a RelationalNode
 * @param x - Value to check
 * @returns true if x is a RelationalNode instance
 */
function isRelationalNode(x: any): boolean;

/**
 * Check if value is a SymbolNode
 * @param x - Value to check
 * @returns true if x is a SymbolNode instance
 */
function isSymbolNode(x: any): boolean;
```

### Type Utility Functions

Additional utilities for working with types.

```typescript { .api }
/**
 * Get type name of value
 * @param x - Value to check
 * @returns String describing the type (e.g., 'number', 'BigNumber', 'Complex', 'Array', 'Matrix')
 */
function typeOf(x: any): string;

/**
 * Convert value to numeric type
 * @param x - Value to convert
 * @param type - Target type name ('number', 'BigNumber', 'Fraction')
 * @returns Converted numeric value
 */
function numeric(x: any, type?: string): number | BigNumber | Fraction;
```

## Types

### Format Options

```typescript { .api }
interface FormatOptions {
  /** Number notation: 'fixed', 'exponential', 'engineering', 'auto' */
  notation?: 'fixed' | 'exponential' | 'engineering' | 'auto';

  /** Number of significant digits or decimal places */
  precision?: number;

  /** Lower bound for exponential notation */
  lowerExp?: number;

  /** Upper bound for exponential notation */
  upperExp?: number;

  /** Fraction format: 'ratio' or 'decimal' */
  fraction?: 'ratio' | 'decimal';

  /** Callback to format custom types */
  handler?: (node: any, options: FormatOptions) => string;
}
```

### Unit Definition

```typescript { .api }
interface UnitDefinition {
  /** Unit name */
  name: string;

  /** Unit prefix (k, M, m, μ, etc.) */
  prefix?: string;

  /** Unit power/exponent */
  power?: number;
}
```

### Range Interface

```typescript { .api }
interface IRange {
  /** Start of range */
  start: number;

  /** End of range */
  end: number;

  /** Step size */
  step: number;
}
```

## Usage Examples

### Creating and Converting Types

```typescript
import { bignumber, complex, fraction, unit, matrix, sparse } from 'mathjs';

// BigNumber for arbitrary precision
const big = bignumber('1.234567890123456789012345678901234567890');
const converted = bignumber(123.456);

// Complex numbers
const c1 = complex(3, 4);           // 3 + 4i
const c2 = complex('2 - 5i');       // Parse from string
const c3 = complex({ re: 1, im: 2 }); // From object

// Fractions
const f1 = fraction(1, 3);          // 1/3
const f2 = fraction(0.333);         // Converts decimal to fraction
const f3 = fraction('2/5');         // Parse from string

// Units
const distance = unit(5, 'km');
const speed = unit(60, 'mile/hour');
const time = unit(2.5, 'hours');

// Matrices
const dense = matrix([[1, 2], [3, 4]]);
const sparse = sparse([[0, 0, 3], [0, 0, 0], [0, 4, 0]]);
```

### Working with Type Classes

```typescript
import { BigNumber, Complex, Fraction, Unit } from 'mathjs';

// BigNumber operations
const a = new BigNumber('0.1');
const b = new BigNumber('0.2');
const sum = a.add(b);                    // 0.3 (exact)
const isGreater = sum.greaterThan(0.25); // true

// Complex number operations
const z1 = new Complex(3, 4);
const z2 = new Complex(1, -2);
const polar = z1.toPolar();              // { r: 5, phi: 0.927... }

// Fraction operations
const f1 = new Fraction(1, 3);
const f2 = new Fraction(1, 6);
const result = f1.add(f2);               // 1/2
const decimal = result.valueOf();         // 0.5

// Unit operations
const length = new Unit(100, 'cm');
const meters = length.to('m');           // 1 m
const inches = length.to('inch');        // 39.37... inch
```

### Type Checking

```typescript
import {
  isNumber, isBigNumber, isComplex, isFraction,
  isMatrix, isUnit, typeOf
} from 'mathjs';

const values = [
  42,
  bignumber(42),
  complex(3, 4),
  fraction(1, 2),
  unit(5, 'kg'),
  matrix([[1, 2]]),
];

values.forEach(val => {
  console.log(`Type: ${typeOf(val)}`);
  console.log(`isNumber: ${isNumber(val)}`);
  console.log(`isBigNumber: ${isBigNumber(val)}`);
  console.log(`isComplex: ${isComplex(val)}`);
  console.log(`isFraction: ${isFraction(val)}`);
  console.log(`isUnit: ${isUnit(val)}`);
  console.log(`isMatrix: ${isMatrix(val)}`);
});
```

### Matrix Operations

```typescript
import { matrix, sparse, index } from 'mathjs';

// Dense matrix
const dense = matrix([
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]);

console.log(dense.size());           // [3, 3]
console.log(dense.get([1, 1]));     // 5
dense.set([0, 0], 10);              // Set element

// Subset operations
const idx = index([0, 1], [1, 2]);
const subset = dense.subset(idx);    // Get subset

// Sparse matrix (efficient for large matrices with many zeros)
const largeSparse = sparse([
  [1, 0, 0, 0],
  [0, 0, 0, 2],
  [0, 0, 3, 0],
  [0, 4, 0, 0]
]);

console.log(largeSparse.nonZeros()); // 4
```

### Unit Conversions and Operations

```typescript
import { unit, createUnit } from 'mathjs';

// Basic conversions
const distance = unit(5, 'km');
console.log(distance.to('mile').toString());      // ~3.107 mile
console.log(distance.toSI().toString());          // 5000 m

// Unit arithmetic
const time = unit(2, 'hour');
const speed = distance.div(time);                 // 2.5 km/hour
const meters_per_sec = speed.to('m/s');          // ~0.694 m/s

// Split compound units
const duration = unit(1.5, 'hour');
const parts = duration.splitUnit(['hour', 'minute']);
// Returns [1 hour, 30 minute]

// Define custom units
createUnit('widget', '5 kg');
createUnit('wobble', { definition: '10 widget', offset: 0 });
const w = unit(2, 'wobble');                     // 100 kg
```

### Complex Type Conversions

```typescript
import {
  bignumber, complex, fraction, number,
  matrix, typeOf
} from 'mathjs';

// Automatic type conversion in operations
const result = bignumber(0.1)
  .add(bignumber(0.2));                          // 0.3 (exact)

// Convert between types
const f = fraction(0.333333);                    // Approximates to 1/3
const b = bignumber(f);                          // Convert to BigNumber
const n = number(b);                             // Convert to number

// Type preservation in matrices
const bigMatrix = matrix([
  [bignumber(1), bignumber(2)],
  [bignumber(3), bignumber(4)]
]);

console.log(typeOf(bigMatrix.get([0, 0])));     // 'BigNumber'
```
