# Chain Interface

Fluent API for composing mathematical operations with automatic result chaining. The chain interface allows you to write more readable code by chaining method calls instead of nesting function calls.

## Capabilities

### Chain Creation

Create a chainable wrapper around any value.

```javascript { .api }
/**
 * Create a chainable wrapper
 * @param value - Initial value to chain operations on
 * @returns MathJsChain instance
 */
function chain(value?: any): MathJsChain

interface MathJsChain {
  /**
   * Unwrap and return the final result
   * @returns The computed value
   */
  done(): any

  /**
   * All math functions are available as methods
   * Each method returns a new chain for further operations
   */
  // Arithmetic
  add(y: any): MathJsChain
  subtract(y: any): MathJsChain
  multiply(y: any): MathJsChain
  divide(y: any): MathJsChain
  pow(y: any): MathJsChain
  sqrt(): MathJsChain
  // ... all other math functions
}
```

**Usage Examples:**

```javascript
import { chain } from 'mathjs'

// Basic chaining
chain(3)
  .add(4)
  .multiply(2)
  .done()                       // 14

// More complex calculation
chain(16)
  .sqrt()
  .add(2)
  .multiply(3)
  .subtract(1)
  .done()                       // 17

// With different types
chain(complex(3, 4))
  .abs()
  .pow(2)
  .done()                       // 25

// Chain matrix operations
chain(matrix([[1, 2], [3, 4]]))
  .transpose()
  .multiply(2)
  .done()                       // Matrix [[2, 6], [4, 8]]
```

## Method Chaining vs Function Nesting

The chain interface provides a more readable alternative to nested function calls.

**Without chaining (nested):**

```javascript
import { subtract, multiply, add, sqrt } from 'mathjs'

// Deeply nested
const result1 = subtract(multiply(add(sqrt(16), 2), 3), 1)  // Hard to read

// With intermediate variables
const step1 = sqrt(16)          // 4
const step2 = add(step1, 2)     // 6
const step3 = multiply(step2, 3) // 18
const result2 = subtract(step3, 1) // 17
```

**With chaining (fluent):**

```javascript
import { chain } from 'mathjs'

// Clear, left-to-right reading
const result = chain(16)
  .sqrt()
  .add(2)
  .multiply(3)
  .subtract(1)
  .done()                       // 17
```

## Available Methods

All Math.js functions are available as chain methods. The chained value is passed as the first argument to each function.

### Arithmetic Operations

```javascript
import { chain } from 'mathjs'

chain(10)
  .add(5)                       // 15
  .subtract(3)                  // 12
  .multiply(2)                  // 24
  .divide(4)                    // 6
  .pow(2)                       // 36
  .sqrt()                       // 6
  .done()

chain(5)
  .square()                     // 25
  .add(11)                      // 36
  .sqrt()                       // 6
  .done()

chain(-5)
  .abs()                        // 5
  .log()                        // ln(5) ≈ 1.609
  .exp()                        // e^1.609 ≈ 5
  .done()
```

### Trigonometric Functions

```javascript
import { chain, pi } from 'mathjs'

chain(pi)
  .divide(4)                    // π/4
  .sin()                        // 0.707...
  .pow(2)                       // 0.5
  .done()

chain(1)
  .asin()                       // π/2
  .multiply(2)                  // π
  .cos()                        // -1
  .done()
```

### Matrix Operations

```javascript
import { chain } from 'mathjs'

chain([[1, 2], [3, 4]])
  .matrix()                     // Convert to Matrix
  .transpose()                  // [[1, 3], [2, 4]]
  .multiply(2)                  // [[2, 6], [4, 8]]
  .det()                        // -8
  .done()

chain([[2, 1], [1, 3]])
  .matrix()
  .inv()                        // Inverse matrix
  .multiply([[5], [7]])         // Solve system
  .done()
```

### Statistical Functions

```javascript
import { chain } from 'mathjs'

chain([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
  .mean()                       // 5.5
  .done()

chain([2, 4, 4, 4, 5, 5, 7, 9])
  .std()                        // Standard deviation
  .round(2)                     // Round to 2 decimals
  .done()

chain([1, 5, 3, 9, 2, 8, 4])
  .sort()                       // [1, 2, 3, 4, 5, 8, 9]
  .median()                     // 4
  .done()
```

### Complex Numbers

```javascript
import { chain, complex } from 'mathjs'

chain(complex(3, 4))
  .abs()                        // 5 (magnitude)
  .done()

chain(complex(1, 0))
  .multiply(complex(0, 1))      // i
  .pow(2)                       // -1
  .done()

chain(-1)
  .sqrt()                       // i
  .multiply(complex(0, 1))      // -1
  .done()
```

### Units

```javascript
import { chain, unit } from 'mathjs'

chain(unit('5 cm'))
  .to('inch')                   // 1.9685 inch
  .multiply(2)                  // 3.937 inch
  .done()

chain(unit('60 mile'))
  .divide(unit('1 hour'))       // 60 mile/hour
  .to('m/s')                    // 26.822 m/s
  .done()
```

## Advanced Chaining Patterns

### Conditional Chaining

Use JavaScript control flow with chaining:

```javascript
import { chain } from 'mathjs'

function compute(x, useSquare) {
  let c = chain(x).add(5)

  if (useSquare) {
    c = c.square()
  } else {
    c = c.multiply(2)
  }

  return c.subtract(1).done()
}

compute(3, true)                // (3+5)^2 - 1 = 63
compute(3, false)               // (3+5)*2 - 1 = 15
```

