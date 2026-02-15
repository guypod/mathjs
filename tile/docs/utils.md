# Utility Functions

Math.js provides 60+ utility functions for type checking, value inspection, cloning, and numerical analysis. These functions help with type validation, numeric properties, AST node type checking, and advanced numerical methods.

## Core Imports

```javascript { .api }
import {
  // Type inspection
  typeOf, clone, hasNumericValue, numeric,

  // Math.js type checking
  isNumber, isBigNumber, isBigInt, isComplex, isFraction,
  isUnit, isString, isArray, isMatrix, isDenseMatrix, isSparseMatrix,
  isRange, isIndex, isBoolean, isResultSet, isHelp, isCollection,
  isFunction, isDate, isRegExp, isObject, isNull, isUndefined,
  isMap, isPartitionedMap, isObjectWrappingMap, isChain,

  // AST node type checking
  isNode, isAccessorNode, isArrayNode, isAssignmentNode, isBlockNode,
  isConditionalNode, isConstantNode, isFunctionAssignmentNode, isFunctionNode,
  isIndexNode, isObjectNode, isOperatorNode, isParenthesisNode,
  isRangeNode, isRelationalNode, isSymbolNode,

  // Numeric property checks
  isInteger, isNumeric, isNaN, isFinite,
  isPositive, isNegative, isZero, isPrime, isBounded,

  // Numerical methods
  solveODE
} from 'mathjs';
```

## Type Inspection

### typeOf

Returns the type name of a value as a string.

```typescript { .api }
function typeOf(x: any): string;
```

**Parameters:**
- `x`: Value to get type of

**Returns:** Type name string

**Usage Examples:**

```javascript
import { typeOf, complex, bignumber, unit, matrix } from 'mathjs';

typeOf(5);                        // 'number'
typeOf('hello');                  // 'string'
typeOf(true);                     // 'boolean'
typeOf(null);                     // 'null'
typeOf(undefined);                // 'undefined'
typeOf([]);                       // 'Array'
typeOf({});                       // 'Object'

// Math.js types
typeOf(complex(2, 3));            // 'Complex'
typeOf(bignumber(123));           // 'BigNumber'
typeOf(unit('5 cm'));             // 'Unit'
typeOf(matrix([1, 2, 3]));        // 'DenseMatrix'

// Useful for type-based logic
if (typeOf(x) === 'Complex') {
  // Handle complex number
}
```

### clone

Creates a deep clone of a value.

```typescript { .api }
function clone<T>(x: T): T;
```

**Parameters:**
- `x`: Value to clone (any type)

**Returns:** Deep copy of the value

**Usage Examples:**

```javascript
import { clone, complex, matrix } from 'mathjs';

// Clone primitives
clone(5);                         // 5
clone('hello');                   // 'hello'

// Clone arrays (deep copy)
const arr = [1, 2, [3, 4]];
const cloned = clone(arr);
cloned[2][0] = 99;
arr[2][0];                        // 3 (original unchanged)

// Clone objects
const obj = {a: 1, b: {c: 2}};
const clonedObj = clone(obj);
clonedObj.b.c = 99;
obj.b.c;                          // 2 (original unchanged)

// Clone Math.js types
clone(complex(2, 3));             // Complex {re: 2, im: 3}
clone(matrix([1, 2, 3]));         // DenseMatrix [1, 2, 3]

// Useful for immutable operations
const original = [1, 2, 3];
const modified = clone(original);
modified.push(4);                 // original remains [1, 2, 3]
```

## Math.js Type Checking Functions

Math.js provides comprehensive type checking predicates for all its data types. These functions return true if the value is of the specified type, false otherwise.

### isNumber

Checks if a value is a JavaScript number.

```typescript { .api }
function isNumber(x: unknown): x is number;
```

**Usage Examples:**

```javascript
import { isNumber, bignumber, complex } from 'mathjs';

isNumber(5);                      // true
isNumber(3.14);                   // true
isNumber(Infinity);               // true
isNumber(NaN);                    // true

isNumber('5');                    // false
isNumber(bignumber(5));           // false
isNumber(complex(2, 3));          // false
```

