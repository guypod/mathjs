# Expression Parser and Symbolic Algebra

Math.js provides a powerful expression parser and symbolic algebra system for parsing mathematical expressions into Abstract Syntax Trees (ASTs), manipulating them symbolically, and evaluating them. The system includes expression parsing, compilation, symbolic differentiation, simplification, and rationalization capabilities.

## Expression Functions

### evaluate

Evaluate a mathematical expression string or array of expressions.

```typescript { .api }
function evaluate(expr: string | string[], scope?: Map | Object): any;
function evaluate(expr: Matrix, scope?: Map | Object): any;
```

**Parameters:**
- `expr` - Expression string, array of expressions, or matrix to evaluate
- `scope` - Optional scope containing variables and functions (Map or Object)

**Returns:** Result of the expression evaluation

**Usage:**

```javascript
import { evaluate } from 'mathjs';

// Simple evaluation
evaluate('sqrt(3^2 + 4^2)');  // 5
evaluate('sqrt(-4)');          // 2i
evaluate('2 inch to cm');      // 5.08 cm

// With scope
const scope = { a: 3, b: 4 };
evaluate('a * b', scope);      // 12

// Assignments modify scope
evaluate('c = 2.3 + 4.5', scope);  // 6.8
console.log(scope.c);              // 6.8

// Multiple expressions
evaluate(['a = 3', 'b = 4', 'a * b']);  // [3, 4, 12]
```

### parse

Parse an expression string into an Abstract Syntax Tree (AST) node.

```typescript { .api }
function parse(expr: string, options?: ParseOptions): MathNode;
function parse(exprs: string[], options?: ParseOptions): MathNode[];

interface ParseOptions {
  nodes?: Record<string, MathNode>;
}
```

**Parameters:**
- `expr` - Expression string or array of expression strings to parse
- `options` - Optional parsing options with custom node definitions

**Returns:** Root MathNode of the expression tree, or array of nodes

**Usage:**

```javascript
import { parse } from 'mathjs';

// Parse an expression
const node = parse('sqrt(3^2 + 4^2)');
console.log(node.toString());  // 'sqrt(3 ^ 2 + 4 ^ 2)'

// Compile and evaluate
const code = node.compile();
code.evaluate();  // 5

// With variables
const node2 = parse('x^2 + 3*x + 1');
const code2 = node2.compile();
code2.evaluate({ x: 2 });  // 11

// Parse multiple expressions
const nodes = parse(['a = 3', 'b = 4', 'a + b']);
```

### compile

Compile an expression into optimized JavaScript code for repeated evaluation.

```typescript { .api }
function compile(expr: string): EvalFunction;
function compile(exprs: string[]): EvalFunction[];

interface EvalFunction {
  evaluate(scope?: Map | Object): any;
}
```

**Parameters:**
- `expr` - Expression string or array of expressions to compile
- `exprs` - Array of expression strings

**Returns:** Compiled function with `evaluate(scope)` method

**Usage:**

```javascript
import { compile } from 'mathjs';

// Compile once, evaluate many times
const code = compile('a * x^2 + b * x + c');

code.evaluate({ a: 1, b: 2, c: 3, x: 5 });  // 38
code.evaluate({ a: 2, b: -1, c: 0, x: 3 }); // 15

// More efficient than using evaluate() repeatedly
for (let x = 0; x < 100; x++) {
  const result = code.evaluate({ a: 1, b: 2, c: 1, x });
}
```

### help

Get help documentation for a math.js function or constant.

```typescript { .api }
function help(topic: Function | string): Help;

interface Help {
  toString(): string;
  toJSON(): Object;
}
```

**Parameters:**
- `topic` - Function or string name to get help for

**Returns:** Help object with documentation

**Usage:**

```javascript
import { help, sqrt } from 'mathjs';

// Get help for a function
console.log(help(sqrt).toString());
// Displays documentation for sqrt function

// Get help by name
console.log(help('sin').toString());
```

### Parser

Create a parser instance that maintains its own scope for variables and functions.

```typescript { .api }
function parser(): Parser;

interface Parser {
  evaluate(expr: string | string[]): any;
  get(name: string): any;
  getAll(): Object;
  getAllAsMap(): Map<string, any>;
  set(name: string, value: any): void;
  remove(name: string): void;
  clear(): void;
}
```

**Returns:** Parser instance with persistent scope

**Usage:**

