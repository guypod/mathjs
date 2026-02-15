# Factory and Dependency Injection

Math.js uses a factory pattern with dependency injection to allow creating custom math instances with selected functionality. This enables tree-shaking for smaller bundle sizes and customization of available functions.

## Core Imports

```javascript { .api }
import { create, factory, typed, all } from 'mathjs';
```

## Core Factory Functions

### create

Creates a custom math.js instance with selected factories and configuration.

```typescript { .api }
function create(factories: FactoryDef | FactoryDef[], config?: ConfigOptions): MathJsInstance;
```

**Parameters:**
- `factories`: Factory definition(s) to include (single factory, array, or `all` for everything)
- `config`: Optional configuration options

**Returns:** Math.js instance with selected functionality

**Usage Examples:**

```javascript
import { create, all } from 'mathjs';

// Create full instance with all functions
const math = create(all);

// Create with custom config
const mathBig = create(all, {
  number: 'BigNumber',
  precision: 128
});

// Create minimal instance with selected functions
import {
  addDependencies,
  subtractDependencies,
  multiplyDependencies,
  divideDependencies
} from 'mathjs';

const mathBasic = create({
  addDependencies,
  subtractDependencies,
  multiplyDependencies,
  divideDependencies
});

mathBasic.add(2, 3);              // 5
mathBasic.subtract(5, 2);         // 3
// mathBasic.sqrt(4);             // Error: sqrt not available

// Create instance for specific domain (e.g., matrix operations)
import {
  matrixDependencies,
  multiplyDependencies,
  invDependencies,
  detDependencies
} from 'mathjs';

const mathMatrix = create({
  matrixDependencies,
  multiplyDependencies,
  invDependencies,
  detDependencies
});

const A = mathMatrix.matrix([[1, 2], [3, 4]]);
const detA = mathMatrix.det(A);   // -2
```

### factory

Creates a factory function that can be used with dependency injection.

```typescript { .api }
function factory(
  name: string,
  dependencies: string[],
  create: (dependencies: Record<string, any>) => any,
  meta?: { lazy?: boolean }
): FactoryFunction;
```

**Parameters:**
- `name`: Name of the function/factory
- `dependencies`: Array of dependency names (strings)
- `create`: Factory function that receives dependencies and returns implementation
- `meta`: Optional metadata (e.g., `{ lazy: true }` for lazy evaluation)

**Returns:** Factory function for use with `create()`

**Usage Examples:**

```javascript
import { create, factory, all } from 'mathjs';

// Define custom function with dependencies
const customAddTenDependencies = factory(
  'customAddTen',
  ['add', 'typed'],
  ({ add, typed }) => {
    return typed('customAddTen', {
      'number': (x) => add(x, 10),
      'Array': (arr) => arr.map(x => add(x, 10))
    });
  }
);

// Create instance with custom function
const math = create({
  ...all,
  customAddTenDependencies
});

math.customAddTen(5);             // 15
math.customAddTen([1, 2, 3]);     // [11, 12, 13]

// Custom function with multiple dependencies
const averageDependencies = factory(
  'average',
  ['add', 'divide', 'typed'],
  ({ add, divide, typed }) => {
    return typed('average', {
      'Array': (arr) => {
        const sum = arr.reduce((a, b) => add(a, b), 0);
        return divide(sum, arr.length);
      }
    });
  }
);

const mathAvg = create({
  ...all,
  averageDependencies
});

mathAvg.average([1, 2, 3, 4, 5]); // 3

// Lazy evaluation factory
const lazyFunctionDependencies = factory(
  'lazyFunction',
  ['typed'],
  ({ typed }) => {
    return typed('lazyFunction', {
      'any': (x) => x
    });
  },
  { lazy: true }  // Enable lazy evaluation
);
```

### typed

Creates a typed function with multiple signatures (function overloading).

```typescript { .api }
function typed(name: string, signatures: Record<string, Function>): TypedFunction;
function typed(signatures: Record<string, Function>): TypedFunction;
```

**Parameters:**
- `name`: Function name (optional)
- `signatures`: Object mapping type signatures to implementations

**Returns:** Typed function that dispatches to correct implementation based on argument types

**Usage Examples:**

