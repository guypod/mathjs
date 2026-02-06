# Type Checking Utilities

Comprehensive type checking functions for all Math.js types including numeric types, collections, AST nodes, and value properties. Essential for runtime type validation and conditional logic.

## Capabilities

### Numeric Type Checking

Check for specific numeric types.

```javascript { .api }
/**
 * Check if value is a JavaScript number
 * @param x - Value to check
 * @returns true if x is a number
 */
function isNumber(x: any): x is number

/**
 * Check if value is a BigNumber (arbitrary precision)
 * @param x - Value to check
 * @returns true if x is BigNumber
 */
function isBigNumber(x: any): x is BigNumber

/**
 * Check if value is a bigint (ES2020)
 * @param x - Value to check
 * @returns true if x is bigint
 */
function isBigInt(x: any): x is bigint

/**
 * Check if value is a Complex number
 * @param x - Value to check
 * @returns true if x is Complex
 */
function isComplex(x: any): x is Complex

/**
 * Check if value is a Fraction
 * @param x - Value to check
 * @returns true if x is Fraction
 */
function isFraction(x: any): x is Fraction

/**
 * Check if value is any numeric type
 * NOTE: Returns true for number, BigNumber, bigint, and Fraction
 * Returns FALSE for Complex numbers - use isComplex() to check for Complex
 * @param x - Value to check
 * @returns true if x is number, BigNumber, bigint, or Fraction (NOT Complex)
 */
function isNumeric(x: any): boolean

/**
 * Check if value has a numeric value (including Unit)
 * @param x - Value to check
 * @returns true if value can be converted to number
 */
function hasNumericValue(x: any): boolean
```

**Usage Examples:**

```javascript
import { isNumber, isBigNumber, isComplex, isFraction, isNumeric, hasNumericValue } from 'mathjs'
import { bignumber, complex, fraction, unit } from 'mathjs'

// JavaScript number
isNumber(5)                   // true
isNumber(3.14)                // true
isNumber('5')                 // false

// BigNumber
isBigNumber(bignumber(5))     // true
isBigNumber(5)                // false

// Complex
isComplex(complex(2, 3))      // true
isComplex(5)                  // false

// Fraction
isFraction(fraction(1, 3))    // true
isFraction(0.333)             // false

// Any numeric type (excludes Complex - use isComplex() for that)
isNumeric(5)                  // true
isNumeric(bignumber(5))       // true
isNumeric(fraction(1, 2))     // true
isNumeric(complex(2, 3))      // false - Complex is NOT considered numeric by isNumeric()
isNumeric('hello')            // false

// Has numeric value
hasNumericValue(5)            // true
hasNumericValue(unit('5 cm')) // true
hasNumericValue('hello')      // false
```

### Collection Type Checking

Check for arrays, matrices, and collections.

```javascript { .api }
/**
 * Check if value is an Array
 * @param x - Value to check
 * @returns true if x is Array
 */
function isArray(x: any): x is Array

/**
 * Check if value is a Matrix
 * @param x - Value to check
 * @returns true if x is Matrix
 */
function isMatrix(x: any): x is Matrix

/**
 * Check if value is a dense Matrix
 * @param x - Value to check
 * @returns true if x is dense Matrix
 */
function isDenseMatrix(x: any): boolean

/**
 * Check if value is a sparse Matrix
 * @param x - Value to check
 * @returns true if x is sparse Matrix
 */
function isSparseMatrix(x: any): boolean

/**
 * Check if value is a collection (Array or Matrix)
 * @param x - Value to check
 * @returns true if x is Array or Matrix
 */
function isCollection(x: any): boolean
```

**Usage Examples:**