### isBigNumber

Checks if a value is a BigNumber (arbitrary precision number).

```typescript { .api }
function isBigNumber(x: unknown): x is BigNumber;
```

**Usage Examples:**

```javascript
import { isBigNumber, bignumber } from 'mathjs';

isBigNumber(bignumber(123));      // true
isBigNumber(bignumber('1e100'));  // true

isBigNumber(123);                 // false
isBigNumber('123');               // false
```

### isBigInt

Checks if a value is a JavaScript BigInt.

```typescript { .api }
function isBigInt(x: unknown): x is bigint;
```

**Usage Examples:**

```javascript
import { isBigInt } from 'mathjs';

isBigInt(123n);                   // true
isBigInt(BigInt(456));            // true

isBigInt(123);                    // false
isBigInt('123');                  // false
```

### isComplex

Checks if a value is a Complex number.

```typescript { .api }
function isComplex(x: unknown): x is Complex;
```

**Usage Examples:**

```javascript
import { isComplex, complex } from 'mathjs';

isComplex(complex(2, 3));         // true
isComplex(complex(5, 0));         // true

isComplex(5);                     // false
isComplex({ re: 2, im: 3 });      // false (plain object)
```

### isFraction

Checks if a value is a Fraction (rational number).

```typescript { .api }
function isFraction(x: unknown): x is Fraction;
```

**Usage Examples:**

```javascript
import { isFraction, fraction } from 'mathjs';

isFraction(fraction(1, 3));       // true
isFraction(fraction(5, 2));       // true

isFraction(0.5);                  // false
isFraction('1/3');                // false
```

### isUnit

Checks if a value is a Unit (physical unit with value).

```typescript { .api }
function isUnit(x: unknown): x is Unit;
```

**Usage Examples:**

```javascript
import { isUnit, unit } from 'mathjs';

isUnit(unit('5 cm'));             // true
isUnit(unit('10 kg'));            // true

isUnit(5);                        // false
isUnit('5 cm');                   // false
```

### isString

Checks if a value is a string.

```typescript { .api }
function isString(x: unknown): x is string;
```

**Usage Examples:**

```javascript
import { isString } from 'mathjs';

isString('hello');                // true
isString('123');                  // true
isString('');                     // true

isString(123);                    // false
isString(null);                   // false
```

### isArray

Checks if a value is a JavaScript array.

```typescript { .api }
const isArray: ArrayConstructor['isArray'];
```

**Usage Examples:**

```javascript
import { isArray } from 'mathjs';

isArray([1, 2, 3]);               // true
isArray([[1, 2], [3, 4]]);        // true
isArray([]);                      // true

isArray(matrix([1, 2, 3]));       // false
isArray('123');                   // false
```

### isMatrix

Checks if a value is a Matrix (DenseMatrix or SparseMatrix).

```typescript { .api }
function isMatrix(x: unknown): x is Matrix;
```

**Usage Examples:**

```javascript
import { isMatrix, matrix, sparse } from 'mathjs';

isMatrix(matrix([1, 2, 3]));      // true
isMatrix(sparse([[0, 1], [2, 0]])); // true

isMatrix([1, 2, 3]);              // false
```

### isDenseMatrix

Checks if a value is a DenseMatrix.

```typescript { .api }
function isDenseMatrix(x: unknown): x is Matrix;
```

**Usage Examples:**

```javascript
import { isDenseMatrix, matrix, sparse } from 'mathjs';

isDenseMatrix(matrix([1, 2, 3])); // true

isDenseMatrix(sparse([[1, 2]]));  // false
isDenseMatrix([1, 2, 3]);         // false
```

### isSparseMatrix

Checks if a value is a SparseMatrix.

```typescript { .api }
function isSparseMatrix(x: unknown): x is Matrix;
```

**Usage Examples:**

```javascript
import { isSparseMatrix, matrix, sparse } from 'mathjs';

isSparseMatrix(sparse([[0, 1]])); // true

isSparseMatrix(matrix([1, 2]));   // false
isSparseMatrix([1, 2, 3]);        // false
```

