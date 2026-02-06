# Expression Parsing and Evaluation

Parse, compile, evaluate, and manipulate mathematical expressions from strings. Includes symbolic computation capabilities like simplification, rationalization, and differentiation.

## Capabilities

### Expression Evaluation

Directly evaluate mathematical expressions from strings.

```javascript { .api }
/**
 * Evaluate one or more expressions
 * @param expr - Expression string or array of expressions
 * @param scope - Object with variable values
 * @returns Evaluation result
 */
function evaluate(expr: string, scope?: object): any
function evaluate(expr: string[], scope?: object): any[]

/**
 * Compile an expression to a reusable function
 * @param expr - Expression string or array
 * @returns Compiled function that accepts a scope
 */
function compile(expr: string): EvalFunction
function compile(expr: string[]): EvalFunction[]

/**
 * Parse an expression to an Abstract Syntax Tree (AST)
 * @param expr - Expression string or array
 * @param options - Parser options
 * @returns MathNode representing the expression
 */
function parse(expr: string, options?: ParseOptions): MathNode
function parse(expr: string[], options?: ParseOptions): MathNode[]
```

**Usage Examples:**

```javascript
import { evaluate, compile, parse } from 'mathjs'

// Direct evaluation
evaluate('2 + 3')                    // 5
evaluate('sin(45 deg)')              // 0.707...
evaluate('det([-1, 2; 3, 1])')       // -7

// With variables
evaluate('x^2 + x', { x: 3 })        // 12
evaluate('a * b', { a: 2, b: 4 })    // 8

// Multiple expressions
evaluate(['a = 3', 'b = 4', 'a * b']) // [3, 4, 12]

// Compile for reuse
const f = compile('x^2 + x')
f.evaluate({ x: 3 })                 // 12
f.evaluate({ x: 5 })                 // 30

// Parse to AST
const node = parse('x^2 + x')
node.evaluate({ x: 3 })              // 12
node.toString()                      // 'x ^ 2 + x'
```

### Stateful Parser

Create a parser instance with persistent variable scope.

```javascript { .api }
/**
 * Create a parser instance with state
 * @returns Parser instance
 */
function parser(): Parser

interface Parser {
  /**
   * Evaluate expression with parser's scope
   * @param expr - Expression string
   * @returns Result
   */
  evaluate(expr: string): any

  /**
   * Get variable from parser scope
   * @param name - Variable name
   * @returns Variable value
   */
  get(name: string): any

  /**
   * Get all variables in scope
   * @returns Object with all variables
   */
  getAll(): object

  /**
   * Set variable in parser scope
   * @param name - Variable name
   * @param value - Value to set
   */
  set(name: string, value: any): void

  /**
   * Remove variable from scope
   * @param name - Variable name
   */
  remove(name: string): void

  /**
   * Clear all variables from scope
   */
  clear(): void
}
```

**Usage Examples:**

```javascript
import { parser } from 'mathjs'

const p = parser()

// Variables persist across evaluations
p.evaluate('x = 7')
p.evaluate('y = x + 2')
p.evaluate('z = x * y')    // 63

// Access variables
p.get('x')                 // 7
p.getAll()                 // { x: 7, y: 9, z: 63 }

// Set variables programmatically
p.set('a', 10)
p.evaluate('a + 5')        // 15

// Remove and clear
p.remove('z')
p.clear()
```

### Symbolic Computation

Simplify and manipulate expressions symbolically.

```javascript { .api }
/**
 * Simplify an expression
 * @param expr - Expression string or MathNode
 * @param rules - Optional simplification rules
 * @param scope - Variable scope
 * @param options - Simplification options
 * @returns Simplified MathNode
 */
function simplify(
  expr: string | MathNode,
  rules?: object[],
  scope?: object,
  options?: SimplifyOptions
): MathNode

/**
 * Simplify constant expressions
 * @param expr - Expression string or MathNode
 * @param options - Simplification options
 * @returns Simplified MathNode
 */
function simplifyConstant(expr: string | MathNode, options?: SimplifyOptions): MathNode

/**
 * Core simplification (minimal simplification)
 * @param expr - Expression string or MathNode
 * @param options - Simplification options
 * @returns Simplified MathNode
 */
function simplifyCore(expr: string | MathNode, options?: SimplifyOptions): MathNode

/**
 * Rationalize an expression
 * @param expr - Expression string or MathNode
 * @param optional - Optional parameter
 * @param detailed - Return detailed result
 * @returns Rationalized expression
 */
function rationalize(
  expr: string | MathNode,
  optional?: boolean,
  detailed?: boolean
): MathNode

/**
 * Test if two expressions are symbolically equal
 * @param expr1 - First expression
 * @param expr2 - Second expression
 * @param options - Comparison options
 * @returns True if symbolically equal
 */
function symbolicEqual(
  expr1: string | MathNode,
  expr2: string | MathNode,
  options?: object
): boolean

/**
 * Substitute variables in an expression
 * @param node - Expression node
 * @param scope - Variables to substitute
 * @returns Expression with substitutions
 */
function resolve(node: MathNode, scope?: object): MathNode
```

