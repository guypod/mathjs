# Complex Number Calculator

Build a module that performs arithmetic on complex numbers represented as `{ re, im }` plain objects.

## Capabilities

### Perform operations on complex numbers

Implement the following functions:

**`complexAdd(a, b)`** — Returns the sum of two complex numbers.

**`complexMultiply(a, b)`** — Returns the product of two complex numbers.

**`complexMagnitude(z)`** — Returns the magnitude (absolute value) of a complex number as a plain JavaScript `number`.

**`complexSqrt(x)`** — Returns the principal square root of a real number `x` as a `{ re, im }` object. For negative inputs, the result has a non-zero imaginary part.

Each function:
- Accepts plain `{ re, im }` objects (not library-specific instances)
- Returns results as plain `{ re, im }` objects (except `complexMagnitude` which returns a number)
- Uses the library's complex number type internally for computation

- `complexAdd({ re: 2, im: 3 }, { re: 1, im: -1 })` returns `{ re: 3, im: 2 }` [@test](./test/add.test.js)
- `complexMultiply({ re: 1, im: 1 }, { re: 1, im: -1 })` returns `{ re: 2, im: 0 }` [@test](./test/multiply.test.js)
- `complexMagnitude({ re: 3, im: 4 })` returns `5` [@test](./test/magnitude.test.js)
- `complexSqrt(-4)` returns an object with `re` approximately `0` and `im` approximately `2` [@test](./test/sqrt_negative.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * @param {{ re: number, im: number }} a
 * @param {{ re: number, im: number }} b
 * @returns {{ re: number, im: number }}
 */
export function complexAdd(a, b) {}

/**
 * @param {{ re: number, im: number }} a
 * @param {{ re: number, im: number }} b
 * @returns {{ re: number, im: number }}
 */
export function complexMultiply(a, b) {}

/**
 * @param {{ re: number, im: number }} z
 * @returns {number}
 */
export function complexMagnitude(z) {}

/**
 * @param {number} x - A real number (may be negative)
 * @returns {{ re: number, im: number }}
 */
export function complexSqrt(x) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library with native complex number support. Provides construction, arithmetic, and magnitude functions for complex numbers.

[@satisfied-by](mathjs)
