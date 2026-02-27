# Chained Calculation Pipeline

Build a module that computes a result by applying a sequence of mathematical operations using a fluent chaining API.

## Capabilities

### Chain multiple operations on a starting value

Implement a function `computePipeline(start, operations)` that takes:
- `start`: a number to start the computation from
- `operations`: an array of operation descriptors, each with a `op` string and optional `args` array

Supported operations:
- `"add"` — add `args[0]` to the current value
- `"subtract"` — subtract `args[0]` from the current value
- `"multiply"` — multiply the current value by `args[0]`
- `"divide"` — divide the current value by `args[0]`
- `"pow"` — raise the current value to the power of `args[0]`
- `"sqrt"` — take the square root of the current value (no args)
- `"abs"` — take the absolute value (no args)

The function must:
- Use a single fluent chain to apply all operations sequentially
- Return the final numeric result after all operations are applied

- `computePipeline(3, [{ op: "add", args: [4] }, { op: "multiply", args: [2] }])` returns `14` [@test](./test/basic_chain.test.js)
- `computePipeline(16, [{ op: "sqrt" }, { op: "add", args: [2] }])` returns `6` [@test](./test/sqrt_chain.test.js)
- `computePipeline(-9, [{ op: "abs" }, { op: "pow", args: [2] }])` returns `81` [@test](./test/abs_pow_chain.test.js)
- `computePipeline(100, [{ op: "divide", args: [4] }, { op: "subtract", args: [5] }])` returns `20` [@test](./test/divide_subtract_chain.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Applies a sequence of math operations to a starting value using chaining.
 *
 * @param {number} start - The initial value
 * @param {Array<{op: string, args?: number[]}>} operations - Operations to apply
 * @returns {number} The final result
 */
export function computePipeline(start, operations) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library providing a fluent chaining API where each operation returns a new chain wrapping the result, finalized by calling done().

[@satisfied-by](mathjs)