```javascript
import { typed } from 'mathjs';

// Create typed function with multiple signatures
const myAdd = typed('myAdd', {
  'number, number': (a, b) => a + b,
  'string, string': (a, b) => a + ' ' + b,
  'Array, Array': (a, b) => a.concat(b)
});

myAdd(2, 3);                      // 5
myAdd('hello', 'world');          // 'hello world'
myAdd([1, 2], [3, 4]);            // [1, 2, 3, 4]

// Type conversion
const convert = typed('convert', {
  'number': (x) => x.toString(),
  'string': (x) => parseFloat(x),
  'boolean': (x) => x ? 1 : 0
});

convert(123);                     // '123'
convert('45.6');                  // 45.6
convert(true);                    // 1

// Optional parameters using null
const greet = typed('greet', {
  'string': (name) => `Hello, ${name}!`,
  'string, string': (name, greeting) => `${greeting}, ${name}!`
});

greet('Alice');                   // 'Hello, Alice!'
greet('Bob', 'Hi');               // 'Hi, Bob!'

// Any type
const identity = typed('identity', {
  'any': (x) => x
});

identity(42);                     // 42
identity('hello');                // 'hello'
identity([1, 2, 3]);              // [1, 2, 3]

// Complex signatures
const process = typed('process', {
  'number': (x) => x * 2,
  'number, number': (x, y) => x + y,
  'Array': (arr) => arr.map(x => x * 2),
  'Array, number': (arr, factor) => arr.map(x => x * factor)
});

process(5);                       // 10
process(3, 4);                    // 7
process([1, 2, 3]);               // [2, 4, 6]
process([1, 2, 3], 10);           // [10, 20, 30]
```

## Available Type Signatures

Common type strings for `typed` functions:

**Basic Types:**
- `'number'` - JavaScript number
- `'string'` - JavaScript string
- `'boolean'` - JavaScript boolean
- `'null'` - null
- `'undefined'` - undefined
- `'any'` - Any type

**Math.js Types:**
- `'BigNumber'` - Arbitrary precision number
- `'Fraction'` - Rational number
- `'Complex'` - Complex number
- `'Unit'` - Physical unit with value
- `'Matrix'` - Matrix (DenseMatrix or SparseMatrix)
- `'DenseMatrix'` - Dense matrix
- `'SparseMatrix'` - Sparse matrix
- `'Array'` - JavaScript array
- `'Range'` - Range object
- `'Index'` - Multi-dimensional index

**Variadic Parameters:**
- `'...number'` - Variable number of numbers
- `'...any'` - Variable number of any type

## Dependency Injection Examples

```javascript
import { create } from 'mathjs';

// Import only needed dependencies
import {
  addDependencies,
  subtractDependencies,
  multiplyDependencies,
  divideDependencies,
  sqrtDependencies,
  powDependencies,
  typedDependencies,
  matrixDependencies
} from 'mathjs';

// Create minimal bundle
const miniMath = create({
  addDependencies,
  subtractDependencies,
  multiplyDependencies,
  divideDependencies,
  sqrtDependencies,
  powDependencies,
  typedDependencies,
  matrixDependencies
});

// Only included functions are available
miniMath.add(2, 3);               // 5
miniMath.sqrt(16);                // 4
// miniMath.sin(0);               // Error: sin not included

// Create specialized instance for statistics
import {
  addDependencies,
  divideDependencies,
  sqrtDependencies,
  meanDependencies,
  medianDependencies,
  stdDependencies,
  varianceDependencies
} from 'mathjs';

const statsMath = create({
  addDependencies,
  divideDependencies,
  sqrtDependencies,
  meanDependencies,
  medianDependencies,
  stdDependencies,
  varianceDependencies
});

statsMath.mean([1, 2, 3, 4, 5]);  // 3
statsMath.std([1, 2, 3, 4, 5]);   // 1.58...
```

## Tree-Shaking for Smaller Bundles

```javascript
// Instead of importing everything (large bundle):
import * as math from 'mathjs';

// Import only what you need (small bundle):
import { create, addDependencies, multiplyDependencies } from 'mathjs';

const math = create({
  addDependencies,
  multiplyDependencies
});

// Bundle only includes add, multiply, and their dependencies
// Significantly smaller bundle size for browser applications
```

## Dynamic Function Import

### import

Dynamically add custom functions to an existing math.js instance without modifying the core library.

```typescript { .api }
/**
 * Import custom functions into the math instance
 * @param object - Object with function definitions or factory definitions
 * @param options - Import options (optional)
 * @returns void
 */
function import(
  object: Record<string, Function | FactoryDef>,
  options?: ImportOptions
): void;
```

