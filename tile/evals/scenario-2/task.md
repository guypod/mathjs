# High-Precision Arithmetic

Build a module that performs arithmetic calculations with configurable decimal precision, avoiding the limitations of standard floating-point numbers.

## Capabilities

### Perform arithmetic with configurable precision

Implement a function `preciseCalculate(expression, precision)` that:
- Takes `expression`: a mathematical expression string (e.g. `"1 / 3"`, `"sqrt(2)"`)
- Takes `precision`: a positive integer specifying the number of significant digits for the result
- Configures the math instance to use the given precision
- Evaluates the expression using arbitrary-precision arithmetic (not standard floating-point)
- Returns the result as a **string** representation of the high-precision value

The function must use high-precision number types (not JavaScript's native `number`) so that the result reflects the configured precision rather than standard 64-bit floating-point limits.

- `preciseCalculate("1 / 3", 10)` returns a string starting with `"0.3333333333"` (10 significant digits) [@test](./test/one_third.test.js)
- `preciseCalculate("sqrt(2)", 20)` returns a string starting with `"1.4142135623730950488"` [@test](./test/sqrt2.test.js)
- `preciseCalculate("0.1 + 0.2", 15)` returns `"0.3"` (exact result, no floating-point error) [@test](./test/float_error.test.js)
- `preciseCalculate("2 ^ 100", 32)` returns the exact integer `"1267650600228229401496703205376"` [@test](./test/bigpow.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Evaluates a mathematical expression with arbitrary precision.
 *
 * @param {string} expression - A mathematical expression to evaluate
 * @param {number} precision - Number of significant digits
 * @returns {string} String representation of the high-precision result
 */
export function preciseCalculate(expression, precision) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library supporting arbitrary-precision arithmetic via BigNumber. Precision can be configured and BigNumber types are used transparently in expression evaluation.

[@satisfied-by](mathjs)
