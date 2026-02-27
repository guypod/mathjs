# Custom Math Extension

Build a module that creates a custom math instance extended with domain-specific functions using the library's extensibility API.

## Capabilities

### Create a custom math instance with additional functions

Implement a function `createCustomMath()` that creates and returns a custom math instance with the following additional functions registered on it:

- **`clamp(value, min, max)`** — Returns `value` clamped to the range `[min, max]`. Uses the library's `max` and `min` functions internally.
- **`degToRad(degrees)`** — Converts degrees to radians. Returns `degrees * pi / 180` using the library's `pi` constant.
- **`average(...values)`** — Computes the arithmetic mean of any number of numeric arguments. Uses the library's `mean` function internally.

The custom instance must:
- Be a valid math instance with all standard functions still available
- Have the three new functions accessible as `customMath.clamp(...)`, `customMath.degToRad(...)`, and `customMath.average(...)`
- Create functions that internally use the math instance's own functions (not standalone imports)

- `createCustomMath().clamp(15, 0, 10)` returns `10` [@test](./test/clamp_above.test.js)
- `createCustomMath().clamp(5, 0, 10)` returns `5` [@test](./test/clamp_within.test.js)
- `createCustomMath().degToRad(180)` returns approximately `3.14159` (pi) [@test](./test/deg_to_rad.test.js)
- `createCustomMath().average(1, 2, 3, 4, 5)` returns `3` [@test](./test/average.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Creates a custom math instance with additional domain-specific functions.
 *
 * @returns {MathJsInstance} A math instance with clamp, degToRad, and average functions added
 */
export function createCustomMath() {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library providing create() for custom instance creation and import() for registering new functions on a math instance.

[@satisfied-by](mathjs)
