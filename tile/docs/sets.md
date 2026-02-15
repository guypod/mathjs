# Set Operations

Math.js provides 10 set theory functions for working with mathematical sets represented as arrays. These operations include union, intersection, difference, cartesian product, and various set properties.

## Core Imports

```javascript { .api }
import {
  setUnion, setIntersect, setDifference, setSymDifference,
  setCartesian, setDistinct, setIsSubset, setMultiplicity,
  setPowerset, setSize
} from 'mathjs';
```

## Basic Set Operations

### setUnion

Returns the union of two sets (all elements from both sets, no duplicates).

```typescript { .api }
function setUnion(a: Array | Matrix, b: Array | Matrix): Array | Matrix;
```

**Parameters:**
- `a`: First set (array of values)
- `b`: Second set (array of values)

**Returns:** Union of sets a and b (a ∪ b)

**Usage Examples:**

```javascript
import { setUnion } from 'mathjs';

setUnion([1, 2, 3], [3, 4, 5]);
// [1, 2, 3, 4, 5]

setUnion([1, 1, 2], [2, 2, 3]);
// [1, 2, 3] (duplicates removed)

// Works with any comparable values
setUnion(['a', 'b'], ['b', 'c']);
// ['a', 'b', 'c']
```

### setIntersect

Returns the intersection of two sets (elements present in both sets).

```typescript { .api }
function setIntersect(a: Array | Matrix, b: Array | Matrix): Array | Matrix;
```

**Parameters:**
- `a`: First set (array of values)
- `b`: Second set (array of values)

**Returns:** Intersection of sets a and b (a ∩ b)

**Usage Examples:**

```javascript
import { setIntersect } from 'mathjs';

setIntersect([1, 2, 3], [2, 3, 4]);
// [2, 3]

setIntersect([1, 2, 3], [4, 5, 6]);
// [] (empty set, no common elements)

setIntersect(['a', 'b', 'c'], ['b', 'c', 'd']);
// ['b', 'c']
```

### setDifference

Returns the difference of two sets (elements in first set but not in second).

```typescript { .api }
function setDifference(a: Array | Matrix, b: Array | Matrix): Array | Matrix;
```

**Parameters:**
- `a`: First set (array of values)
- `b`: Second set (array of values)

**Returns:** Set difference (a - b or a \ b)

**Usage Examples:**

```javascript
import { setDifference } from 'mathjs';

setDifference([1, 2, 3, 4], [2, 4]);
// [1, 3]

setDifference([1, 2, 3], [3, 4, 5]);
// [1, 2]

// Order matters!
setDifference([3, 4, 5], [1, 2, 3]);
// [4, 5]

setDifference(['a', 'b', 'c'], ['b']);
// ['a', 'c']
```

### setSymDifference

Returns the symmetric difference of two sets (elements in either set but not in both).

```typescript { .api }
function setSymDifference(a: Array | Matrix, b: Array | Matrix): Array | Matrix;
```

**Parameters:**
- `a`: First set (array of values)
- `b`: Second set (array of values)

**Returns:** Symmetric difference (a △ b)

**Usage Examples:**

```javascript
import { setSymDifference } from 'mathjs';

setSymDifference([1, 2, 3], [3, 4, 5]);
// [1, 2, 4, 5]

setSymDifference([1, 2], [3, 4]);
// [1, 2, 3, 4] (no common elements)

setSymDifference([1, 2, 3], [1, 2, 3]);
// [] (identical sets)

setSymDifference(['a', 'b'], ['b', 'c']);
// ['a', 'c']
```

## Advanced Set Operations

### setCartesian

Returns the Cartesian product of two sets (all ordered pairs).

```typescript { .api }
function setCartesian(a: Array | Matrix, b: Array | Matrix): Array | Matrix;
```

**Parameters:**
- `a`: First set (array of values)
- `b`: Second set (array of values)

**Returns:** Cartesian product (a × b) as array of [a, b] pairs

**Usage Examples:**