```javascript
import { isArray, isMatrix, isDenseMatrix, isSparseMatrix, isCollection } from 'mathjs'
import { matrix, sparse } from 'mathjs'

// Arrays
isArray([1, 2, 3])            // true
isArray(matrix([1, 2, 3]))    // false

// Matrices
isMatrix(matrix([1, 2, 3]))   // true
isMatrix([1, 2, 3])           // false

// Dense vs sparse
isDenseMatrix(matrix([1, 2])) // true
isDenseMatrix(sparse([1, 2])) // false
isSparseMatrix(sparse([1, 2]))// true

// Collections (array or matrix)
isCollection([1, 2, 3])       // true
isCollection(matrix([1, 2]))  // true
isCollection(5)               // false
```

### Special Type Checking

Check for units, ranges, indices, and other special types.

```javascript { .api }
/**
 * Check if value is a Unit
 * @param x - Value to check
 * @returns true if x is Unit
 */
function isUnit(x: any): x is Unit

/**
 * Check if value is a Range
 * NOTE: This function is NOT available as a public export in mathjs 15.1.0
 * Use range() to create ranges, but isRange() cannot be imported for checking
 * @param x - Value to check
 * @returns true if x is Range
 * @deprecated Not available for import - internal function only
 */
// function isRange(x: any): boolean  // NOT AVAILABLE

/**
 * Check if value is an Index
 * @param x - Value to check
 * @returns true if x is Index
 */
function isIndex(x: any): boolean

/**
 * Check if value is a Chain
 * @param x - Value to check
 * @returns true if x is MathJsChain
 */
function isChain(x: any): boolean

/**
 * Check if value is a Help object
 * @param x - Value to check
 * @returns true if x is Help
 */
function isHelp(x: any): boolean

/**
 * Check if value is a ResultSet
 * @param x - Value to check
 * @returns true if x is ResultSet
 */
function isResultSet(x: any): boolean
```

**Usage Examples:**

```javascript
import { isUnit, isIndex, isChain, isHelp } from 'mathjs'
import { unit, range, index, chain, help } from 'mathjs'

// Unit
isUnit(unit('5 cm'))          // true
isUnit(5)                     // false

// Range - NOTE: isRange() is not available for import
// Use typeOf() or duck typing to check for Range objects
const r = range(1, 10)
// isRange(r) would fail - function not exported
// Instead use: typeOf(r) === 'Range'

// Index
isIndex(index(0, 1, 2))       // true
isIndex([0, 1, 2])            // false

// Chain
isChain(chain(5))             // true
isChain(5)                    // false

// Help
isHelp(help(sqrt))            // true
```

### AST Node Type Checking

Check for expression parser node types.

```javascript { .api }
/**
 * Check if value is any MathNode
 * @param x - Value to check
 * @returns true if x is MathNode
 */
function isNode(x: any): boolean

/**
 * Check if value is an AccessorNode
 */
function isAccessorNode(x: any): boolean

/**
 * Check if value is an ArrayNode
 */
function isArrayNode(x: any): boolean

/**
 * Check if value is an AssignmentNode
 */
function isAssignmentNode(x: any): boolean

/**
 * Check if value is a BlockNode
 */
function isBlockNode(x: any): boolean

/**
 * Check if value is a ConditionalNode
 */
function isConditionalNode(x: any): boolean

/**
 * Check if value is a ConstantNode
 */
function isConstantNode(x: any): boolean

/**
 * Check if value is a FunctionAssignmentNode
 */
function isFunctionAssignmentNode(x: any): boolean

/**
 * Check if value is a FunctionNode
 */
function isFunctionNode(x: any): boolean

/**
 * Check if value is an IndexNode
 */
function isIndexNode(x: any): boolean

/**
 * Check if value is an ObjectNode
 */
function isObjectNode(x: any): boolean

/**
 * Check if value is an OperatorNode
 */
function isOperatorNode(x: any): boolean

/**
 * Check if value is a ParenthesisNode
 */
function isParenthesisNode(x: any): boolean

/**
 * Check if value is a RangeNode
 */
function isRangeNode(x: any): boolean

/**
 * Check if value is a RelationalNode
 */
function isRelationalNode(x: any): boolean

/**
 * Check if value is a SymbolNode
 */
function isSymbolNode(x: any): boolean
```

