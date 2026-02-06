# Physics Expression Evaluator

Build a physics expression evaluator that can parse, simplify, and evaluate mathematical expressions with physical units, including symbolic differentiation for motion analysis.

## Capabilities

### Expression Simplification

- Simplifies algebraic expressions correctly [@test](../test/simplify.test.js)
- Simplifies expressions with multiple variables [@test](../test/simplify_multi_var.test.js)

### Unit-Aware Calculations

- Performs calculations with physical units [@test](../test/units_calculation.test.js)
- Converts between compatible units [@test](../test/units_conversion.test.js)
- Handles compound unit expressions [@test](../test/units_compound.test.js)

### Symbolic Derivatives

- Computes symbolic derivatives of polynomial expressions [@test](../test/derivative_basic.test.js)
- Computes derivatives and simplifies the result [@test](../test/derivative_simplified.test.js)

## Implementation

[@generates](./src/physics-evaluator.js)

## API

```javascript { #api }
/**
 * Simplifies a mathematical expression algebraically.
 *
 * @param {string} expression - The mathematical expression to simplify (e.g., "2*x + 3*x")
 * @returns {string} The simplified expression as a string
 */
function simplifyExpression(expression) {
  // IMPLEMENTATION HERE
}

/**
 * Evaluates an expression with units and optionally converts to target units.
 *
 * @param {string} expression - Expression with units (e.g., "100 km / 2 hours")
 * @param {string} [targetUnit] - Optional target unit for conversion (e.g., "m/s")
 * @returns {string} Result formatted as "value unit"
 */
function evaluateWithUnits(expression, targetUnit = null) {
  // IMPLEMENTATION HERE
}

/**
 * Computes the symbolic derivative of an expression and optionally simplifies it.
 *
 * @param {string} expression - Expression to differentiate (e.g., "3*t^2 + 2*t")
 * @param {string} variable - Variable to differentiate with respect to (e.g., "t")
 * @param {boolean} [simplify] - Whether to simplify the result (default: false)
 * @returns {string} The derivative expression as a string
 */
function computeDerivative(expression, variable, simplify = false) {
  // IMPLEMENTATION HERE
}

module.exports = {
  simplifyExpression,
  evaluateWithUnits,
  computeDerivative
};
```

## Dependencies { .dependencies }

### mathjs { .dependency }

Provides mathematical computation, symbolic algebra, and unit handling capabilities.
