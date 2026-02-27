# Math Expression Evaluator

Build a module that evaluates mathematical expressions provided as strings and returns their computed results.

## Capabilities

### Evaluate expressions with variables

Implement a function `evaluateExpr(expression, variables)` that takes:
- `expression`: a string containing a mathematical expression (e.g. `"2 * x + y"`)
- `variables`: an optional plain object mapping variable names to numeric values

The function must return the computed numeric result of the expression with the given variables substituted in.

The function must:
- Support standard arithmetic operators (`+`, `-`, `*`, `/`, `^`)
- Support parentheses for grouping
- Support common mathematical functions such as square root, absolute value, logarithm, sine, and cosine when written in expression syntax
- Substitute variables from the `variables` object into the expression before evaluation
- Return the numeric result of the expression

- Given `"12 / (2.3 + 0.7)"` with no variables, it returns `4` [@test](./test/basic.test.js)
- Given `"2 * x + 3"` with `{ x: 5 }`, it returns `13` [@test](./test/variables.test.js)
- Given `"x^2 + y^2"` with `{ x: 3, y: 4 }`, it returns `25` [@test](./test/multivar.test.js)
- Given `"sin(45 deg) ^ 2"` with no variables, it returns approximately `0.5` [@test](./test/trig.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Evaluates a mathematical expression string.
 *
 * @param {string} expression - A mathematical expression, e.g. "2 * x + y"
 * @param {Object} [variables] - Optional map of variable names to values
 * @returns {number} The result of evaluating the expression
 */
export function evaluateExpr(expression, variables) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library providing expression parsing and evaluation with support for variables, units, complex numbers, and many built-in functions.

[@satisfied-by](mathjs)