```javascript
import { parser } from 'mathjs';

const p = parser();

// Variables persist between evaluations
p.evaluate('x = 7');
p.evaluate('x + 3');       // 10

// Define functions
p.evaluate('f(x, y) = x^y');
p.evaluate('f(2, 3)');     // 8

// Get and set variables
const x = p.get('x');      // 7
p.set('y', 10);
p.evaluate('x + y');       // 17

// Custom JavaScript functions
p.set('greet', name => `Hello, ${name}!`);
p.evaluate('greet("Alice")');  // "Hello, Alice!"

// Clear all variables
p.clear();
```

### ParserClass

The Parser class constructor (accessed via `parser()` function).

```typescript { .api }
class ParserClass {
  constructor();
  evaluate(expr: string | string[]): any;
  get(name: string): any;
  getAll(): Object;
  getAllAsMap(): Map<string, any>;
  set(name: string, value: any): void;
  remove(name: string): void;
  clear(): void;
}
```

## Algebra Functions

### derivative

Calculate the symbolic derivative of an expression with respect to a variable.

```typescript { .api }
function derivative(
  expr: MathNode | string,
  variable: MathNode | string,
  options?: { simplify: boolean }
): MathNode;
```

**Parameters:**
- `expr` - Expression to differentiate (string or MathNode)
- `variable` - Variable to differentiate with respect to (string or MathNode)
- `options` - Optional settings; `simplify: true` to simplify result (default: true)

**Returns:** MathNode representing the derivative

**Usage:**

```javascript
import { derivative, parse } from 'mathjs';

// Differentiate expressions
derivative('x^2', 'x').toString();           // '2 * x'
derivative('sin(x)', 'x').toString();        // 'cos(x)'
derivative('2*x^3 + 3*x^2 - 4*x + 5', 'x').toString();  // '6 * x^2 + 6 * x - 4'

// With expression nodes
const expr = parse('x^2 + 2*x');
const x = parse('x');
const dexpr = derivative(expr, x);
console.log(dexpr.toString());    // '2 * x + 2'
console.log(dexpr.evaluate({ x: 3 }));  // 8

// Trigonometric functions
derivative('sin(2*x)', 'x').toString();  // '2 * cos(2 * x)'
derivative('tan(x)', 'x').toString();    // 'sec(x)^2'

// Disable simplification
derivative('x + x', 'x', { simplify: false }).toString();  // '1 + 1'
```

### leafCount

Count the number of leaf nodes in an expression tree. Leaf nodes are symbols and constants with no subexpressions.

```typescript { .api }
function leafCount(expr: MathNode): number;
```

**Parameters:**
- `expr` - Expression node to count leaves in

**Returns:** Number of leaf nodes

**Usage:**

```javascript
import { leafCount, parse } from 'mathjs';

leafCount(parse('x'));           // 1
leafCount(parse('x + y'));       // 2
leafCount(parse('x^2 + y^2'));   // 4
leafCount(parse('5!'));          // 1 (just the 5)
leafCount(parse('sin(x) + cos(y)'));  // 2
```

### rationalize

Transform an expression into a rational fraction form and optionally return it in canonical form.

```typescript { .api }
function rationalize(
  expr: MathNode | string,
  scope?: Object | boolean,
  detailed?: false
): MathNode;

function rationalize(
  expr: MathNode | string,
  scope?: Object | boolean,
  detailed: true
): {
  expression: MathNode | string;
  variables?: string[];
  coefficients?: number[];
};
```

**Parameters:**
- `expr` - Expression to rationalize
- `scope` - Variable values as object, or boolean for `detailed` parameter
- `detailed` - If true, return object with expression, variables, and coefficients

**Returns:** Rationalized MathNode, or detailed object if `detailed: true`

**Usage:**

```javascript
import { rationalize } from 'mathjs';

// Basic rationalization
rationalize('2*x/y - y/(x+1)').toString();
// '(2 * x^2 - y^2 + 2 * x) / (x * y + y)'

// Expand polynomials
rationalize('(2*x + 1)^3').toString();
// '8 * x^3 + 12 * x^2 + 6 * x + 1'

// Complex rational expressions
rationalize('2*x/((2*x-1)/(3*x+2)) - 5*x/((3*x+4)/(2*x^2-5)) + 3').toString();
// '(-20 * x^4 + 28 * x^3 + 104 * x^2 + 6 * x - 12) / (6 * x^2 + 5 * x - 4)'

// With variable substitution
rationalize('x + x + x + y', { y: 1 }).toString();  // '3 * x + 1'
rationalize('x + x + x + y', {}).toString();        // '3 * x + y'

// Detailed output with coefficients
const result = rationalize('-2 + 5*x^2', {}, true);
// result.expression: '5 * x^2 - 2'
// result.variables: ['x']
// result.coefficients: [-2, 0, 5]
```

