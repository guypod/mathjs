# Physical Units Converter

Create a physical units converter that performs arithmetic with automatic unit tracking and conversions.

## Requirements

Create two functions:

1. `convertUnit(value, fromUnit, toUnit)` - Converts a value from one unit to another (e.g., "5 cm" to "inch")

2. `calculateWithUnits(expr)` - Evaluates arithmetic expressions with units and returns the result with appropriate units. Expressions like "10 kg + 5000 g" should automatically handle unit conversion.

Both functions should support:
- Length units (m, cm, mm, km, inch, foot, mile)
- Mass units (kg, g, lb, oz)
- Time units (s, min, hour, day)
- Automatic conversion when units are compatible

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing physical unit support with automatic tracking and conversion.

## Test Cases

- `convertUnit(5, 'cm', 'inch')` returns approximately 1.968 inches [@test](./test-1.js)
- `calculateWithUnits('10 kg + 5000 g')` returns 15 kg or 15000 g [@test](./test-2.js)
- `calculateWithUnits('100 km / 2 hour')` returns 50 km/hour [@test](./test-3.js)
