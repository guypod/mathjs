# Mathematical Expression Pipeline

Create a function that processes numerical values through a pipeline of mathematical operations using a fluent, chainable API.

## Requirements

Create a function `processPipeline(value, operations)` that:

- Takes an initial numerical value
- Takes an array of operation objects, each with `{op: string, arg: number}` where `op` is the operation name ('add', 'multiply', 'divide', 'sqrt', 'square')
- Applies operations sequentially using a chainable API
- Returns the final computed result

The implementation should use a fluent chaining pattern where operations are applied in sequence.

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing chainable operations for fluent computation pipelines.

## Test Cases

- `processPipeline(3, [{op: 'add', arg: 4}, {op: 'multiply', arg: 2}])` returns 14 [@test](./test-1.js)
- `processPipeline(16, [{op: 'sqrt'}, {op: 'add', arg: 6}])` returns 10 [@test](./test-2.js)
- `processPipeline(2, [{op: 'square'}, {op: 'add', arg: 1}, {op: 'square'}])` returns 25 [@test](./test-3.js)