**Usage Examples:**

```javascript
import { simplify, rationalize, symbolicEqual, parse } from 'mathjs'

// Simplify expressions
simplify('2 * x + 3 * x').toString()           // '5 * x'
simplify('x^2 + 2*x + 1').toString()           // '(x + 1) ^ 2'
simplify('sin(x)^2 + cos(x)^2').toString()     // '1'

// Rationalize (remove radicals from denominator)
rationalize('1 / (sqrt(2) + 2)').toString()    // '(sqrt(2) - 2) / -2'

// Test symbolic equality
symbolicEqual('x + x', '2 * x')                // true
symbolicEqual('sin(x)^2 + cos(x)^2', '1')      // true

// Custom simplification rules
const rules = [
  { l: 'n1 * x + n2 * x', r: '(n1 + n2) * x' }
]
simplify('3*x + 2*x', rules).toString()        // '5 * x'
```

### Symbolic Differentiation

Calculate derivatives of expressions symbolically.

```javascript { .api }
/**
 * Calculate the derivative of an expression
 * @param expr - Expression to differentiate
 * @param variable - Variable to differentiate with respect to
 * @param options - Differentiation options
 * @returns Derivative as MathNode
 */
function derivative(
  expr: string | MathNode,
  variable: string | MathNode,
  options?: object
): MathNode
```

**Usage Examples:**

```javascript
import { derivative, simplify } from 'mathjs'

// Basic differentiation
derivative('x^2', 'x').toString()              // '2 * x'
derivative('sin(x)', 'x').toString()           // 'cos(x)'
derivative('x^2 + 2*x + 1', 'x').toString()    // '2 * x + 2'

// Simplify the result
simplify(derivative('x^3 + x^2', 'x')).toString()  // '3 * x ^ 2 + 2 * x'

// Multi-variable
derivative('x*y', 'x').toString()              // 'y'
derivative('x*y', 'y').toString()              // 'x'

// Chain rule
derivative('sin(x^2)', 'x').toString()         // 'cos(x ^ 2) * 2 * x'

// Evaluate at a point
const df = derivative('x^2 + x', 'x')
df.evaluate({ x: 3 })                          // 7
```

### Expression Analysis

Analyze expression structure and properties.

```javascript { .api }
/**
 * Count the number of leaf nodes in expression tree
 * @param expr - Expression string or MathNode
 * @returns Number of leaf nodes
 */
function leafCount(expr: string | MathNode): number

/**
 * Find roots of a polynomial (degree ≤ 3)
 * @param coefficients - Polynomial coefficients
 * @returns Array of roots
 */
function polynomialRoot(...coefficients: number[]): number[] | Complex[]
```

**Usage Examples:**

```javascript
import { leafCount, polynomialRoot, parse } from 'mathjs'

// Count leaves in expression tree
leafCount('x + y')                    // 2
leafCount('x + y + z')                // 3
leafCount('(x + y) * (a + b)')        // 4

// Find polynomial roots
polynomialRoot(1, -3, 2)              // [2, 1] (roots of x^2 - 3x + 2)
polynomialRoot(1, 0, 0, -1)           // [1, -0.5+0.866i, -0.5-0.866i]
```

### AST Node Interface

All parsed expressions return MathNode objects with these methods.

