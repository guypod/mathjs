# Expression Tree Analyzer

Build an expression analyzer that examines and manipulates the abstract syntax tree of mathematical expressions.

## Requirements

Create a function `analyzeExpression(expression)` that:

- Takes a mathematical expression as a string
- Parses it into an abstract syntax tree (AST)
- Analyzes the tree to extract information:
  - Count the total number of operator nodes (like +, -, *, /)
  - Find all variable names used
  - Count the total number of function calls
- Returns an object with: `{operators: number, variables: string[], functions: number}`

The analyzer should traverse the expression tree and identify different node types.

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing expression parsing and AST manipulation capabilities.

## Test Cases

- `analyzeExpression('2 * x + 3')` returns `{operators: 2, variables: ['x'], functions: 0}` [@test](./test-1.js)
- `analyzeExpression('sin(a) + cos(b)')` returns `{operators: 1, variables: ['a','b'], functions: 2}` [@test](./test-2.js)
- `analyzeExpression('(x + y) * (z - 1)')` returns `{operators: 3, variables: ['x','y','z'], functions: 0}` [@test](./test-3.js)