### resolve

Replace variable nodes with their values from a scope.

```typescript { .api }
function resolve(node: MathNode | string, scope?: Map | Object): MathNode;
function resolve(node: (MathNode | string)[], scope?: Map | Object): MathNode[];
function resolve(node: Matrix, scope?: Map | Object): Matrix;
```

**Parameters:**
- `node` - Expression node, string, array, or matrix to resolve
- `scope` - Scope containing variable values

**Returns:** Node with variables replaced by their values

**Usage:**

```javascript
import { resolve, parse } from 'mathjs';

const node = parse('x + y');
const scope = { x: 3, y: 4 };

const resolved = resolve(node, scope);
resolved.toString();     // '3 + 4'
resolved.evaluate();     // 7

// Partial resolution
const partial = resolve('a*x + b*x', { a: 2, b: 3 });
partial.toString();  // '2 * x + 3 * x'

// Can then simplify
import { simplify } from 'mathjs';
simplify(partial).toString();  // '5 * x'
```

### simplify

Simplify an expression tree using algebraic rules.

```typescript { .api }
function simplify(expr: MathNode | string): MathNode;
function simplify(
  expr: MathNode | string,
  rules: SimplifyRule[],
  scope?: Map | Object,
  options?: SimplifyOptions
): MathNode;
function simplify(
  expr: MathNode | string,
  scope: Map | Object,
  options?: SimplifyOptions
): MathNode;

interface SimplifyOptions {
  exactFractions?: boolean;
  fractionsLimit?: number;
  consoleDebug?: boolean;
  context?: SimplifyContext;
}

type SimplifyRule = string | {
  l: string;
  r: string;
  repeat?: boolean;
  assuming?: SimplifyContext;
  imposeContext?: SimplifyContext;
} | ((node: MathNode) => MathNode);
```

**Parameters:**
- `expr` - Expression to simplify (string or MathNode)
- `rules` - Optional custom simplification rules
- `scope` - Optional variable values for substitution
- `options` - Simplification options (exactFractions, context, etc.)

**Returns:** Simplified MathNode

**Usage:**

```javascript
import { simplify } from 'mathjs';

// Basic simplification
simplify('3 + 2/4').toString();           // '7 / 2'
simplify('2*x + 3*x').toString();         // '5 * x'
simplify('x^2 + x + 3 + x^2').toString(); // '2 * x^2 + x + 3'
simplify('x * y * -x / (x^2)').toString(); // '-y'

// With variables in scope
const expr = parse('x + x + x');
simplify(expr, { x: 2 }).toString();  // '6'

// Convert functions to operators
simplify('multiply(x, 3)').toString();  // '3 * x'

// Custom simplification rules
const customRules = [
  { l: 'n * n', r: 'n^2' }
];
simplify('x * x', customRules).toString();  // 'x^2'

// Control commutativity
const expr2 = parse('x*y - y*x');
simplify(expr2).toString();  // '0' (commutative by default)

const nonCommContext = {
  context: { multiply: { commutative: false } }
};
simplify(expr2, {}, nonCommContext).toString();  // 'x * y - y * x'

// Use real number context
simplify('sqrt(x^2)', {}, { context: simplify.realContext }).toString();
// 'abs(x)' (not 'x', which would be wrong for negative x)
```

### simplifyConstant

Simplify constant subexpressions in an expression.

```typescript { .api }
function simplifyConstant(expr: MathNode | string, options?: SimplifyOptions): MathNode;
```

**Parameters:**
- `expr` - Expression to simplify constants in
- `options` - Simplification options

**Returns:** MathNode with constants simplified

**Usage:**

```javascript
import { simplifyConstant } from 'mathjs';

// Simplify only constant expressions
simplifyConstant('2 + 3 + x').toString();        // '5 + x'
simplifyConstant('2 * 3 * x').toString();        // '6 * x'
simplifyConstant('sqrt(4) + x').toString();      // '2 + x'
simplifyConstant('sin(0) + cos(0)').toString();  // '1'
```

### simplifyCore

Perform core simplification without applying full simplification rules.

```typescript { .api }
function simplifyCore(expr: MathNode | string, options?: SimplifyOptions): MathNode;
```

