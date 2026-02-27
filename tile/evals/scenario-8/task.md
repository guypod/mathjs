# Symbolic Derivative Solver

Build a module that computes symbolic derivatives of polynomial and trigonometric expressions and evaluates them at specific points.

## Capabilities

### Symbolically differentiate mathematical expressions

Implement the following functions:

**`differentiate(expression, variable)`** — Computes the symbolic derivative of `expression` (a string) with respect to `variable` (a string). Returns the derivative as a **string** representation of the simplified result.

**`evaluateDerivative(expression, variable, point)`** — Computes the derivative of `expression` with respect to `variable` and evaluates it at `variable = point`. Returns the numeric result as a plain JavaScript `number`.

The functions must compute the derivative symbolically (not numerically) using the library's symbolic differentiation capability.

- `differentiate("x^2 + x", "x")` returns `"2 * x + 1"` or an equivalent simplified form [@test](./test/polynomial.test.js)
- `differentiate("sin(x)", "x")` returns `"cos(x)"` or equivalent [@test](./test/trig_deriv.test.js)
- `evaluateDerivative("x^3", "x", 2)` returns `12` (since d/dx(x^3) = 3x^2, and 3*(2^2) = 12) [@test](./test/eval_cubic.test.js)
- `evaluateDerivative("x^2 + 3*x + 2", "x", 0)` returns `3` (since d/dx = 2x + 3, evaluated at x=0) [@test](./test/eval_quadratic.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Computes the symbolic derivative of an expression.
 *
 * @param {string} expression - A mathematical expression string, e.g. "x^2 + x"
 * @param {string} variable - The variable to differentiate with respect to
 * @returns {string} String representation of the derivative
 */
export function differentiate(expression, variable) {}

/**
 * Computes the derivative and evaluates it at a specific point.
 *
 * @param {string} expression - A mathematical expression string
 * @param {string} variable - The variable to differentiate with respect to
 * @param {number} point - The value to substitute for the variable
 * @returns {number} The numeric value of the derivative at the given point
 */
export function evaluateDerivative(expression, variable, point) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library providing symbolic differentiation and expression simplification, returning expression trees that can be converted to strings or evaluated.

[@satisfied-by](mathjs)
