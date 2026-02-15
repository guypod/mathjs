# Chaining API

Math.js provides a chainable interface that allows fluent, readable syntax for sequential mathematical operations. Instead of nesting function calls, you can chain operations together for improved code clarity.

## Core Imports

```javascript { .api }
import { chain } from 'mathjs';
```

## Capabilities

### Creating Chains

Start a chain by wrapping a value with the `chain()` function.

```typescript { .api }
/**
 * Create a chainable math.js object
 * @param value - Initial value to chain operations on
 * @returns Chainable object with all math.js functions available
 */
function chain<T>(value: T): MathJsChain<T>;
```

**Parameters:**
- `value`: Initial value (number, array, matrix, or any math.js type)

**Returns:** Chain object with all math.js functions available as chainable methods

**Usage Examples:**

```javascript
import { chain } from 'mathjs';

// Basic chaining
const result = chain(5)
  .add(3)
  .multiply(2)
  .done();
// Result: 16 (equivalent to multiply(add(5, 3), 2))

// Chaining with more operations
const result2 = chain(10)
  .subtract(3)
  .divide(2)
  .pow(2)
  .done();
// Result: 12.25 (equivalent to pow(divide(subtract(10, 3), 2), 2))

// Matrix operations
const result3 = chain([[1, 2], [3, 4]])
  .multiply([[2, 0], [0, 2]])
  .add([[1, 1], [1, 1]])
  .done();
// Result: [[3, 5], [7, 9]]

// Complex calculations
const result4 = chain(45)
  .multiply(Math.PI / 180)  // Convert to radians
  .sin()
  .pow(2)
  .done();
// Result: 0.5 (sin²(45°))
```

### Finishing Chains

Extract the final value from a chain using `done()` or `valueOf()`.

```typescript { .api }
interface MathJsChain<T> {
  /**
   * Extract the final result from the chain
   * @returns The computed value
   */
  done(): T;

  /**
   * Alias for done() - extract the final result
   * @returns The computed value
   */
  valueOf(): T;
}
```

**Usage Examples:**

```javascript
import { chain } from 'mathjs';

const ch = chain(5).add(3).multiply(2);

// Using done()
ch.done();                    // 16

// Using valueOf()
ch.valueOf();                 // 16

// Implicit valueOf() in expressions
const x = ch + 10;            // 26 (chain is auto-converted)
console.log(ch);              // Calls valueOf() implicitly
```

### All Mathematical Functions Are Chainable

Every math.js function is available as a chainable method. The chain object acts as the first argument to the function.

```javascript
import { chain } from 'mathjs';

// Arithmetic operations
chain(10).add(5).done();                // 15
chain(10).subtract(3).done();           // 7
chain(4).multiply(5).done();            // 20
chain(20).divide(4).done();             // 5
chain(2).pow(8).done();                 // 256
chain(25).sqrt().done();                // 5

// Trigonometry
chain(Math.PI / 2).sin().done();        // 1
chain(Math.PI).cos().done();            // -1
chain(1).asin().done();                 // π/2

// Rounding
chain(3.7).round().done();              // 4
chain(3.7).floor().done();              // 3
chain(3.2).ceil().done();               // 4

// Logarithms
chain(100).log10().done();              // 2
chain(Math.E).log().done();             // 1

// Absolute value and sign
chain(-5).abs().done();                 // 5
chain(-3.5).sign().done();              // -1

// Complex numbers
chain(complex(3, 4)).abs().done();      // 5

// Arrays and matrices
chain([1, 2, 3, 4])
  .map(x => x * 2)
  .filter(x => x > 5)
  .done();
// [6, 8]

chain([[1, 2], [3, 4]])
  .transpose()
  .det()
  .done();
// -2
```

### Chaining Type Constructors

Convert between types while chaining.

```javascript
import { chain } from 'mathjs';

// Convert to BigNumber
chain('123.456')
  .bignumber()
  .multiply(2)
  .done();
// BigNumber 246.912

// Convert to Fraction
chain(0.5)
  .fraction()
  .add(fraction(1, 3))
  .done();
// Fraction 5/6

// Convert to Complex
chain(3)
  .complex()
  .multiply(complex(0, 1))  // Multiply by i
  .done();
// Complex 0 + 3i

// Convert to Unit
chain(5)
  .unit('cm')
  .to('inch')
  .done();
// Unit 1.9685... inch
```

### Chaining Matrix Operations

Perform sophisticated matrix manipulations using chains.

```javascript
import { chain, matrix } from 'mathjs';

// Matrix chain example
const result = chain(matrix([[1, 2], [3, 4]]))
  .transpose()
  .multiply(2)
  .add(matrix([[1, 0], [0, 1]]))
  .done();
// Result: [[3, 6], [4, 9]]

// Solving linear systems
const solution = chain([[2, 1], [1, 3]])
  .multiply([[5], [6]])
  .done();
// Matrix multiplication result

// More complex pipeline
const data = chain([1, 2, 3, 4, 5, 6, 7, 8, 9])
  .reshape([3, 3])
  .transpose()
  .map(x => x * 2)
  .done();
```

### Chaining Statistical Functions

Process data through statistical transformations.

```javascript
import { chain } from 'mathjs';

// Statistical pipeline
const result = chain([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
  .map(x => x * x)
  .mean()
  .sqrt()
  .done();
// Root mean square

// Filtering and statistics
const filtered = chain([1, 5, 2, 8, 3, 9, 4])
  .filter(x => x > 3)
  .std()
  .done();
// Standard deviation of values > 3
```

### Chaining Expression Evaluation

Chains work seamlessly with evaluated expressions.

```javascript
import { chain, evaluate } from 'mathjs';

// Evaluate and chain
const result = chain(evaluate('5 + 3'))
  .multiply(2)
  .subtract(4)
  .done();
// 12

// Chain multiple evaluations
const scope = { a: 5, b: 3 };
const result2 = chain(evaluate('a * b', scope))
  .pow(2)
  .sqrt()
  .done();
// 15
```

## Comparison: Nested vs Chained

**Nested (traditional):**
```javascript
import { add, multiply, divide, sqrt } from 'mathjs';

const result = sqrt(divide(multiply(add(3, 5), 2), 4));
// Hard to read: operations are inside-out
```

**Chained (fluent):**
```javascript
import { chain } from 'mathjs';

const result = chain(3)
  .add(5)
  .multiply(2)
  .divide(4)
  .sqrt()
  .done();
// Easy to read: operations flow left-to-right
```

## Type Safety

The chain interface preserves type information:

```typescript
import { chain, MathJsChain } from 'mathjs';

// Type is preserved through the chain
const numberChain: MathJsChain<number> = chain(5);
const result: number = numberChain.add(3).done();

// Matrix types
const matrixChain = chain([[1, 2], [3, 4]]);
const transposed = matrixChain.transpose().done();
```

## Common Types

```typescript { .api }
interface MathJsChain<TValue> {
  done(): TValue;
  valueOf(): TValue;

  // All math.js functions available as methods
  add(y: MathType): MathJsChain<MathType>;
  subtract(y: MathType): MathJsChain<MathType>;
  multiply(y: MathType): MathJsChain<MathType>;
  divide(y: MathType): MathJsChain<MathType>;
  // ... (250+ functions available)
}
```

## When to Use Chains

**Use chains when:**
- Performing multiple sequential operations
- Improving code readability
- Building data processing pipelines
- Working with intermediate results

**Use regular functions when:**
- Single operation needed
- Performance is critical (chains have slight overhead)
- Working with functional programming patterns (map, reduce, etc.)