**Parameters:**
- `expr` - Expression to simplify
- `options` - Simplification options

**Returns:** Core-simplified MathNode

**Usage:**

```javascript
import { simplifyCore } from 'mathjs';

// Minimal simplification
simplifyCore('1*x').toString();       // 'x'
simplifyCore('x + 0').toString();     // 'x'
simplifyCore('x^1').toString();       // 'x'
```

### symbolicEqual

Test whether two expressions are symbolically equal.

```typescript { .api }
function symbolicEqual(
  expr1: MathNode | string,
  expr2: MathNode | string,
  options?: SimplifyOptions
): boolean;
```

**Parameters:**
- `expr1` - First expression
- `expr2` - Second expression
- `options` - Simplification options for comparison

**Returns:** `true` if expressions are symbolically equal, `false` otherwise

**Usage:**

```javascript
import { symbolicEqual } from 'mathjs';

symbolicEqual('x + x', '2*x');           // true
symbolicEqual('x^2 - 1', '(x-1)*(x+1)'); // true
symbolicEqual('sin(x)^2 + cos(x)^2', '1'); // true

symbolicEqual('x + 1', 'x + 2');         // false
symbolicEqual('x * y', 'y * x');         // true (commutative)

// With custom context
const nonComm = { context: { multiply: { commutative: false } } };
symbolicEqual('x*y', 'y*x', nonComm);    // false
```

## AST Node Constructors

All AST node constructors are available in the `math` namespace and can be used to programmatically build expression trees.

### Node

Base class for all AST nodes.

```typescript { .api }
class Node {
  comment: string;
  type: string;
  isNode: true;

  clone(): Node;
  cloneDeep(): Node;
  compile(): EvalFunction;
  evaluate(scope?: Map | Object): any;
  equals(other: Node): boolean;
  filter(callback: (node: Node, path: string, parent: Node) => boolean): Node[];
  forEach(callback: (node: Node, path: string, parent: Node) => void): void;
  map(callback: (node: Node, path: string, parent: Node) => Node): Node;
  toString(options?: Object): string;
  toTex(options?: Object): string;
  toHTML(options?: Object): string;
  toJSON(): Object;
  transform(callback: (node: Node, path: string, parent: Node) => Node): Node;
  traverse(callback: (node: Node, path: string, parent: Node) => void): void;
}
```

**Common Methods:**
- `clone()` - Shallow clone of the node
- `cloneDeep()` - Deep clone including all children
- `compile()` - Compile to executable code
- `evaluate(scope)` - Evaluate the node with optional scope
- `toString()` - Convert to string representation
- `toTex()` - Convert to LaTeX representation
- `transform(callback)` - Recursively transform the tree
- `traverse(callback)` - Recursively visit all nodes

### AccessorNode

Property or index access: `object.property` or `object[index]`.

```typescript { .api }
class AccessorNode extends Node {
  constructor(object: Node, index: IndexNode, optionalChaining?: boolean);

  object: Node;
  index: IndexNode;
  name: string;
  optionalChaining: boolean;
}
```

**Properties:**
- `object` - The object being accessed
- `index` - The index/property to access
- `name` - Property name (read-only)
- `optionalChaining` - Whether using optional chaining (`?.`)

**Usage:**

```javascript
import { AccessorNode, SymbolNode, IndexNode, ConstantNode } from 'mathjs';

// a[3]
const a = new SymbolNode('a');
const three = new ConstantNode(3);
const index = new IndexNode([three]);
const node = new AccessorNode(a, index);

// a.b (dot notation)
const b = new ConstantNode('b');
const indexB = new IndexNode([b], true);  // dotNotation = true
const dotAccess = new AccessorNode(a, indexB);

// Optional chaining: a?.b
const optional = new AccessorNode(a, indexB, true);
```

### ArrayNode

Array literal: `[1, 2, 3]`.

```typescript { .api }
class ArrayNode extends Node {
  constructor(items: Node[]);

  items: Node[];
}
```

**Properties:**
- `items` - Array of nodes representing array elements

**Usage:**

```javascript
import { ArrayNode, ConstantNode } from 'mathjs';

// [1, 2, 3]
const one = new ConstantNode(1);
const two = new ConstantNode(2);
const three = new ConstantNode(3);
const node = new ArrayNode([one, two, three]);

console.log(node.toString());  // '[1, 2, 3]'
console.log(node.evaluate());  // [1, 2, 3]
```

### AssignmentNode

Variable or indexed assignment: `a = 5` or `a[1] = 5`.

