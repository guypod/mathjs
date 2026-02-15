# Configuration

Math.js provides a configuration system to customize behavior including number types, precision, tolerances, and output formatting. Configuration can be set globally or per math instance.

## Core Imports

```javascript { .api }
import { config } from 'mathjs';
```

## Configuration Function

### config

Gets or sets configuration options for the math instance.

```typescript { .api }
function config(options: ConfigOptions): ConfigOptions;
function config(): ConfigOptions;
```

**Parameters:**
- `options`: Configuration options object (optional)

**Returns:** Current configuration (all options if no argument, or updated configuration)

**Usage Examples:**

```javascript
import { config, evaluate } from 'mathjs';

// Get current configuration
const currentConfig = config();

// Set configuration options
config({
  number: 'BigNumber',
  precision: 128
});

// Now all operations use BigNumber with 128 digits precision
evaluate('0.1 + 0.2');            // BigNumber 0.3 (exact)

// Configure matrix type
config({
  matrix: 'Array'
});

// Now matrix operations return arrays instead of Matrix objects
evaluate('[[1, 2], [3, 4]]');     // Array [[1, 2], [3, 4]]

// Set comparison tolerances
config({
  relTol: 1e-10,
  absTol: 1e-10
});

// Configure random seed for reproducibility
config({
  randomSeed: 'my-seed-12345'
});
```

## Configuration Options

```typescript { .api }
interface ConfigOptions {
  // Number type for calculations
  number?: 'number' | 'BigNumber' | 'Fraction';

  // Precision for BigNumber (number of significant digits)
  precision?: number;

  // Relative tolerance for equality comparisons
  relTol?: number;

  // Absolute tolerance for equality comparisons
  absTol?: number;

  // Default matrix type
  matrix?: 'Matrix' | 'Array';

  // Parenthesis handling in expressions
  parenthesis?: 'keep' | 'auto' | 'all';

  // Random seed for reproducible randomness
  randomSeed?: string | number | null;
}
```

## Configuration Properties

### number

Sets the default number type for calculations.

**Values:**
- `'number'`: Standard JavaScript numbers (default, fast, ~15 digit precision)
- `'BigNumber'`: Arbitrary precision decimal numbers (slower, configurable precision)
- `'Fraction'`: Exact rational arithmetic (fractions, exact for rationals)

**Usage Examples:**

```javascript
import { config, evaluate } from 'mathjs';

// Default: number
config({number: 'number'});
evaluate('0.1 + 0.2');            // 0.30000000000000004 (floating point)

// Use BigNumber for exact decimals
config({number: 'BigNumber'});
evaluate('0.1 + 0.2');            // BigNumber 0.3 (exact)

// Use Fraction for exact rational arithmetic
config({number: 'Fraction'});
evaluate('1/3 + 1/6');            // Fraction 1/2 (exact)
```

### precision

Sets the precision (number of significant digits) for BigNumber calculations.

**Type:** `number`
**Default:** 64
**Range:** 1 to thousands of digits

**Usage Examples:**

```javascript
import { config, evaluate, pi } from 'mathjs';

// Default precision (64 digits)
config({
  number: 'BigNumber',
  precision: 64
});
evaluate('pi');
// BigNumber 3.141592653589793238462643383279502884197169399375105820974944592

// Higher precision (128 digits)
config({
  number: 'BigNumber',
  precision: 128
});
evaluate('pi');
// BigNumber 3.141592653589793238462643383279502884197...
// (128 significant digits)

// Lower precision for performance
config({
  number: 'BigNumber',
  precision: 10
});
evaluate('pi');
// BigNumber 3.141592654
```

### relTol and absTol

Set relative and absolute tolerances for equality comparisons.

**Type:** `number`
**Defaults:**
- `relTol`: 1e-12 (relative tolerance)
- `absTol`: 1e-15 (absolute tolerance)

**Usage:**
Two numbers x and y are considered equal if:
|x - y| <= max(relTol × max(|x|, |y|), absTol)

**Usage Examples:**