### isRange

Checks if a value is a Range object.

```typescript { .api }
function isRange(x: unknown): boolean;
```

**Usage Examples:**

```javascript
import { isRange, range } from 'mathjs';

isRange(range(0, 10));            // true
isRange(range(1, 10, 2));         // true

isRange([0, 1, 2, 3]);            // false
```

### isIndex

Checks if a value is an Index object (used for matrix subsetting).

```typescript { .api }
function isIndex(x: unknown): x is Index;
```

**Usage Examples:**

```javascript
import { isIndex, index } from 'mathjs';

isIndex(index(0, 1));             // true
isIndex(index(range(0, 10)));     // true

isIndex([0, 1]);                  // false
```

### isBoolean

Checks if a value is a boolean.

```typescript { .api }
function isBoolean(x: unknown): x is boolean;
```

**Usage Examples:**

```javascript
import { isBoolean } from 'mathjs';

isBoolean(true);                  // true
isBoolean(false);                 // true

isBoolean(1);                     // false
isBoolean('true');                // false
```

### isResultSet

Checks if a value is a ResultSet (result from Parser.evaluate with multiple expressions).

```typescript { .api }
function isResultSet(x: unknown): x is ResultSet;
```

**Usage Examples:**

```javascript
import { isResultSet, parser } from 'mathjs';

const p = parser();
const result = p.evaluate(['a = 5', 'b = 10']);
isResultSet(result);              // true

isResultSet([5, 10]);             // false
```

### isHelp

Checks if a value is a Help object (returned by help() function).

```typescript { .api }
function isHelp(x: unknown): x is Help;
```

**Usage Examples:**

```javascript
import { isHelp, help } from 'mathjs';

isHelp(help('sin'));              // true
isHelp(help('add'));              // true

isHelp('sin');                    // false
```

### isCollection

Checks if a value is a collection (Matrix or Array).

```typescript { .api }
function isCollection(x: unknown): x is Matrix | any[];
```

**Usage Examples:**

```javascript
import { isCollection, matrix } from 'mathjs';

isCollection([1, 2, 3]);          // true
isCollection([[1, 2], [3, 4]]);   // true
isCollection(matrix([1, 2, 3]));  // true

isCollection(5);                  // false
isCollection('123');              // false
```

### isFunction

Checks if a value is a JavaScript function.

```typescript { .api }
function isFunction(x: unknown): boolean;
```

**Usage Examples:**

```javascript
import { isFunction } from 'mathjs';

isFunction(() => 5);              // true
isFunction(function() {});        // true
isFunction(Math.sin);             // true

isFunction('function');           // false
```

### isDate

Checks if a value is a JavaScript Date object.

```typescript { .api }
function isDate(x: unknown): x is Date;
```

**Usage Examples:**

```javascript
import { isDate } from 'mathjs';

isDate(new Date());               // true
isDate(new Date('2024-01-01'));   // true

isDate('2024-01-01');             // false
isDate(1234567890);               // false
```

### isRegExp

Checks if a value is a JavaScript RegExp object.

```typescript { .api }
function isRegExp(x: unknown): x is RegExp;
```

**Usage Examples:**

```javascript
import { isRegExp } from 'mathjs';

isRegExp(/abc/);                  // true
isRegExp(new RegExp('test'));     // true

isRegExp('abc');                  // false
```

### isObject

Checks if a value is a plain JavaScript object.

```typescript { .api }
function isObject(x: unknown): boolean;
```

**Usage Examples:**

```javascript
import { isObject } from 'mathjs';

isObject({a: 1, b: 2});           // true
isObject({});                     // true

isObject([1, 2]);                 // false
isObject(null);                   // false
```

### isNull

Checks if a value is null.

```typescript { .api }
function isNull(x: unknown): x is null;
```

**Usage Examples:**

```javascript
import { isNull } from 'mathjs';

isNull(null);                     // true

isNull(undefined);                // false
isNull(0);                        // false
```

### isUndefined

Checks if a value is undefined.

```typescript { .api }
function isUndefined(x: unknown): x is undefined;
```

**Usage Examples:**