```typescript { .api }
class AssignmentNode extends Node {
  constructor(object: SymbolNode | AccessorNode, value: Node);
  constructor(object: SymbolNode | AccessorNode, index: IndexNode, value: Node);

  object: SymbolNode | AccessorNode;
  index: IndexNode | null;
  value: Node;
  name: string;
}
```

**Properties:**
- `object` - Variable or accessor being assigned to
- `index` - Optional index for indexed assignment
- `value` - Value being assigned
- `name` - Variable name (read-only)

**Usage:**

```javascript
import { AssignmentNode, SymbolNode, ConstantNode } from 'mathjs';

// x = 5
const x = new SymbolNode('x');
const five = new ConstantNode(5);
const node = new AssignmentNode(x, five);

const scope = {};
node.evaluate(scope);
console.log(scope.x);  // 5

// a[2] = 10
const a = new SymbolNode('a');
const two = new ConstantNode(2);
const ten = new ConstantNode(10);
const index = new IndexNode([two]);
const indexed = new AssignmentNode(a, index, ten);
```

### BlockNode

Block of statements: `a=1; b=2; c=3` or multi-line expressions.

```typescript { .api }
class BlockNode extends Node {
  constructor(blocks: Array<{node: Node, visible: boolean}>);

  blocks: Array<{node: Node, visible: boolean}>;
}
```

**Properties:**
- `blocks` - Array of objects with `node` and `visible` properties

**Usage:**

```javascript
import { BlockNode, AssignmentNode, SymbolNode, ConstantNode } from 'mathjs';

// a=1; b=2; c=3
const a = new SymbolNode('a');
const one = new ConstantNode(1);
const assign1 = new AssignmentNode(a, one);

const b = new SymbolNode('b');
const two = new ConstantNode(2);
const assign2 = new AssignmentNode(b, two);

const c = new SymbolNode('c');
const three = new ConstantNode(3);
const assign3 = new AssignmentNode(c, three);

const block = new BlockNode([
  { node: assign1, visible: false },  // ends with ;
  { node: assign2, visible: false },  // ends with ;
  { node: assign3, visible: true }    // no semicolon
]);

// Evaluates to ResultSet with visible results
const result = block.evaluate();
console.log(result.entries);  // [3]
```

### ConditionalNode

Ternary conditional operator: `condition ? trueExpr : falseExpr`.

```typescript { .api }
class ConditionalNode extends Node {
  constructor(condition: Node, trueExpr: Node, falseExpr: Node);

  condition: Node;
  trueExpr: Node;
  falseExpr: Node;
}
```

**Properties:**
- `condition` - Boolean condition expression
- `trueExpr` - Expression to evaluate if condition is true
- `falseExpr` - Expression to evaluate if condition is false

**Usage:**

```javascript
import { ConditionalNode, SymbolNode, OperatorNode, ConstantNode } from 'mathjs';

// x > 0 ? x : -x (absolute value)
const x = new SymbolNode('x');
const zero = new ConstantNode(0);
const condition = new OperatorNode('>', 'larger', [x, zero]);
const trueExpr = x;
const falseExpr = new OperatorNode('-', 'unaryMinus', [x]);
const node = new ConditionalNode(condition, trueExpr, falseExpr);

console.log(node.evaluate({ x: 5 }));   // 5
console.log(node.evaluate({ x: -3 }));  // 3
```

### ConstantNode

Literal constant value: numbers, strings, etc.

```typescript { .api }
class ConstantNode extends Node {
  constructor(value: any);

  value: any;
}
```

**Properties:**
- `value` - The constant value (number, string, boolean, null, etc.)

**Usage:**

```javascript
import { ConstantNode } from 'mathjs';

const num = new ConstantNode(42);
const str = new ConstantNode('hello');
const bool = new ConstantNode(true);

console.log(num.evaluate());   // 42
console.log(str.evaluate());   // 'hello'
console.log(bool.evaluate());  // true
```

### FunctionAssignmentNode

Function definition: `f(x, y) = x^2 + y^2`.

```typescript { .api }
class FunctionAssignmentNode extends Node {
  constructor(name: string, params: string[], expr: Node);

  name: string;
  params: string[];
  expr: Node;
}
```

**Properties:**
- `name` - Function name
- `params` - Array of parameter names
- `expr` - Function body expression

**Usage:**