**Usage Examples:**

```javascript
import { parse, isNode, isOperatorNode, isSymbolNode, isConstantNode } from 'mathjs'

const expr = parse('x + 5')

// Check if node
isNode(expr)                  // true
isOperatorNode(expr)          // true (root is + operator)

// Traverse and check node types
expr.traverse((node, path, parent) => {
  if (isSymbolNode(node)) {
    console.log('Variable:', node.name)  // 'x'
  }
  if (isConstantNode(node)) {
    console.log('Constant:', node.value) // 5
  }
})
```

### Value Property Checking

Check properties of values.

```javascript { .api }
/**
 * Check if value is an integer
 * @param x - Value to check
 * @returns true if x is an integer
 */
function isInteger(x: any): boolean

/**
 * Check if value is positive
 * @param x - Value to check
 * @returns true if x > 0
 */
function isPositive(x: any): boolean

/**
 * Check if value is negative
 * @param x - Value to check
 * @returns true if x < 0
 */
function isNegative(x: any): boolean

/**
 * Check if value is zero
 * @param x - Value to check
 * @returns true if x equals 0
 */
function isZero(x: any): boolean

/**
 * Check if value is NaN
 * @param x - Value to check
 * @returns true if x is NaN
 */
function isNaN(x: any): boolean

/**
 * Check if value is finite
 * @param x - Value to check
 * @returns true if x is finite
 */
function isFinite(x: any): boolean

/**
 * Check if value is prime
 * @param x - Value to check (must be integer)
 * @returns true if x is prime
 */
function isPrime(x: number | BigNumber): boolean

/**
 * Check if value is bounded (finite and not NaN)
 * @param x - Value to check
 * @returns true if x is bounded
 */
function isBounded(x: any): boolean
```

**Usage Examples:**

```javascript
import { isInteger, isPositive, isNegative, isZero, isNaN, isPrime } from 'mathjs'

// Integer check
isInteger(5)                  // true
isInteger(5.5)                // false
isInteger(bignumber(5))       // true

// Sign checks
isPositive(5)                 // true
isPositive(-5)                // false
isNegative(-5)                // true
isZero(0)                     // true

// Special values
isNaN(NaN)                    // true
isNaN(5)                      // false
isFinite(5)                   // true
isFinite(Infinity)            // false

// Prime check
isPrime(7)                    // true
isPrime(8)                    // false
isPrime(2)                    // true
isPrime(1)                    // false
```

### JavaScript Type Checking

Check for standard JavaScript types.

```javascript { .api }
/**
 * Check if value is a boolean
 */
function isBoolean(x: any): x is boolean

/**
 * Check if value is a string
 */
function isString(x: any): x is string

/**
 * Check if value is a function
 */
function isFunction(x: any): x is Function

/**
 * Check if value is a Date
 */
function isDate(x: any): x is Date

/**
 * Check if value is a RegExp
 */
function isRegExp(x: any): x is RegExp

/**
 * Check if value is a Map
 */
function isMap(x: any): x is Map

/**
 * Check if value is null
 */
function isNull(x: any): x is null

/**
 * Check if value is undefined
 */
function isUndefined(x: any): x is undefined
```

**Usage Examples:**

```javascript
import { isBoolean, isString, isFunction, isDate, isNull, isUndefined } from 'mathjs'

isBoolean(true)               // true
isString('hello')             // true
isFunction(Math.sqrt)         // true
isDate(new Date())            // true
isNull(null)                  // true
isUndefined(undefined)        // true
```

### Type Identification

Get the type name of any value.

```javascript { .api }
/**
 * Get the type name of a value
 * @param x - Value to identify
 * @returns Type name as string
 */
function typeOf(x: any): string
```

**Usage Examples:**