**Parameters:**
- `object`: Object containing functions or factory definitions to import
- `options`: Optional configuration
  - `override`: If true, allow overriding existing functions (default: false)
  - `silent`: If true, suppress warnings (default: false)
  - `wrap`: If true, wrap functions with typed-function (default: false)

**Usage Examples:**

```javascript
import { create, all } from 'mathjs';

const math = create(all);

// Import simple custom function
math.import({
  hello: function(name) {
    return 'Hello, ' + name + '!';
  }
});

math.hello('World');              // 'Hello, World!'

// Import custom function using math.js functions
math.import({
  triple: function(x) {
    return math.multiply(x, 3);
  }
});

math.triple(5);                   // 15

// Import function with typed-function for multiple signatures
math.import({
  addOne: math.typed('addOne', {
    'number': function(x) {
      return x + 1;
    },
    'BigNumber': function(x) {
      return math.add(x, math.bignumber(1));
    }
  })
});

math.addOne(5);                   // 6
math.addOne(math.bignumber(10));  // BigNumber 11

// Import with override option
math.import({
  add: function(x, y) {
    console.log('Custom add called');
    return x + y + 1;
  }
}, { override: true });

math.add(2, 3);                   // 6 (custom implementation)

// Import factory function with dependencies
math.import({
  hypotenuse: math.factory('hypotenuse', ['sqrt', 'square', 'add'], function(deps) {
    return function(a, b) {
      return deps.sqrt(deps.add(deps.square(a), deps.square(b)));
    };
  })
});

math.hypotenuse(3, 4);            // 5

// Import multiple functions at once
math.import({
  double: x => x * 2,
  quadruple: x => x * 4,
  half: x => x / 2
});

math.double(10);                  // 20
math.quadruple(5);                // 20
math.half(10);                    // 5

// Import custom constants
math.import({
  answer: 42,
  goldenRatio: 1.618033988749895
});

math.answer;                      // 42
math.goldenRatio;                 // 1.618...
```

**Advanced Usage - Plugin Pattern:**

```javascript
import { create, all } from 'mathjs';

// Create reusable plugin
const statisticsPlugin = {
  // Root mean square
  rms: function(values) {
    return math.sqrt(math.mean(math.map(values, x => x * x)));
  },

  // Coefficient of variation
  coefficientOfVariation: function(values) {
    return math.divide(math.std(values), math.mean(values));
  },

  // Z-score normalization
  zscore: function(values) {
    const mu = math.mean(values);
    const sigma = math.std(values);
    return math.map(values, x => (x - mu) / sigma);
  }
};

const math = create(all);
math.import(statisticsPlugin);

const data = [1, 2, 3, 4, 5];
math.rms(data);                   // 3.31...
math.coefficientOfVariation(data); // 0.52...
math.zscore(data);                // [-1.41, -0.71, 0, 0.71, 1.41]
```

**Important Notes:**
- Imported functions have access to the math instance via closure
- Use `override: true` to replace existing functions (use with caution)
- Imported functions don't automatically use typed-function unless wrapped
- For best performance, use factory pattern for functions with dependencies

## Types

```typescript { .api }
type FactoryDef = Record<string, any>;

interface MathJsInstance {
  [key: string]: any;
  config: (options?: ConfigOptions) => ConfigOptions;
  import: (factories: FactoryDef | FactoryDef[]) => void;
}

interface FactoryFunction {
  fn: string;
  dependencies: string[];
  factory: (dependencies: Record<string, any>) => any;
  meta?: { lazy?: boolean };
}

interface TypedFunction {
  (...args: any[]): any;
  signatures: Record<string, Function>;
}

interface ConfigOptions {
  number?: 'number' | 'BigNumber' | 'Fraction';
  precision?: number;
  relTol?: number;
  absTol?: number;
  matrix?: 'Matrix' | 'Array';
  parenthesis?: 'keep' | 'auto' | 'all';
  randomSeed?: string | number | null;
}
```

## All Available Dependencies

Math.js exports `*Dependencies` objects for all 250+ functions. Examples:

- Arithmetic: `addDependencies`, `subtractDependencies`, `multiplyDependencies`, etc.
- Trigonometry: `sinDependencies`, `cosDependencies`, `tanDependencies`, etc.
- Matrix: `matrixDependencies`, `detDependencies`, `invDependencies`, etc.
- Statistics: `meanDependencies`, `stdDependencies`, `sumDependencies`, etc.
- And many more...

Use `all` to import everything, or individual dependencies for custom builds.