```javascript
import { FunctionAssignmentNode, SymbolNode, OperatorNode, ConstantNode } from 'mathjs';

// f(x) = x^2
const x = new SymbolNode('x');
const two = new ConstantNode(2);
const expr = new OperatorNode('^', 'pow', [x, two]);
const node = new FunctionAssignmentNode('f', ['x'], expr);

const scope = {};
node.evaluate(scope);

// Now f is available in scope
console.log(scope.f(5));  // 25
console.log(scope.f(10)); // 100
```

### FunctionNode

Function call: `sin(x)`, `max(a, b, c)`.

```typescript { .api }
class FunctionNode extends Node {
  constructor(fn: Node | string, args: Node[]);

  fn: Node | string;
  args: Node[];

  static onUndefinedFunction(name: string): void;
}
```

**Properties:**
- `fn` - Function name (string) or node
- `args` - Array of argument nodes

**Static Methods:**
- `onUndefinedFunction(name)` - Called when undefined function is evaluated (can be overridden)

**Usage:**

```javascript
import { FunctionNode, SymbolNode, ConstantNode } from 'mathjs';

// sqrt(4)
const four = new ConstantNode(4);
const sqrt = new FunctionNode('sqrt', [four]);
console.log(sqrt.evaluate());  // 2

// max(x, y, z)
const x = new SymbolNode('x');
const y = new SymbolNode('y');
const z = new SymbolNode('z');
const max = new FunctionNode('max', [x, y, z]);
console.log(max.evaluate({ x: 5, y: 10, z: 3 }));  // 10

// Custom undefined function handler
FunctionNode.onUndefinedFunction = name => {
  console.log(`Function ${name} is not defined`);
  return 0;
};
```

### IndexNode

Index specification for matrix/array access: `[1, 2:5, :]`.

```typescript { .api }
class IndexNode extends Node {
  constructor(dimensions: Node[], dotNotation?: boolean);

  dimensions: Node[];
  dotNotation: boolean;
}
```

**Properties:**
- `dimensions` - Array of index expressions (constants, ranges, or symbols)
- `dotNotation` - Whether this was written with dot notation

**Usage:**

```javascript
import { IndexNode, ConstantNode, RangeNode } from 'mathjs';

// [3]
const three = new ConstantNode(3);
const index1 = new IndexNode([three]);

// [1:5, 2]
const one = new ConstantNode(1);
const five = new ConstantNode(5);
const two = new ConstantNode(2);
const range = new RangeNode(one, five);
const index2 = new IndexNode([range, two]);
```

### ObjectNode

Object literal: `{a: 1, b: 2}`.

```typescript { .api }
class ObjectNode extends Node {
  constructor(properties: Object<string, Node>);

  properties: Object<string, Node>;
}
```

**Properties:**
- `properties` - Object mapping property names to value nodes

**Usage:**

```javascript
import { ObjectNode, ConstantNode, SymbolNode } from 'mathjs';

// {a: 1, b: 2, c: x}
const one = new ConstantNode(1);
const two = new ConstantNode(2);
const x = new SymbolNode('x');
const node = new ObjectNode({
  a: one,
  b: two,
  c: x
});

console.log(node.evaluate({ x: 3 }));
// {a: 1, b: 2, c: 3}
```

### OperatorNode

Binary or unary operator: `+`, `-`, `*`, `/`, `^`, etc.

```typescript { .api }
class OperatorNode extends Node {
  constructor(op: string, fn: string, args: Node[], implicit?: boolean);

  op: string;
  fn: string;
  args: Node[];
  implicit: boolean;

  isUnary(): boolean;
  isBinary(): boolean;
}
```

**Properties:**
- `op` - Operator symbol ('+', '-', '*', '/', '^', etc.)
- `fn` - Function name ('add', 'subtract', 'multiply', 'divide', 'pow', etc.)
- `args` - Operand nodes (1 for unary, 2 for binary)
- `implicit` - Whether this is implicit multiplication (e.g., `2x`)

**Methods:**
- `isUnary()` - Returns true if unary operator
- `isBinary()` - Returns true if binary operator

**Usage:**

```javascript
import { OperatorNode, ConstantNode, SymbolNode } from 'mathjs';

// 2 + 3
const two = new ConstantNode(2);
const three = new ConstantNode(3);
const add = new OperatorNode('+', 'add', [two, three]);
console.log(add.evaluate());  // 5

// -x (unary minus)
const x = new SymbolNode('x');
const neg = new OperatorNode('-', 'unaryMinus', [x]);
console.log(neg.evaluate({ x: 5 }));  // -5

// x * y (implicit multiplication: xy)
const y = new SymbolNode('y');
const mult = new OperatorNode('*', 'multiply', [x, y], true);
console.log(mult.toString());  // 'x y' (implicit)
```