```javascript { .api }
interface MathNode {
  /**
   * Compile node to executable function
   * @returns Compiled function
   */
  compile(): EvalFunction

  /**
   * Evaluate the node
   * @param scope - Variable values
   * @returns Evaluation result
   */
  evaluate(scope?: object): any

  /**
   * Convert to string representation
   * @param options - Formatting options
   * @returns String representation
   */
  toString(options?: object): string

  /**
   * Convert to LaTeX representation
   * @param options - Formatting options
   * @returns LaTeX string
   */
  toTex(options?: object): string

  /**
   * Convert to JSON
   * @returns JSON representation
   */
  toJSON(): object

  /**
   * Transform the tree with a callback
   * @param callback - Transformation function
   * @returns Transformed node
   */
  transform(callback: (node: MathNode, path: string, parent: MathNode) => MathNode): MathNode

  /**
   * Traverse the tree
   * @param callback - Function to call for each node
   */
  traverse(callback: (node: MathNode, path: string, parent: MathNode) => void): void

  /**
   * Clone the node
   * @returns Cloned node
   */
  clone(): MathNode

  /**
   * Deep clone the node
   * @returns Cloned node
   */
  cloneDeep(): MathNode

  /**
   * Check equality with another node
   * @param other - Node to compare
   * @returns True if equal
   */
  equals(other: MathNode): boolean

  /**
   * Map over child nodes
   * @param callback - Function to apply to children
   * @returns New node with mapped children
   */
  map(callback: (node: MathNode) => MathNode): MathNode

  /**
   * Filter child nodes
   * @param callback - Filter predicate
   * @returns Filtered children
   */
  filter(callback: (node: MathNode) => boolean): MathNode[]
}

interface EvalFunction {
  /**
   * Evaluate with scope
   * @param scope - Variable values
   * @returns Result
   */
  (scope?: object): any

  /**
   * Evaluate with scope (method)
   * @param scope - Variable values
   * @returns Result
   */
  evaluate(scope?: object): any
}

interface ParseOptions {
  /**
   * Set of custom node constructors
   */
  nodes?: Record<string, MathNode>
}

interface SimplifyOptions {
  /**
   * Return exact fractions when possible (default: true)
   */
  exactFractions?: boolean

  /**
   * Maximum numerator/denominator for exact fractions (default: 10000)
   */
  fractionsLimit?: number

  /**
   * Enable console debug output (default: false)
   */
  consoleDebug?: boolean
}
```

**Usage Examples:**

```javascript
import { parse } from 'mathjs'

const node = parse('x^2 + 2*x + 1')

// Evaluation
node.evaluate({ x: 3 })              // 16

// String representations
node.toString()                       // 'x ^ 2 + 2 * x + 1'
node.toTex()                          // 'x^{2}+2 x+1'

// Compilation
const fn = node.compile()
fn({ x: 3 })                          // 16
fn({ x: 5 })                          // 36

// Transformation
const doubled = node.map(n => {
  if (n.isConstantNode) {
    return parse(String(n.value * 2))
  }
  return n
})
doubled.toString()                    // 'x ^ 4 + 4 * x + 2'

// Traversal
node.traverse((n, path, parent) => {
  console.log(n.type, path)
})
```

### Parser Utilities

Helper functions for character classification in parsing.

```javascript { .api }
/**
 * Check if character is alphabetic
 */
parse.isAlpha(c: string, cPrev: string, cNext: string): boolean

/**
 * Check if valid Latin or Greek character
 */
parse.isValidLatinOrGreek(c: string): boolean

/**
 * Check if valid math symbol (surrogate pair)
 */
parse.isValidMathSymbol(high: string, low: string): boolean

/**
 * Check if whitespace character
 */
parse.isWhitespace(c: string, nestingLevel: number): boolean

/**
 * Check if decimal mark
 */
parse.isDecimalMark(c: string, cNext: string): boolean

/**
 * Check if digit or dot
 */
parse.isDigitDot(c: string): boolean

/**
 * Check if digit
 */
parse.isDigit(c: string): boolean

/**
 * Check if hex digit
 */
parse.isHexDigit(c: string): boolean
```

## Expression Syntax

Math.js expressions support rich syntax:

**Operators:**
- Arithmetic: `+`, `-`, `*`, `/`, `^`, `mod`, `!`
- Element-wise: `.*`, `./`, `.^`
- Comparison: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Logical: `and`, `or`, `xor`, `not`
- Bitwise: `&`, `|`, `^|`, `~`, `<<`, `>>`, `>>>`

**Functions:**
```javascript
evaluate('sin(pi/4)')
evaluate('sqrt(16)')
evaluate('log(100, 10)')
```

**Variables:**
```javascript
evaluate('x + y', { x: 2, y: 3 })
```

**Units:**
```javascript
evaluate('5 cm to inch')
evaluate('9.81 m/s^2')
```

**Matrices:**
```javascript
evaluate('[1, 2; 3, 4]')          // 2×2 matrix
evaluate('det([1, 2; 3, 4])')     // -2
```

**Assignments:**
```javascript
evaluate(['x = 5', 'y = x + 2', 'x * y'])  // [5, 7, 35]
```

**Conditionals:**
```javascript
evaluate('x > 0 ? 1 : -1', { x: 5 })  // 1
```
