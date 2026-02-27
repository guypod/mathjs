# mathjs Expression Parser

Version: **15.1.0** | Source: `types/index.d.ts`

## Overview

The mathjs expression parser transforms human-readable mathematical expression strings into an Abstract Syntax Tree (AST) of `MathNode` objects. That tree can be compiled to an `EvalFunction` and evaluated with an optional scope object, enabling symbolic computation, algebraic manipulation, and safe repeated evaluation against changing variable bindings.

**Security note:** Evaluating arbitrary user-supplied expressions carries inherent security risks. Consult the [mathjs security documentation](https://mathjs.org/docs/expressions/security.html) before exposing the parser to untrusted input.

---

## Core Parser Functions

```typescript { .api }
// Parse an expression string into a MathNode tree.
parse(expr: MathExpression, options?: ParseOptions): MathNode
parse(exprs: MathExpression[], options?: ParseOptions): MathNode[]

// Parse and compile an expression into an EvalFunction.
compile(expr: MathExpression): EvalFunction
compile(exprs: MathExpression[]): EvalFunction[]

// Parse and immediately evaluate an expression.
evaluate(expr: MathExpression | Matrix, scope?: MathScope): any
evaluate(expr: MathExpression[], scope?: MathScope): any[]

// Create a stateful Parser instance with a persistent scope.
parser(): Parser

// Retrieve embedded documentation for a function.
help(search: () => any): Help
```

`MathExpression` is `string | string[] | MathCollection`.

`MathScope<TValue = any>` is `Record<string, TValue> | MapLike<string, TValue>`.

---

## ParseFunction Interface

`math.parse` is typed as the `ParseFunction` interface, which exposes both call signatures and a set of static character-classification helpers used internally by the parser.

```typescript { .api }
export interface ParseFunction {
  // Call signatures
  (expr: MathExpression, options?: ParseOptions): MathNode
  (exprs: MathExpression[], options?: ParseOptions): MathNode[]

  // Character classification helpers

  /**
   * Check whether c is a valid identifier alpha character:
   * Latin letters (a-z, A-Z), underscore (_), dollar sign ($),
   * accented Latin letters (U+00C0–U+02AF), Greek letters (U+0370–U+03FF),
   * and mathematical alphanumeric symbols (U+1D400–U+1D7FF).
   * cPrev and cNext are needed for surrogate-pair detection.
   */
  isAlpha(c: string, cPrev: string, cNext: string): boolean

  /**
   * Return true when c is a valid Latin, Greek, or letter-like character.
   */
  isValidLatinOrGreek(c: string): boolean

  /**
   * Return true when the UTF-16 surrogate pair (high, low) encodes a
   * Unicode mathematical symbol (U+1D400–U+1D7FF).
   */
  isValidMathSymbol(high: string, low: string): boolean

  /**
   * Return true when c is whitespace (space, tab, newline).
   * nestingLevel is the current parenthesis/bracket depth.
   */
  isWhitespace(c: string, nestingLevel: number): boolean

  /**
   * Return true when the dot character c is a decimal mark and not the
   * start of a '.', '.*', './', or '.^' operator.
   */
  isDecimalMark(c: string, cNext: string): boolean

  /** Return true when c is a digit (0–9) or a dot (.). */
  isDigitDot(c: string): boolean

  /** Return true when c is a decimal digit (0–9). */
  isDigit(c: string): boolean

  /** Return true when c is a hexadecimal digit (0–9, a–f, A–F). */
  isHexDigit(c: string): boolean
}
```

---

## ParseOptions Interface

```typescript { .api }
export interface ParseOptions {
  /**
   * A map of custom node constructors keyed by node type name.
   * Allows injecting user-defined node classes into the parser.
   */
  nodes?: Record<string, MathNode>
}
```

---

## EvalFunction Interface

Returned by `parse(...).compile()` and by `math.compile(...)`. Holds compiled JavaScript code ready for repeated evaluation.

```typescript { .api }
export interface EvalFunction {
  /**
   * Evaluate the compiled expression.
   * @param scope  Optional variable bindings. Keys are variable names;
   *               values are the current bindings. Mutations to scope
   *               are reflected in subsequent calls.
   * @returns      The result of the expression.
   */
  evaluate(scope?: MathScope): any
}
```

---

## Parser Interface

A stateful session created by `math.parser()`. Maintains its own variable scope across multiple `evaluate` calls.

```typescript { .api }
export interface Parser {
  /**
   * Evaluate an expression (or array of expressions) in the parser's scope.
   * Assignments and function definitions persist between calls.
   */
  evaluate(expr: string | string[]): any

  /**
   * Retrieve a variable or function from the parser's scope by name.
   */
  get(name: string): any

  /**
   * Retrieve all defined variables and functions as a plain object.
   */
  getAll(): { [key: string]: any }

  /**
   * Retrieve all defined variables and functions as a Map.
   */
  getAllAsMap(): Map<string, any>

  /**
   * Set a variable or function in the parser's scope.
   */
  set: (name: string, value: any) => void

  /**
   * Remove a named variable or function from the parser's scope.
   */
  remove: (name: string) => void

  /**
   * Clear all variables and functions from the parser's scope.
   */
  clear: () => void
}
```

---

## Help Interface

Returned by `math.help(fn)`. Provides documentation for a built-in function.

```typescript { .api }
export interface Help {
  /** Format the documentation as a human-readable string. */
  toString(): string

  /** Serialize the documentation to a JSON string. */
  toJSON(): string
}
```

---

## MathNode Base Interface

Every node in the AST implements `MathNode`. Concrete node types narrow `type` to a specific string literal and add their own properties.

```typescript { .api }
export interface MathNode {
  /** Always true; used as a type guard. */
  isNode: true

  /** String discriminator identifying the node subtype. */
  type: string

  /** Source comment text attached to this node, if any. */
  comment: string

  /** True when this node performs an in-place update (e.g. assignment). */
  isUpdateNode?: boolean

  // --- Evaluation ---

  /**
   * Compile the node to an EvalFunction for efficient repeated evaluation.
   */
  compile(): EvalFunction

  /**
   * Shorthand for node.compile().evaluate(scope).
   */
  evaluate(scope?: MathScope): any

  // --- Cloning ---

  /**
   * Create a shallow clone. Child nodes are shared with the original.
   */
  clone(): this

  /**
   * Create a deep clone. All descendant nodes are cloned recursively.
   */
  cloneDeep(): this

  // --- Comparison ---

  /**
   * Deep structural equality check against another MathNode.
   */
  equals(other: MathNode): boolean

  // --- Tree traversal ---

  /**
   * Iterate over direct child nodes only (non-recursive).
   * callback(node, path, parent) — path is a relative JSON Path string.
   */
  forEach(
    callback: (node: MathNode, path: string, parent: MathNode) => void
  ): void

  /**
   * Rebuild the node by replacing each direct child with the return value
   * of the callback. Non-recursive; use transform() for full tree rewrites.
   * callback(node, path, parent) must return a MathNode.
   */
  map(
    callback: (node: MathNode, path: string, parent: MathNode) => MathNode
  ): MathNode

  /**
   * Collect all nodes in the subtree for which the callback returns truthy.
   * callback(node, path, parent) — returns an array of matching MathNodes.
   */
  filter(
    callback: (node: MathNode, path: string, parent: MathNode) => any
  ): MathNode[]

  /**
   * Recursively transform the entire subtree. The callback is called for
   * every node top-down; its return value replaces the node.
   * callback(node, path, parent) must return a MathNode (or TResult).
   */
  transform<TResult>(
    callback: (node: this, path: string, parent: MathNode) => TResult
  ): TResult

  /**
   * Recursively traverse the entire subtree without modifying it.
   * callback(node, path, parent) — return value is ignored.
   */
  traverse(
    callback: (node: MathNode, path: string, parent: MathNode) => void
  ): void

  // --- Serialization ---

  /** Render the node as a math expression string. */
  toString(options?: object): string

  /** Render the node as an HTML string. */
  toHTML(options?: object): string

  /** Render the node as a LaTeX string. */
  toTex(options?: object): string
}
```

---

## AST Node Types

All node types extend `MathNode`. The `type` property is a string literal that uniquely identifies each subtype, enabling discriminated union narrowing.

### AccessorNode

Property or index access expression, e.g. `a.b` or `a[i]`.

```typescript { .api }
export interface AccessorNode<TObject extends MathNode = MathNode>
  extends MathNode {
  type: 'AccessorNode'
  isAccessorNode: true

  /** The object being accessed. */
  object: TObject

  /** The index expression (dimensions/keys). */
  index: IndexNode

  /** The accessed property name (dot-notation access). */
  name: string

  /** True when using optional chaining syntax (a?.b). */
  optionalChaining: boolean
}

export interface AccessorNodeCtor {
  new <TObject extends MathNode = MathNode>(
    object: TObject,
    index: IndexNode,
    optionalChaining?: boolean
  ): AccessorNode<TObject>
}
```

### ArrayNode

Array or matrix literal, e.g. `[1, 2, 3]`.

```typescript { .api }
export interface ArrayNode<TItems extends MathNode[] = MathNode[]>
  extends MathNode {
  type: 'ArrayNode'
  isArrayNode: true

  /** Ordered list of element nodes. */
  items: [...TItems]
}

export interface ArrayNodeCtor {
  new <TItems extends MathNode[] = MathNode[]>(
    items: [...TItems]
  ): ArrayNode<TItems>
}
```

### AssignmentNode

Variable or indexed assignment, e.g. `a = 3` or `a[i] = 5`.

```typescript { .api }
export interface AssignmentNode<TValue extends MathNode = MathNode>
  extends MathNode {
  type: 'AssignmentNode'
  isAssignmentNode: true

  /** The target symbol or accessor being assigned to. */
  object: SymbolNode | AccessorNode

  /** Index node for indexed assignment; null for plain assignment. */
  index: IndexNode | null

  /** The value being assigned. */
  value: TValue

  /** The name of the assigned variable. */
  name: string
}

export interface AssignmentNodeCtor {
  // Plain assignment: a = value
  new <TValue extends MathNode = MathNode>(
    object: SymbolNode,
    value: TValue
  ): AssignmentNode<TValue>

  // Indexed assignment: a[index] = value
  new <TValue extends MathNode = MathNode>(
    object: SymbolNode | AccessorNode,
    index: IndexNode,
    value: TValue
  ): AssignmentNode<TValue>
}
```

### BlockNode

Semicolon-separated statement block, e.g. `a = 1; b = 2`. Each block entry carries a `visible` flag controlling whether its result appears in output.

```typescript { .api }
export interface BlockNode<TNode extends MathNode = MathNode> extends MathNode {
  type: 'BlockNode'
  isBlockNode: true

  /** Ordered array of {node, visible} entries. */
  blocks: Array<{ node: TNode; visible: boolean }>
}

export interface BlockNodeCtor {
  new <TNode extends MathNode = MathNode>(
    arr: Array<{ node: TNode } | { node: TNode; visible: boolean }>
  ): BlockNode
}
```

### ConditionalNode

Ternary conditional expression, e.g. `a > 0 ? a : -a`.

```typescript { .api }
export interface ConditionalNode<
  TCond extends MathNode = MathNode,
  TTrueNode extends MathNode = MathNode,
  TFalseNode extends MathNode = MathNode
> extends MathNode {
  type: 'ConditionalNode'
  isConditionalNode: boolean

  /** The boolean condition. */
  condition: TCond

  /** Node evaluated when condition is truthy. */
  trueExpr: TTrueNode

  /** Node evaluated when condition is falsy. */
  falseExpr: TFalseNode
}

export interface ConditionalNodeCtor {
  new <
    TCond extends MathNode = MathNode,
    TTrueNode extends MathNode = MathNode,
    TFalseNode extends MathNode = MathNode
  >(
    condition: TCond,
    trueExpr: TTrueNode,
    falseExpr: TFalseNode
  ): ConditionalNode
}
```

### ConstantNode

A numeric, string, boolean, `null`, `undefined`, `bigint`, `BigNumber`, or `Fraction` literal.

```typescript { .api }
export interface ConstantNode<
  TValue extends
    | string
    | number
    | boolean
    | null
    | undefined
    | bigint
    | BigNumber
    | Fraction = number
> extends MathNode {
  type: 'ConstantNode'
  isConstantNode: true

  /** The literal value. */
  value: TValue
}

export interface ConstantNodeCtor {
  new <
    TValue extends
      | string
      | number
      | boolean
      | null
      | undefined
      | bigint
      | BigNumber
      | Fraction = string
  >(
    value: TValue
  ): ConstantNode<TValue>
}
```

### FunctionAssignmentNode

User-defined function definition, e.g. `f(x) = x^2`.

```typescript { .api }
export interface FunctionAssignmentNode<TExpr extends MathNode = MathNode>
  extends MathNode {
  type: 'FunctionAssignmentNode'
  isFunctionAssignmentNode: true

  /** Function name. */
  name: string

  /** Ordered list of parameter names. */
  params: string[]

  /** The function body expression. */
  expr: TExpr
}

export interface FunctionAssignmentNodeCtor {
  new <TExpr extends MathNode = MathNode>(
    name: string,
    params: string[],
    expr: TExpr
  ): FunctionAssignmentNode<TExpr>
}
```

### FunctionNode

Function call expression, e.g. `sin(x)`.

```typescript { .api }
export interface FunctionNode<
  TFn = SymbolNode,
  TArgs extends MathNode[] = MathNode[]
> extends MathNode {
  type: 'FunctionNode'
  isFunctionNode: true

  /** The function reference (typically a SymbolNode). */
  fn: TFn

  /** Ordered list of argument nodes. */
  args: [...TArgs]
}

export interface FunctionNodeCtor {
  new <TFn = SymbolNode, TArgs extends MathNode[] = MathNode[]>(
    fn: TFn,
    args: [...TArgs]
  ): FunctionNode<TFn, TArgs>

  /**
   * Hook called when a function name cannot be resolved in scope.
   * Override to provide custom error handling or default behaviour.
   */
  onUndefinedFunction: (name: string) => any
}
```

### IndexNode

Index subscript expression used inside `AccessorNode`, e.g. `[i, j]`.

```typescript { .api }
export interface IndexNode<TDims extends MathNode[] = MathNode[]>
  extends MathNode {
  type: 'IndexNode'
  isIndexNode: true

  /** Ordered list of dimension/key expressions. */
  dimensions: [...TDims]

  /** True when accessed via dot notation (a.b vs a['b']). */
  dotNotation: boolean
}

export interface IndexNodeCtor {
  new <TDims extends MathNode[] = MathNode[]>(dimensions: [...TDims]): IndexNode
  new <TDims extends MathNode[] = MathNode[]>(
    dimensions: [...TDims],
    dotNotation: boolean
  ): IndexNode<TDims>
}
```

### ObjectNode

Object literal expression, e.g. `{a: 1, b: 2}`.

```typescript { .api }
export interface ObjectNode<
  TProps extends Record<string, MathNode> = Record<string, MathNode>
> extends MathNode {
  type: 'ObjectNode'
  isObjectNode: true

  /** Map of property name strings to their value nodes. */
  properties: TProps
}

export interface ObjectNodeCtor {
  new <TProps extends Record<string, MathNode> = Record<string, MathNode>>(
    properties: TProps
  ): ObjectNode<TProps>
}
```

### OperatorNode

Operator expression, e.g. `a + b`, `-x`, `a!`.

```typescript { .api }
export interface OperatorNode<
  TOp extends OperatorNodeMap[TFn] = never,
  TFn extends OperatorNodeFn = never,
  TArgs extends MathNode[] = MathNode[]
> extends MathNode {
  type: 'OperatorNode'
  isOperatorNode: true

  /** The operator symbol string (e.g. '+', '-', '*'). */
  op: TOp

  /** The mathjs function name implementing the operator. */
  fn: TFn

  /** Ordered list of operand nodes. */
  args: [...TArgs]

  /**
   * True when the multiplication is implicit (juxtaposition),
   * e.g. `2x` instead of `2 * x`.
   */
  implicit: boolean

  /** Returns true when this node has exactly one operand. */
  isUnary(): boolean

  /** Returns true when this node has exactly two operands. */
  isBinary(): boolean
}

export interface OperatorNodeCtor extends MathNode {
  new <
    TOp extends OperatorNodeMap[TFn],
    TFn extends OperatorNodeFn,
    TArgs extends MathNode[]
  >(
    op: TOp,
    fn: TFn,
    args: [...TArgs],
    implicit?: boolean
  ): OperatorNode<TOp, TFn, TArgs>
}
```

#### OperatorNodeMap, OperatorNodeOp, OperatorNodeFn

`OperatorNodeMap` maps mathjs function names to their operator symbol strings. `OperatorNodeFn` is `keyof OperatorNodeMap`. `OperatorNodeOp` is `OperatorNodeMap[keyof OperatorNodeMap]`.

```typescript { .api }
export type OperatorNodeMap = {
  xor:             'xor'
  and:             'and'
  or:              'or'
  bitOr:           '|'
  bitXor:          '^|'
  bitAnd:          '&'
  equal:           '=='
  unequal:         '!='
  smaller:         '<'
  larger:          '>'
  smallerEq:       '<='
  largerEq:        '>='
  leftShift:       '<<'
  rightArithShift: '>>'
  rightLogShift:   '>>>'
  to:              'to'
  add:             '+'
  subtract:        '-'
  multiply:        '*'
  divide:          '/'
  dotMultiply:     '.*'
  dotDivide:       './'
  mod:             'mod'
  unaryPlus:       '+'
  unaryMinus:      '-'
  bitNot:          '~'
  not:             'not'
  pow:             '^'
  dotPow:          '.^'
  factorial:       '!'
}

export type OperatorNodeOp = OperatorNodeMap[keyof OperatorNodeMap]
export type OperatorNodeFn = keyof OperatorNodeMap
```

### ParenthesisNode

An explicitly parenthesized sub-expression, e.g. `(a + b)`. Parenthesis nodes are only produced when the parser option `implicit` is `'keep'` or when parentheses affect precedence.

```typescript { .api }
export interface ParenthesisNode<TContent extends MathNode = MathNode>
  extends MathNode {
  type: 'ParenthesisNode'
  isParenthesisNode: true

  /** The enclosed expression. */
  content: TContent
}

export interface ParenthesisNodeCtor {
  new <TContent extends MathNode>(content: TContent): ParenthesisNode<TContent>
}
```

### RangeNode

Range expression, e.g. `1:10` or `1:2:10` (with step).

```typescript { .api }
export interface RangeNode<
  TStart extends MathNode = MathNode,
  TEnd extends MathNode = MathNode,
  TStep extends MathNode = MathNode
> extends MathNode {
  type: 'RangeNode'
  isRangeNode: true

  /** Start of the range (inclusive). */
  start: TStart

  /** End of the range (inclusive). */
  end: TEnd

  /** Optional step size; null when using the default step of 1. */
  step: TStep | null
}

export interface RangeNodeCtor {
  new <
    TStart extends MathNode = MathNode,
    TEnd extends MathNode = MathNode,
    TStep extends MathNode = MathNode
  >(
    start: TStart,
    end: TEnd,
    step?: TStep
  ): RangeNode<TStart, TEnd, TStep>
}
```

### RelationalNode

Chained relational comparison, e.g. `a < b < c`. Each comparison in the chain is stored as a string in `conditionals`; the operands are in `params`.

```typescript { .api }
export interface RelationalNode<TParams extends MathNode[] = MathNode[]>
  extends MathNode {
  type: 'RelationalNode'
  isRelationalNode: true

  /**
   * Array of comparison operator strings, e.g. ['<', '<'].
   * Length is always params.length - 1.
   */
  conditionals: string[]

  /** Ordered list of operand nodes for the chained comparisons. */
  params: [...TParams]
}

export interface RelationalNodeCtor {
  new <TParams extends MathNode[] = MathNode[]>(
    conditionals: string[],
    params: [...TParams]
  ): RelationalNode<TParams>
}
```

### SymbolNode

A named variable or constant reference, e.g. `x`, `pi`, `true`.

```typescript { .api }
export interface SymbolNode extends MathNode {
  type: 'SymbolNode'
  isSymbolNode: true

  /** The identifier name. */
  name: string
}

export interface SymbolNodeCtor {
  new (name: string): SymbolNode

  /**
   * Hook called when a symbol name cannot be resolved in scope.
   * Override to provide custom error handling or default behaviour.
   */
  onUndefinedSymbol: (name: string) => any
}
```

---

## Node Constructors

The base `NodeCtor` and each concrete constructor interface are exported separately. Constructors are accessible via `math.expression.node.*` at runtime.

```typescript { .api }
export interface NodeCtor {
  new (): MathNode
}

// Concrete constructors (see individual node sections for signatures):
export interface AccessorNodeCtor      { /* see AccessorNode section */      }
export interface ArrayNodeCtor         { /* see ArrayNode section */         }
export interface AssignmentNodeCtor    { /* see AssignmentNode section */    }
export interface BlockNodeCtor         { /* see BlockNode section */         }
export interface ConditionalNodeCtor   { /* see ConditionalNode section */   }
export interface ConstantNodeCtor      { /* see ConstantNode section */      }
export interface FunctionAssignmentNodeCtor { /* see FunctionAssignmentNode section */ }
export interface FunctionNodeCtor      { /* see FunctionNode section */      }
export interface IndexNodeCtor         { /* see IndexNode section */         }
export interface ObjectNodeCtor        { /* see ObjectNode section */        }
export interface OperatorNodeCtor      { /* see OperatorNode section */      }
export interface ParenthesisNodeCtor   { /* see ParenthesisNode section */   }
export interface RangeNodeCtor         { /* see RangeNode section */         }
export interface RelationalNodeCtor    { /* see RelationalNode section */    }
export interface SymbolNodeCtor        { /* see SymbolNode section */        }
```

---

## Usage Examples

### Parsing and evaluating an expression

```typescript { .api }
import { parse, evaluate } from 'mathjs'

// One-shot evaluation
evaluate('12 / (2.3 + 0.7)')        // => 4
evaluate('sin(45 deg) ^ 2')         // => 0.5
evaluate('det([-1, 2; 3, 1])')      // => -7

// Parse to a node tree, then compile for reuse
const node = parse('sqrt(3^2 + 4^2)')
const code = node.compile()
code.evaluate()                      // => 5

// Compile once, evaluate repeatedly with changing scope
const expr = parse('a * b')
const compiled = expr.compile()
const scope = { a: 3, b: 4 }
compiled.evaluate(scope)             // => 12
scope.a = 5
compiled.evaluate(scope)             // => 20
```

### Parsing multiple expressions

```typescript { .api }
import { parse } from 'mathjs'

const nodes = parse(['a = 3', 'b = 4', 'a * b'])
nodes[2].compile().evaluate()        // => 12
```

### Using the stateful Parser

```typescript { .api }
import { parser } from 'mathjs'

const p = parser()

p.evaluate('x = 7 / 2')             // => 3.5
p.evaluate('x + 3')                 // => 6.5

p.set('y', 100)
p.evaluate('y / 4')                 // => 25

p.get('x')                          // => 3.5
p.getAll()                          // => { x: 3.5, y: 100 }

p.remove('x')
p.clear()
```

### Traversing the AST

```typescript { .api }
import { parse } from 'mathjs'

const node = parse('x^2 + x/4 + 3*y')

// filter: collect all SymbolNodes named 'x'
const xNodes = node.filter(
  (n) => n.type === 'SymbolNode' && (n as SymbolNode).name === 'x'
)
// xNodes.length === 2

// traverse: log every node type and key value
node.traverse((n, path, parent) => {
  switch (n.type) {
    case 'OperatorNode':
      console.log(n.type, (n as OperatorNode).op)
      break
    case 'ConstantNode':
      console.log(n.type, (n as ConstantNode).value)
      break
    case 'SymbolNode':
      console.log(n.type, (n as SymbolNode).name)
      break
  }
})
```

### Transforming the AST

```typescript { .api }
import { parse, ConstantNode, SymbolNode } from 'mathjs'

// Replace every occurrence of symbol 'x' with the constant 3
const node = parse('x^2 + 5*x')
const transformed = node.transform((n) => {
  if (n.type === 'SymbolNode' && (n as SymbolNode).name === 'x') {
    return new ConstantNode(3)
  }
  return n
})
transformed.toString()               // => '3 ^ 2 + 5 * 3'
transformed.compile().evaluate()     // => 24
```

### Custom nodes via ParseOptions

```typescript { .api }
import { parse, MathNode } from 'mathjs'

// Supply a custom node class under a chosen type name
class MyNode implements MathNode { /* ... */ }

const node = parse('myFunc(x)', {
  nodes: { myFunc: new MyNode() }
})
```