### ParenthesisNode

Parenthesized expression: `(x + 1)`.

```typescript { .api }
class ParenthesisNode extends Node {
  constructor(content: Node);

  content: Node;
}
```

**Properties:**
- `content` - The expression inside parentheses

**Usage:**

```javascript
import { ParenthesisNode, ConstantNode, OperatorNode } from 'mathjs';

// (2 + 3)
const two = new ConstantNode(2);
const three = new ConstantNode(3);
const add = new OperatorNode('+', 'add', [two, three]);
const node = new ParenthesisNode(add);

console.log(node.toString());  // '(2 + 3)'
console.log(node.evaluate());  // 5
```

### RangeNode

Range expression: `1:10` or `0:2:10` (start:end or start:step:end).

```typescript { .api }
class RangeNode extends Node {
  constructor(start: Node, end: Node, step?: Node);

  start: Node;
  end: Node;
  step: Node | null;
}
```

**Properties:**
- `start` - Start value of range
- `end` - End value of range (inclusive)
- `step` - Step size (default: 1)

**Usage:**

```javascript
import { RangeNode, ConstantNode } from 'mathjs';

// 1:5
const one = new ConstantNode(1);
const five = new ConstantNode(5);
const range1 = new RangeNode(one, five);
console.log(range1.evaluate());  // [1, 2, 3, 4, 5]

// 0:2:10 (start:step:end)
const zero = new ConstantNode(0);
const two = new ConstantNode(2);
const ten = new ConstantNode(10);
const range2 = new RangeNode(zero, ten, two);
console.log(range2.evaluate());  // [0, 2, 4, 6, 8, 10]
```

### RelationalNode

Chained comparison: `a < b < c` (equivalent to `a < b and b < c` with short-circuit evaluation).

```typescript { .api }
class RelationalNode extends Node {
  constructor(conditionals: string[], params: Node[]);

  conditionals: string[];
  params: Node[];
}
```

**Properties:**
- `conditionals` - Array of comparison operators ('smaller', 'larger', 'smallerEq', 'largerEq', 'equal', 'unequal')
- `params` - Array of operand nodes (length = conditionals.length + 1)

**Usage:**

```javascript
import { RelationalNode, ConstantNode, SymbolNode } from 'mathjs';

// 10 < x <= 50
const ten = new ConstantNode(10);
const x = new SymbolNode('x');
const fifty = new ConstantNode(50);
const node = new RelationalNode(
  ['smaller', 'smallerEq'],
  [ten, x, fifty]
);

console.log(node.evaluate({ x: 25 }));  // true
console.log(node.evaluate({ x: 5 }));   // false
console.log(node.evaluate({ x: 100 })); // false

// Chained equality: a == b == c
const a = new SymbolNode('a');
const b = new SymbolNode('b');
const c = new SymbolNode('c');
const equals = new RelationalNode(
  ['equal', 'equal'],
  [a, b, c]
);
console.log(equals.evaluate({ a: 5, b: 5, c: 5 }));  // true
```

### SymbolNode

Variable reference: `x`, `myVar`.

```typescript { .api }
class SymbolNode extends Node {
  constructor(name: string);

  name: string;

  static onUndefinedSymbol(name: string): void;
}
```

**Properties:**
- `name` - Variable name

**Static Methods:**
- `onUndefinedSymbol(name)` - Called when undefined symbol is evaluated (can be overridden)

**Usage:**

```javascript
import { SymbolNode } from 'mathjs';

const x = new SymbolNode('x');
const y = new SymbolNode('y');

console.log(x.evaluate({ x: 5 }));      // 5
console.log(y.evaluate({ y: 'hello' })); // 'hello'

// Custom undefined symbol handler
SymbolNode.onUndefinedSymbol = name => {
  console.log(`Symbol ${name} is not defined`);
  return 0;
};
```

## Common Usage Patterns

### Building Expression Trees Manually

```javascript
import {
  OperatorNode, FunctionNode, ConstantNode, SymbolNode
} from 'mathjs';

// Build: sqrt(x^2 + y^2)
const x = new SymbolNode('x');
const y = new SymbolNode('y');
const two = new ConstantNode(2);

const xSquared = new OperatorNode('^', 'pow', [x, two]);
const ySquared = new OperatorNode('^', 'pow', [y, two]);
const sum = new OperatorNode('+', 'add', [xSquared, ySquared]);
const sqrt = new FunctionNode('sqrt', [sum]);

console.log(sqrt.toString());           // 'sqrt(x ^ 2 + y ^ 2)'
console.log(sqrt.evaluate({ x: 3, y: 4 })); // 5
```

