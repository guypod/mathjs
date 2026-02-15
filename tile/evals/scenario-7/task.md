# Symbolic Derivative Calculator

Build a symbolic derivative calculator that computes derivatives of mathematical expressions.

## Requirements

Create a function `calculateDerivative(expression, variable)` that:

- Takes a mathematical expression as a string (e.g., "x^2 + 3*x + 1")
- Takes the variable to differentiate with respect to (e.g., "x")
- Computes the symbolic derivative
- Returns the derivative as a simplified string expression

The calculator should:
- Apply differentiation rules (power rule, chain rule, product rule)
- Simplify the resulting expression
- Handle polynomial, trigonometric, and exponential functions

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing symbolic differentiation and algebraic simplification.

## Test Cases

- `calculateDerivative('x^2 + 3*x + 1', 'x')` returns '2*x + 3' or equivalent [@test](./test-1.js)
- `calculateDerivative('sin(x) * x', 'x')` returns expression equivalent to 'cos(x)*x + sin(x)' [@test](./test-2.js)
- `calculateDerivative('exp(2*x)', 'x')` returns expression equivalent to '2*exp(2*x)' [@test](./test-3.js)