```javascript
import { isUndefined } from 'mathjs';

isUndefined(undefined);           // true

isUndefined(null);                // false
isUndefined(0);                   // false
```

### isMap

Checks if a value is a JavaScript Map.

```typescript { .api }
function isMap<T, U>(x: unknown): x is Map<T, U>;
```

**Usage Examples:**

```javascript
import { isMap } from 'mathjs';

isMap(new Map());                 // true
isMap(new Map([[1, 'a']]));       // true

isMap({});                        // false
```

### isPartitionedMap

Checks if a value is a PartitionedMap (internal mathjs type).

```typescript { .api }
function isPartitionedMap<T, U>(x: unknown): x is PartitionedMap<T, U>;
```

### isObjectWrappingMap

Checks if a value is an ObjectWrappingMap (internal mathjs type).

```typescript { .api }
function isObjectWrappingMap<T extends string | number | symbol, U>(
  x: unknown
): x is ObjectWrappingMap<T, U>;
```

### isChain

Checks if a value is a Chain object (from chain() function).

```typescript { .api }
function isChain(x: unknown): x is MathJsChain<unknown>;
```

**Usage Examples:**

```javascript
import { isChain, chain } from 'mathjs';

isChain(chain(5));                // true
isChain(chain([1, 2, 3]));        // true

isChain(5);                       // false
```

## AST Node Type Checking Functions

Math.js provides type checking functions for all AST (Abstract Syntax Tree) node types created by the expression parser. These are useful when working with parsed expressions programmatically.

### isNode

Checks if a value is any type of AST node.

```typescript { .api }
function isNode(x: unknown): x is MathNode;
```

**Usage Examples:**

```javascript
import { isNode, parse } from 'mathjs';

const node = parse('x + 5');
isNode(node);                     // true

isNode('x + 5');                  // false
```

### isAccessorNode

Checks if a value is an AccessorNode (property or array access like `obj.prop` or `arr[i]`).

```typescript { .api }
function isAccessorNode(x: unknown): x is AccessorNode;
```

**Usage Examples:**

```javascript
import { isAccessorNode, parse } from 'mathjs';

const node = parse('obj.prop');
isAccessorNode(node);             // true

const addNode = parse('1 + 2');
isAccessorNode(addNode);          // false
```

### isArrayNode

Checks if a value is an ArrayNode (array literal like `[1, 2, 3]`).

```typescript { .api }
function isArrayNode(x: unknown): x is ArrayNode;
```

**Usage Examples:**

```javascript
import { isArrayNode, parse } from 'mathjs';

const node = parse('[1, 2, 3]');
isArrayNode(node);                // true
```

### isAssignmentNode

Checks if a value is an AssignmentNode (variable assignment like `x = 5`).

```typescript { .api }
function isAssignmentNode(x: unknown): x is AssignmentNode;
```

**Usage Examples:**

```javascript
import { isAssignmentNode, parse } from 'mathjs';

const node = parse('x = 5');
isAssignmentNode(node);           // true
```

### isBlockNode

Checks if a value is a BlockNode (block of statements).

```typescript { .api }
function isBlockNode(x: unknown): x is BlockNode;
```

### isConditionalNode

Checks if a value is a ConditionalNode (ternary expression like `condition ? true : false`).

```typescript { .api }
function isConditionalNode(x: unknown): x is ConditionalNode;
```

**Usage Examples:**

```javascript
import { isConditionalNode, parse } from 'mathjs';

const node = parse('x > 0 ? x : -x');
isConditionalNode(node);          // true
```

### isConstantNode

Checks if a value is a ConstantNode (numeric or string constant).

```typescript { .api }
function isConstantNode(x: unknown): x is ConstantNode;
```

**Usage Examples:**

```javascript
import { isConstantNode, parse } from 'mathjs';

const node = parse('42');
isConstantNode(node);             // true
```

### isFunctionAssignmentNode

Checks if a value is a FunctionAssignmentNode (function definition like `f(x) = x^2`).

```typescript { .api }
function isFunctionAssignmentNode(x: unknown): x is FunctionAssignmentNode;
```