### Transforming Expression Trees

```javascript
import { parse, ConstantNode } from 'mathjs';

// Replace all occurrences of x with 5
const node = parse('x^2 + 2*x + 1');
const transformed = node.transform((node, path, parent) => {
  if (node.isSymbolNode && node.name === 'x') {
    return new ConstantNode(5);
  }
  return node;
});

console.log(transformed.toString());  // '5 ^ 2 + 2 * 5 + 1'
console.log(transformed.evaluate());  // 36
```

### Filtering Nodes

```javascript
import { parse } from 'mathjs';

// Find all symbol nodes
const node = parse('x^2 + y*sin(x) + z');
const symbols = node.filter(n => n.isSymbolNode);
const names = symbols.map(s => s.name);
console.log(names);  // ['x', 'x', 'y', 'x', 'z']

// Find unique symbols
const uniqueNames = [...new Set(names)];
console.log(uniqueNames);  // ['x', 'y', 'z']
```

### Compile Once, Evaluate Many Times

```javascript
import { compile } from 'mathjs';

// Compile expression once
const code = compile('a*x^2 + b*x + c');

// Evaluate with different values efficiently
const results = [];
for (let x = -10; x <= 10; x++) {
  results.push(code.evaluate({ a: 1, b: 0, c: -25, x }));
}
```

### Parser with Persistent State

```javascript
import { parser } from 'mathjs';

const p = parser();

// Define constants and functions
p.evaluate('g = 9.81');
p.evaluate('mass = 10');
p.evaluate('force(m, a) = m * a');

// Use them in calculations
p.evaluate('weight = force(mass, g)');  // 98.1
p.evaluate('weight / 2');               // 49.05

// Serialize and restore parser state
const json = JSON.stringify(p);
// ... later ...
const p2 = JSON.parse(json, math.reviver);
```

## ResultSet Class

When evaluating multiple expressions (arrays of expressions or expressions separated by newlines), the Parser returns a ResultSet containing all evaluated results.

```typescript { .api }
/**
 * Container for multiple expression results
 */
class ResultSet {
  /** Array of all results (both visible and hidden) */
  entries: any[];

  /** Convert to string representation */
  toString(): string;

  /** Convert to JSON */
  toJSON(): any;

  /** Get value (returns entries array) */
  valueOf(): any[];
}
```

**Properties:**
- `entries`: Array containing all evaluated expression results

**Methods:**
- `toString()`: Convert to string showing all entries
- `toJSON()`: Serialize to JSON
- `valueOf()`: Returns entries array

**Usage Examples:**

```javascript
import { parser } from 'mathjs';

const p = parser();

// Evaluate multiple expressions
const result = p.evaluate([
  'a = 5',
  'b = 10',
  'c = a + b'
]);

// result is a ResultSet
result.entries;                   // [5, 10, 15]
result.toString();                // "5\n10\n15"

// With hidden expressions (semicolon suppresses output)
const result2 = p.evaluate(`
  x = 100;
  y = 200;
  x + y
`);

result2.entries;                  // [100, 200, 300]

// Accessing individual results
const [valA, valB, valC] = result.entries;
console.log(valA);                // 5
console.log(valB);                // 10
console.log(valC);                // 15

// Filtering visible results
// (In BlockNode, visible property indicates if semicolon was used)

// Using with evaluate() on arrays
import { evaluate } from 'mathjs';

const results = evaluate([
  '2 + 2',
  '3 * 3',
  '4 ^ 2'
]);

results.entries;                  // [4, 9, 16]

// Iterating results
results.entries.forEach((value, index) => {
  console.log(`Result ${index + 1}: ${value}`);
});
// Result 1: 4
// Result 2: 9
// Result 3: 16

// Check if value is ResultSet
import { isResultSet } from 'mathjs';

if (isResultSet(result)) {
  console.log('Multiple results:', result.entries);
} else {
  console.log('Single result:', result);
}
```

**Note:** A ResultSet is returned when:
- Evaluating an array of expression strings
- Evaluating a single string with multiple expressions (newline-separated)
- A BlockNode containing multiple visible results is evaluated

Single expression evaluations return the result value directly, not wrapped in a ResultSet.
