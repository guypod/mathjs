# Formula Evaluator with Variables

Build a formula evaluator that parses mathematical expressions from strings and evaluates them with variable substitution.

## Requirements

Create a function `evaluateFormula(expression, variables)` that:

- Takes a mathematical expression as a string (e.g., "2 * x + y^2")
- Takes an object of variable values (e.g., `{x: 5, y: 3}`)
- Parses the expression and evaluates it with the provided variables
- Returns the numerical result

The expression should support:
- Basic arithmetic operators: +, -, *, /, ^
- Variable substitution
- Function calls like sqrt(), sin(), cos()
- Proper operator precedence

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing expression parsing and evaluation with variable scopes.

## Test Cases

- `evaluateFormula("2 * x + 3", {x: 5})` returns 13 [@test](./test-1.js)
- `evaluateFormula("sqrt(a^2 + b^2)", {a: 3, b: 4})` returns 5 [@test](./test-2.js)
- `evaluateFormula("sin(angle) + cos(angle)", {angle: 0})` returns 1 [@test](./test-3.js)