**Usage Examples:**

```javascript
import { isFunctionAssignmentNode, parse } from 'mathjs';

const node = parse('f(x) = x^2');
isFunctionAssignmentNode(node);   // true
```

### isFunctionNode

Checks if a value is a FunctionNode (function call like `sin(x)`).

```typescript { .api }
function isFunctionNode(x: unknown): x is FunctionNode;
```

**Usage Examples:**

```javascript
import { isFunctionNode, parse } from 'mathjs';

const node = parse('sin(x)');
isFunctionNode(node);             // true
```

### isIndexNode

Checks if a value is an IndexNode (array indices).

```typescript { .api }
function isIndexNode(x: unknown): x is IndexNode;
```

### isObjectNode

Checks if a value is an ObjectNode (object literal like `{a: 1, b: 2}`).

```typescript { .api }
function isObjectNode(x: unknown): x is ObjectNode;
```

**Usage Examples:**

```javascript
import { isObjectNode, parse } from 'mathjs';

const node = parse('{a: 1, b: 2}');
isObjectNode(node);               // true
```

### isOperatorNode

Checks if a value is an OperatorNode (binary or unary operator like `+`, `-`, `*`).

```typescript { .api }
function isOperatorNode(x: unknown): x is OperatorNode;
```

**Usage Examples:**

```javascript
import { isOperatorNode, parse } from 'mathjs';

const node = parse('a + b');
isOperatorNode(node);             // true
```

### isParenthesisNode

Checks if a value is a ParenthesisNode (grouped expression like `(x + y)`).

```typescript { .api }
function isParenthesisNode(x: unknown): x is ParenthesisNode;
```

**Usage Examples:**

```javascript
import { isParenthesisNode, parse } from 'mathjs';

const node = parse('(x + 5)');
isParenthesisNode(node);          // true
```

### isRangeNode

Checks if a value is a RangeNode (range notation like `start:end:step`).

```typescript { .api }
function isRangeNode(x: unknown): x is RangeNode;
```

**Usage Examples:**

```javascript
import { isRangeNode, parse } from 'mathjs';

const node = parse('1:10');
isRangeNode(node);                // true
```

### isRelationalNode

Checks if a value is a RelationalNode (chained comparisons like `a < b < c`).

```typescript { .api }
function isRelationalNode(x: unknown): x is RelationalNode;
```

**Usage Examples:**

```javascript
import { isRelationalNode, parse } from 'mathjs';

const node = parse('a < b < c');
isRelationalNode(node);           // true
```

### isSymbolNode

Checks if a value is a SymbolNode (variable name like `x`, `myVar`).

```typescript { .api }
function isSymbolNode(x: unknown): x is SymbolNode;
```

**Usage Examples:**

```javascript
import { isSymbolNode, parse } from 'mathjs';

const node = parse('x');
isSymbolNode(node);               // true
```

## Numeric Type Functions

### hasNumericValue

Checks if a value has a numeric value (can be converted to a number).

```typescript { .api }
function hasNumericValue(x: any): boolean;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if value has numeric representation, false otherwise

**Usage Examples:**

```javascript
import { hasNumericValue, complex, unit, bignumber } from 'mathjs';

hasNumericValue(5);               // true
hasNumericValue('5');             // true (can be converted)
hasNumericValue(complex(2, 3));   // true
hasNumericValue(unit('5 cm'));    // true
hasNumericValue(bignumber(10));   // true

hasNumericValue('hello');         // false
hasNumericValue(null);            // false
hasNumericValue(undefined);       // false
hasNumericValue([]);              // false
```

### isNumeric

Checks if a value is a numeric type (number, BigNumber, Fraction, or Complex).

```typescript { .api }
function isNumeric(x: any): boolean;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x is a numeric type, false otherwise

**Usage Examples:**

```javascript
import { isNumeric, complex, bignumber, fraction } from 'mathjs';

isNumeric(5);                     // true
isNumeric(bignumber(123));        // true
isNumeric(fraction(1, 3));        // true
isNumeric(complex(2, 3));         // true

isNumeric('5');                   // false (string, not numeric type)
isNumeric([1, 2, 3]);             // false
isNumeric(unit('5 cm'));          // false (Unit is not purely numeric)
```

