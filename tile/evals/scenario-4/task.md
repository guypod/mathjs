# Physical Unit Converter

Build a module that converts physical measurements between different units and performs unit-aware arithmetic.

## Capabilities

### Convert values between compatible physical units

Implement the following functions:

**`convertUnit(value, fromUnit, toUnit)`** — Converts a numeric `value` from `fromUnit` to `toUnit`. Returns the converted value as a plain JavaScript `number`. Throws if the units are incompatible.

**`addWithUnits(valueA, unitA, valueB, unitB)`** — Adds two measurements that may be in different but compatible units (e.g., `5 km` + `500 m`). Returns an object `{ value, unit }` where `unit` is the unit of the first operand and `value` is the sum expressed in that unit.

- `convertUnit(5, "km", "m")` returns `5000` [@test](./test/km_to_m.test.js)
- `convertUnit(100, "cm", "inch")` returns approximately `39.3701` [@test](./test/cm_to_inch.test.js)
- `convertUnit(0, "celsius", "fahrenheit")` returns `32` [@test](./test/celsius_to_fahrenheit.test.js)
- `addWithUnits(1, "km", 500, "m")` returns `{ value: 1.5, unit: "km" }` [@test](./test/add_units.test.js)

## Implementation

[@generates](./src/index.js)

## API

```javascript { #api }
/**
 * Converts a value from one unit to another.
 *
 * @param {number} value - The numeric value to convert
 * @param {string} fromUnit - The source unit (e.g. "km", "celsius")
 * @param {string} toUnit - The target unit (e.g. "m", "fahrenheit")
 * @returns {number} The converted value as a plain number
 */
export function convertUnit(value, fromUnit, toUnit) {}

/**
 * Adds two measurements with (possibly different) compatible units.
 *
 * @param {number} valueA - The numeric value of the first measurement
 * @param {string} unitA - The unit of the first measurement
 * @param {number} valueB - The numeric value of the second measurement
 * @param {string} unitB - The unit of the second measurement
 * @returns {{ value: number, unit: string }} Sum in the unit of the first operand
 */
export function addWithUnits(valueA, unitA, valueB, unitB) {}
```

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

An extensive math library with built-in support for physical units including SI and imperial units, unit conversion, and unit-aware arithmetic.

[@satisfied-by](mathjs)