```javascript
import { setCartesian } from 'mathjs';

setCartesian([1, 2], [3, 4]);
// [[1, 3], [1, 4], [2, 3], [2, 4]]

setCartesian(['a', 'b'], [1, 2]);
// [['a', 1], ['a', 2], ['b', 1], ['b', 2]]

// Empty set produces empty result
setCartesian([1, 2], []);
// []
```

### setPowerset

Returns the power set of a set (all possible subsets including empty set).

```typescript { .api }
function setPowerset(a: Array | Matrix): Array | Matrix;
```

**Parameters:**
- `a`: Input set (array of values)

**Returns:** Power set (all subsets of a)

**Usage Examples:**

```javascript
import { setPowerset } from 'mathjs';

setPowerset([1, 2]);
// [[], [1], [2], [1, 2]]

setPowerset([1, 2, 3]);
// [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]

setPowerset(['a']);
// [[], ['a']]

// Power set of empty set is set containing empty set
setPowerset([]);
// [[]]

// Size of power set is 2^n where n is set size
```

## Set Properties and Utilities

### setDistinct

Removes duplicate elements from a set (array).

```typescript { .api }
function setDistinct(a: Array | Matrix): Array | Matrix;
```

**Parameters:**
- `a`: Input array (may contain duplicates)

**Returns:** Array with unique elements only

**Usage Examples:**

```javascript
import { setDistinct } from 'mathjs';

setDistinct([1, 2, 2, 3, 3, 3]);
// [1, 2, 3]

setDistinct([1, 1, 1, 1]);
// [1]

setDistinct(['a', 'b', 'a', 'c', 'b']);
// ['a', 'b', 'c']

// Preserves order of first occurrence
setDistinct([3, 1, 2, 1, 3]);
// [3, 1, 2]
```

### setSize

Returns the cardinality (number of unique elements) of a set.

```typescript { .api }
function setSize(a: Array | Matrix): number;
```

**Parameters:**
- `a`: Input set (array of values)

**Returns:** Number of unique elements in the set

**Usage Examples:**

```javascript
import { setSize } from 'mathjs';

setSize([1, 2, 3]);
// 3

setSize([1, 2, 2, 3, 3, 3]);
// 3 (counts unique elements only)

setSize([]);
// 0

setSize(['a', 'b', 'c', 'a']);
// 3
```

### setIsSubset

Checks if one set is a subset of another set.

```typescript { .api }
function setIsSubset(a: Array | Matrix, b: Array | Matrix): boolean;
```

**Parameters:**
- `a`: Potential subset
- `b`: Potential superset

**Returns:** true if a ⊆ b (all elements of a are in b), false otherwise

**Usage Examples:**

```javascript
import { setIsSubset } from 'mathjs';

setIsSubset([1, 2], [1, 2, 3, 4]);
// true (a is subset of b)

setIsSubset([1, 2, 3], [1, 2]);
// false (a has elements not in b)

// Every set is a subset of itself
setIsSubset([1, 2, 3], [1, 2, 3]);
// true

// Empty set is subset of every set
setIsSubset([], [1, 2, 3]);
// true

setIsSubset(['a', 'b'], ['a', 'b', 'c']);
// true
```

### setMultiplicity

Returns the multiplicity of an element in a multiset (number of occurrences).

```typescript { .api }
function setMultiplicity(element: any, set: Array | Matrix): number;
```

**Parameters:**
- `element`: Element to count
- `set`: Multiset (array that may contain duplicates)

**Returns:** Number of times element appears in set

**Usage Examples:**

```javascript
import { setMultiplicity } from 'mathjs';

setMultiplicity(2, [1, 2, 2, 3, 2]);
// 3

setMultiplicity(5, [1, 2, 3, 4]);
// 0 (element not in set)

setMultiplicity('a', ['a', 'b', 'a', 'c', 'a']);
// 3

// Works with complex values
setMultiplicity([1, 2], [[1, 2], [3, 4], [1, 2]]);
// 2
```

## Common Types

```typescript { .api }
type MathArray<T> = T[] | Array<MathArray<T>>;
```