### numeric

Converts a value to a specified numeric type.

```typescript { .api }
function numeric(x: string | Unit | boolean | Array | Matrix | null, type?: string): number | BigNumber | Fraction;
```

**Parameters:**
- `x`: Value to convert
- `type`: Target type: 'number', 'BigNumber', or 'Fraction' (default: 'number')

**Returns:** Converted numeric value

**Usage Examples:**

```javascript
import { numeric, unit } from 'mathjs';

// String to number
numeric('123');                   // 123
numeric('3.14');                  // 3.14

// Boolean to number
numeric(true);                    // 1
numeric(false);                   // 0

// Unit to number (base unit value)
numeric(unit('5 cm'));            // 0.05 (meters)

// Specify target type
numeric('123', 'BigNumber');      // BigNumber 123
numeric('1/3', 'Fraction');       // Fraction 1/3

// Arrays (element-wise)
numeric(['1', '2', '3']);         // [1, 2, 3]
```

## Numeric Property Checks

### isInteger

Checks if a value is an integer.

```typescript { .api }
function isInteger(x: number | BigNumber | Fraction | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x is an integer, false otherwise

**Usage Examples:**

```javascript
import { isInteger, bignumber, fraction } from 'mathjs';

isInteger(5);                     // true
isInteger(5.0);                   // true
isInteger(5.5);                   // false
isInteger(-3);                    // true

// Works with BigNumber
isInteger(bignumber('123'));      // true
isInteger(bignumber('123.456'));  // false

// Works with Fraction
isInteger(fraction(4, 2));        // true (reduces to 2)
isInteger(fraction(5, 2));        // false (2.5)