```javascript
import { config, equal } from 'mathjs';

// Default tolerance
equal(1, 1.0000000000001);        // true (within tolerance)

// Stricter tolerance
config({
  relTol: 1e-15,
  absTol: 1e-20
});
equal(1, 1.0000000000001);        // false (exceeds tolerance)

// Looser tolerance
config({
  relTol: 1e-6,
  absTol: 1e-8
});
equal(1, 1.000001);               // true (within tolerance)

// Useful for comparing floating point results
config({relTol: 1e-10, absTol: 1e-12});
equal(0.1 + 0.2, 0.3);            // true (compensates for rounding)
```

### matrix

Sets the default matrix type returned by matrix operations.

**Values:**
- `'Matrix'`: Returns Matrix objects (default, DenseMatrix or SparseMatrix)
- `'Array'`: Returns plain JavaScript arrays

**Usage Examples:**

```javascript
import { config, evaluate, typeOf } from 'mathjs';

// Default: Matrix
config({matrix: 'Matrix'});
const m1 = evaluate('[[1, 2], [3, 4]]');
typeOf(m1);                       // 'DenseMatrix'

// Use arrays
config({matrix: 'Array'});
const m2 = evaluate('[[1, 2], [3, 4]]');
typeOf(m2);                       // 'Array'

// Affects matrix operations
config({matrix: 'Array'});
evaluate('eye(3)');
// [[1, 0, 0], [0, 1, 0], [0, 0, 1]] (plain array)

config({matrix: 'Matrix'});
evaluate('eye(3)');
// DenseMatrix [[1, 0, 0], [0, 1, 0], [0, 0, 1]]
```

### parenthesis

Controls how parentheses are handled in expression formatting.

**Values:**
- `'keep'`: Keep all parentheses as in input
- `'auto'`: Automatically add/remove based on operator precedence (default)
- `'all'`: Keep all parentheses for clarity

**Usage Examples:**

```javascript
import { config, parse } from 'mathjs';

// Auto (default): minimal parentheses
config({parenthesis: 'auto'});
parse('(2 + 3) * 4').toString();  // '(2 + 3) * 4'
parse('2 + (3 * 4)').toString();  // '2 + 3 * 4' (unnecessary parens removed)

// Keep: preserve original
config({parenthesis: 'keep'});
parse('2 + (3 * 4)').toString();  // '2 + (3 * 4)' (kept)

// All: maximum clarity
config({parenthesis: 'all'});
parse('2 + 3 * 4').toString();    // '2 + (3 * 4)' (added for clarity)
```

### randomSeed

Sets a seed for pseudo-random number generation to make random functions reproducible.

**Type:** `string | number | null`
**Default:** `null` (non-reproducible randomness)

**Usage Examples:**

```javascript
import { config, random } from 'mathjs';

// Non-reproducible (default)
config({randomSeed: null});
random();                         // 0.7234... (different each time)
random();                         // 0.1892...

// Reproducible with seed
config({randomSeed: 'my-seed'});
random();                         // 0.5123... (same each time with this seed)
random();                         // 0.8765...

// Reset to same seed = same sequence
config({randomSeed: 'my-seed'});
random();                         // 0.5123... (same as before)
random();                         // 0.8765...

// Numeric seeds also work
config({randomSeed: 12345});
random();                         // 0.4123... (reproducible)

// Useful for testing and reproducible simulations
config({randomSeed: 'test-seed-2024'});
const results = Array.from({length: 10}, () => random());
// Same results every run with same seed
```

## Creating Custom Math Instances

For multiple configurations, create separate math instances instead of modifying global config:

```javascript
import { create, all } from 'mathjs';

// Create instance with BigNumber default
const mathBig = create(all, {
  number: 'BigNumber',
  precision: 128
});

// Create instance with Fraction default
const mathFrac = create(all, {
  number: 'Fraction'
});

// Use independently
mathBig.evaluate('0.1 + 0.2');    // BigNumber 0.3
mathFrac.evaluate('1/3 + 1/6');   // Fraction 1/2
```

## Performance Considerations

- **number**: Fastest, but limited precision (~15 digits)
- **BigNumber**: Slower, but arbitrary precision (configurable)
- **Fraction**: Exact for rationals, but slower and can overflow with large numerators/denominators
- **Matrix vs Array**: Matrix objects have more features but slightly more overhead

Choose based on your needs:
- **Speed**: Use `number` and `Array`
- **Precision**: Use `BigNumber` with appropriate `precision`
- **Exact rationals**: Use `Fraction`
- **Mixed**: Create separate instances for different use cases