```javascript
import { typeOf, bignumber, complex, fraction, matrix, unit } from 'mathjs'

typeOf(5)                     // 'number'
typeOf(bignumber(5))          // 'BigNumber'
typeOf(complex(2, 3))         // 'Complex'
typeOf(fraction(1, 3))        // 'Fraction'
typeOf([1, 2, 3])             // 'Array'
typeOf(matrix([1, 2]))        // 'DenseMatrix'
typeOf(unit('5 cm'))          // 'Unit'
typeOf('hello')               // 'string'
typeOf(null)                  // 'null'
typeOf(undefined)             // 'undefined'
```

## Usage Patterns

### Type Guards in Functions

Use type checking for conditional logic:

```javascript
import { isNumber, isComplex, isUnit, re, im } from 'mathjs'

function describe(x) {
  if (isNumber(x)) {
    return `Number: ${x}`
  } else if (isComplex(x)) {
    return `Complex: ${re(x)} + ${im(x)}i`
  } else if (isUnit(x)) {
    return `Unit: ${x.toString()}`
  } else {
    return `Other: ${typeof x}`
  }
}

describe(5)                   // "Number: 5"
describe(complex(2, 3))       // "Complex: 2 + 3i"
describe(unit('5 cm'))        // "Unit: 5 cm"
```

### Polymorphic Functions

Handle different input types appropriately:

```javascript
import { isArray, isMatrix, typeOf, map, matrix } from 'mathjs'

function processData(data) {
  if (isArray(data)) {
    return map(data, x => x * 2)
  } else if (isMatrix(data)) {
    return map(data, x => x * 2)
  } else {
    return data * 2
  }
}

processData(5)                // 10
processData([1, 2, 3])        // [2, 4, 6]
processData(matrix([1, 2]))   // Matrix [2, 4]
```

### Validation

Validate function inputs:

```javascript
import { isNumber, isPositive, isInteger } from 'mathjs'

function factorial(n) {
  if (!isNumber(n)) {
    throw new TypeError('Input must be a number')
  }
  if (!isInteger(n)) {
    throw new TypeError('Input must be an integer')
  }
  if (!isPositive(n) && n !== 0) {
    throw new RangeError('Input must be non-negative')
  }

  // Calculate factorial...
}
```

### AST Node Processing

Process parsed expressions based on node types:

```javascript
import { parse, isOperatorNode, isSymbolNode, isConstantNode } from 'mathjs'

function analyzeExpression(expr) {
  const node = parse(expr)
  const info = { variables: [], constants: [], operators: [] }

  node.traverse((n) => {
    if (isSymbolNode(n)) {
      info.variables.push(n.name)
    } else if (isConstantNode(n)) {
      info.constants.push(n.value)
    } else if (isOperatorNode(n)) {
      info.operators.push(n.op)
    }
  })

  return info
}

analyzeExpression('x + 5 * y')
// { variables: ['x', 'y'], constants: [5], operators: ['*', '+'] }
```

## Type Hierarchy

Understanding Math.js type relationships:

```
MathType
├── MathNumericType
│   ├── number
│   ├── BigNumber
│   ├── bigint
│   ├── Fraction
│   └── Complex
├── Unit
└── MathCollection
    ├── Array
    └── Matrix
        ├── DenseMatrix
        └── SparseMatrix
```

Use type checking to navigate this hierarchy:

```javascript
import { isNumeric, isUnit, isCollection, typeOf } from 'mathjs'

function categorize(x) {
  if (isNumeric(x)) return 'numeric'
  if (isUnit(x)) return 'unit'
  if (isCollection(x)) return 'collection'
  return 'other'
}
```

## Performance Considerations

- Type checks are fast (simple property/prototype checks)
- Use specific type checks (`isNumber`) over generic checks (`typeOf`) when possible
- Type checking adds minimal overhead to function calls
- In performance-critical code, consider assuming types if validated elsewhere
- `isNumeric` checks multiple types; use specific checks if you know the expected type