// Element-wise for arrays
isInteger([1, 2.5, 3, 4.0]);
// [true, false, true, true]
```

### isPositive

Checks if a value is positive (greater than zero).

```typescript { .api }
function isPositive(x: number | BigNumber | Fraction | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x > 0, false otherwise

**Usage Examples:**

```javascript
import { isPositive, bignumber } from 'mathjs';

isPositive(5);                    // true
isPositive(0);                    // false
isPositive(-5);                   // false
isPositive(0.001);                // true

// Works with all numeric types
isPositive(bignumber('1e10'));    // true

// Element-wise for arrays
isPositive([1, 0, -1, 2]);
// [true, false, false, true]
```

### isNegative

Checks if a value is negative (less than zero).

```typescript { .api }
function isNegative(x: number | BigNumber | Fraction | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x < 0, false otherwise

**Usage Examples:**

```javascript
import { isNegative } from 'mathjs';

isNegative(-5);                   // true
isNegative(0);                    // false
isNegative(5);                    // false
isNegative(-0.001);               // true

// Element-wise for arrays
isNegative([1, 0, -1, -2]);
// [false, false, true, true]
```

### isZero

Checks if a value is zero or approximately zero.

```typescript { .api }
function isZero(x: number | BigNumber | Fraction | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x equals zero, false otherwise

**Usage Examples:**

```javascript
import { isZero, complex } from 'mathjs';

isZero(0);                        // true
isZero(1);                        // false
isZero(-0);                       // true

// Very small numbers may be treated as zero
isZero(1e-16);                    // true (within epsilon)

// Complex zero
isZero(complex(0, 0));            // true

// Element-wise for arrays
isZero([0, 1, 0, 2]);
// [true, false, true, false]
```

### isNaN

Checks if a value is NaN (Not a Number).

```typescript { .api }
function isNaN(x: number | BigNumber | Fraction | Complex | Unit | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x is NaN, false otherwise

**Usage Examples:**

```javascript
import { isNaN } from 'mathjs';

isNaN(NaN);                       // true
isNaN(5);                         // false
isNaN(0 / 0);                     // true
isNaN(Infinity);                  // false

// Element-wise for arrays
isNaN([1, NaN, 2, 0/0]);
// [false, true, false, true]
```

### isFinite

Checks if a value is finite (not Infinity or NaN).

```typescript { .api }
function isFinite(x: number | BigNumber | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x is finite, false otherwise

**Usage Examples:**

```javascript
import { isFinite } from 'mathjs';

isFinite(5);                      // true
isFinite(Infinity);               // false
isFinite(-Infinity);              // false
isFinite(NaN);                    // false
isFinite(1e308);                  // true

// Element-wise for arrays
isFinite([1, Infinity, 2, NaN]);
// [true, false, true, false]
```

### isBounded

Checks if a value is bounded (finite and not NaN).

```typescript { .api }
function isBounded(x: number | BigNumber | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check

**Returns:** true if x is bounded (finite and valid), false otherwise

**Usage Examples:**

```javascript
import { isBounded } from 'mathjs';

isBounded(5);                     // true
isBounded(Infinity);              // false
isBounded(-Infinity);             // false
isBounded(NaN);                   // false
isBounded(1e308);                 // true

// Element-wise for arrays
isBounded([1, Infinity, 2, NaN]);
// [true, false, true, false]
```

### isPrime

Checks if a value is a prime number.

```typescript { .api }
function isPrime(x: number | BigNumber | Array | Matrix): boolean | Array | Matrix;
```

**Parameters:**
- `x`: Value to check (must be integer)

**Returns:** true if x is prime, false otherwise

**Usage Examples:**

```javascript
import { isPrime, bignumber } from 'mathjs';

isPrime(2);                       // true
isPrime(3);                       // true
isPrime(4);                       // false
isPrime(17);                      // true
isPrime(1);                       // false (1 is not prime)
isPrime(-5);                      // false (negatives not prime)

// Works with BigNumber for large primes
isPrime(bignumber('2147483647')); // true (Mersenne prime)

// Element-wise for arrays
isPrime([2, 3, 4, 5, 6]);
// [true, true, false, true, false]
```

## Numerical Methods

### solveODE

Solves an Ordinary Differential Equation (ODE) using numerical methods.

```typescript { .api }
function solveODE(
  func: (x: number, y: number | number[]) => number | number[],
  x0: number,
  options?: ODEOptions
): { x: number[], y: number[] | number[][] };
```

**Parameters:**
- `func`: ODE function dy/dx = f(x, y) or system of ODEs
- `x0`: Initial x value
- `options`: Solver options

**Options:**
- `y0`: Initial y value(s) (default: 0)
- `xEnd`: Final x value (default: 1)
- `step`: Step size (default: 0.1)
- `method`: 'RK4' (Runge-Kutta 4th order, default) or 'Euler'

**Returns:** Object with x array and y array (or 2D array for systems)

**Usage Examples:**

```javascript
import { solveODE } from 'mathjs';

// Solve dy/dx = x (solution: y = x^2/2)
const result = solveODE(
  (x, y) => x,
  0,
  { y0: 0, xEnd: 2, step: 0.5 }
);
// result.x = [0, 0.5, 1.0, 1.5, 2.0]
// result.y ≈ [0, 0.125, 0.5, 1.125, 2.0]

// Solve dy/dx = y (exponential growth)
const expResult = solveODE(
  (x, y) => y,
  0,
  { y0: 1, xEnd: 1, step: 0.25 }
);
// Solution: y = e^x

// System of ODEs (dx/dt = y, dy/dt = -x)
const system = solveODE(
  (t, [x, y]) => [y, -x],
  0,
  { y0: [1, 0], xEnd: 6.28, step: 0.1, method: 'RK4' }
);
// Solution: simple harmonic oscillator

// Higher accuracy with smaller step
const accurate = solveODE(
  (x, y) => x,
  0,
  { y0: 0, xEnd: 2, step: 0.1 }
);
```

## Types

```typescript { .api }
interface ODEOptions {
  y0?: number | number[];
  xEnd?: number;
  step?: number;
  method?: 'RK4' | 'Euler';
}
```

## Common Types

```typescript { .api }
type MathNumericType = number | BigNumber | bigint | Fraction | Complex;
type MathArray<T> = T[] | Array<MathArray<T>>;
```