### Iterative Chaining

Build chains dynamically:

```javascript
import { chain } from 'mathjs'

function applyOperations(value, operations) {
  let c = chain(value)

  for (const [op, arg] of operations) {
    c = c[op](arg)
  }

  return c.done()
}

applyOperations(10, [
  ['add', 5],
  ['multiply', 2],
  ['subtract', 3]
])                              // 27
```

### Reusable Chain Segments

Create reusable chain templates:

```javascript
import { chain } from 'mathjs'

function normalizeAndScale(data, scale) {
  return chain(data)
    .subtract(chain(data).mean().done())
    .divide(chain(data).std().done())
    .multiply(scale)
    .done()
}

const data = [1, 2, 3, 4, 5]
normalizeAndScale(data, 10)     // Normalized and scaled data
```

### Complex Calculations

Chain complex multi-step calculations:

```javascript
import { chain, matrix } from 'mathjs'

// Solve linear system Ax = b
const A = [[2, 1], [1, 3]]
const b = [5, 7]

const solution = chain(A)
  .matrix()
  .lusolve(b)
  .done()                       // [1, 2]

// Calculate eigenvalues
const eigenvalues = chain([[2, 1], [1, 2]])
  .matrix()
  .eigs()
  .get('values')
  .done()                       // [3, 1]
```

## Comparison with Direct Function Calls

### Readability Example

**Function calls (hard to read):**

```javascript
import { evaluate, simplify, derivative, parse } from 'mathjs'

const result = evaluate(
  simplify(
    derivative(
      parse('x^3 + 2*x^2 + x'),
      'x'
    )
  ).toString(),
  { x: 2 }
)  // Hard to follow the flow
```

**Chain interface (easier to read):**

```javascript
import { chain } from 'mathjs'

const result = chain('x^3 + 2*x^2 + x')
  .parse()
  .derivative('x')
  .simplify()
  .evaluate({ x: 2 })
  .done()                       // Clear step-by-step flow
```

### Performance Considerations

- Chaining has minimal overhead (just wrapper objects)
- Use when readability is more important than microseconds
- For tight loops, direct function calls may be slightly faster
- No performance difference for complex calculations

```javascript
import { chain, add, multiply, sqrt } from 'mathjs'

// Direct calls (slightly faster in tight loops)
for (let i = 0; i < 1000000; i++) {
  const result = sqrt(multiply(add(i, 5), 2))
}

// Chaining (more readable, minimal overhead)
for (let i = 0; i < 1000000; i++) {
  const result = chain(i).add(5).multiply(2).sqrt().done()
}
```

## Integration with Other Features

### With Expression Parsing

```javascript
import { chain } from 'mathjs'

chain('sin(pi/4)^2 + cos(pi/4)^2')
  .evaluate()                   // 1
  .done()

chain('x^2 + 2*x + 1')
  .parse()
  .simplify()
  .toString()
  .done()                       // '(x + 1) ^ 2'
```

### With Matrix Operations

```javascript
import { chain } from 'mathjs'

chain([[1, 2, 3], [4, 5, 6]])
  .matrix()
  .subset(chain.index(0, [0, 2]))  // First row, columns 0-1
  .done()                       // [1, 2]

chain([[1, 2], [3, 4]])
  .matrix()
  .map(x => x * 2)
  .det()
  .done()                       // -16
```

## Chain Method Reference

All methods return a new `MathJsChain` except `done()`:

| Category | Methods |
|----------|---------|
| **Core** | `done()` |
| **Arithmetic** | `abs`, `add`, `ceil`, `cube`, `divide`, `exp`, `floor`, `log`, `mod`, `multiply`, `pow`, `round`, `sign`, `sqrt`, `square`, `subtract` |
| **Trigonometry** | `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `sinh`, `cosh`, `tanh`, etc. |
| **Matrix** | `det`, `inv`, `transpose`, `eigs`, `lup`, `qr`, `trace`, `cross`, `dot` |
| **Statistics** | `mean`, `median`, `std`, `variance`, `min`, `max`, `sum`, `prod` |
| **Logical** | `and`, `or`, `not`, `xor`, `equal`, `larger`, `smaller` |
| **Complex** | `re`, `im`, `arg`, `conj`, `complex` |
| **Units** | `unit`, `to` |
| **Collections** | `filter`, `map`, `forEach`, `sort`, `concat`, `size` |
| **Evaluation** | `evaluate`, `parse`, `compile`, `simplify`, `derivative` |

## Best Practices

1. **Use for readability**: Chain when operations have a clear sequential flow
2. **Call `done()` at the end**: Always unwrap the final result
3. **Mix with direct calls**: Use chaining where it improves readability, direct calls elsewhere
4. **Avoid very long chains**: Break into logical segments if chains become too long
5. **Consider intermediate values**: Sometimes intermediate variables improve clarity

**Good chaining:**

```javascript
chain(data)
  .mean()
  .multiply(2)
  .add(offset)
  .done()
```

**Consider breaking up:**

```javascript
// If chain is very long, consider segments
const normalized = chain(data).subtract(mean).divide(std).done()
const scaled = chain(normalized).multiply(scale).add(offset).done()
```
