# Complex Number Calculator

Build a calculator for complex number operations supporting both rectangular and polar forms.

## Requirements

Create a function `complexCalc(operation, ...values)` that performs operations on complex numbers:

- `operation` can be: 'add', 'multiply', 'conjugate', 'magnitude', 'phase'
- `values` are complex numbers represented as strings (e.g., "3+4i") or objects
- For 'add' and 'multiply', take two complex numbers
- For 'conjugate', 'magnitude', and 'phase', take one complex number
- Return the result as appropriate (complex number for add/multiply/conjugate, real number for magnitude/phase)

The calculator should properly handle imaginary units and complex arithmetic.

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing complex number support with arithmetic and conversion operations.

## Test Cases

- `complexCalc('add', '2+3i', '1+4i')` returns complex number equivalent to 3+7i [@test](./test-1.js)
- `complexCalc('multiply', '2+i', '3+2i')` returns complex number equivalent to 4+7i [@test](./test-2.js)
- `complexCalc('magnitude', '3+4i')` returns 5 [@test](./test-3.js)
- `complexCalc('conjugate', '5-2i')` returns complex number equivalent to 5+2i [@test](./test-4.js)
