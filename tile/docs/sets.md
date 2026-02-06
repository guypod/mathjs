# Set Operations

Mathematical set operations on arrays including union, intersection, difference, and set properties. These functions treat arrays as mathematical sets.

## Capabilities

### Basic Set Operations

Fundamental set operations following mathematical set theory.

```javascript { .api }
/**
 * Calculate the union of two sets (A ∪ B)
 * Returns all unique elements from both sets
 * @param a1 - First set (array)
 * @param a2 - Second set (array)
 * @returns Union of both sets
 */
function setUnion(a1: MathCollection, a2: MathCollection): MathCollection

/**
 * Calculate the intersection of two sets (A ∩ B)
 * Returns elements that exist in both sets
 * @param a1 - First set (array)
 * @param a2 - Second set (array)
 * @returns Intersection of both sets
 */
function setIntersect(a1: MathCollection, a2: MathCollection): MathCollection

/**
 * Calculate the set difference (A \ B)
 * Returns elements in first set but not in second
 * @param a1 - First set (array)
 * @param a2 - Second set (array)
 * @returns Set difference (A minus B)
 */
function setDifference(a1: MathCollection, a2: MathCollection): MathCollection

/**
 * Calculate the symmetric difference (A Δ B)
 * Returns elements in either set but not in both
 * @param a1 - First set (array)
 * @param a2 - Second set (array)
 * @returns Symmetric difference
 */
function setSymDifference(a1: MathCollection, a2: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { setUnion, setIntersect, setDifference, setSymDifference } from 'mathjs'

const A = [1, 2, 3, 4, 5]
const B = [3, 4, 5, 6, 7]

// Union (all unique elements)
setUnion(A, B)                // [1, 2, 3, 4, 5, 6, 7]

// Intersection (common elements)
setIntersect(A, B)            // [3, 4, 5]

// Difference (in A but not B)
setDifference(A, B)           // [1, 2]
setDifference(B, A)           // [6, 7]

// Symmetric difference (in either but not both)
setSymDifference(A, B)        // [1, 2, 6, 7]

// Empty sets
setIntersect([1, 2], [3, 4])  // []
setUnion([], [1, 2, 3])       // [1, 2, 3]
```

### Set Properties

Functions to analyze set properties.

```javascript { .api }
/**
 * Remove duplicate elements (create distinct set)
 * @param a - Input array
 * @returns Array with unique elements only
 */
function setDistinct(a: MathCollection): MathCollection

/**
 * Calculate the cardinality (size) of a set
 * @param a - Input set
 * @returns Number of unique elements
 */
function setSize(a: MathCollection): number

/**
 * Test if first set is a subset of second (A ⊆ B)
 * @param a1 - Potential subset
 * @param a2 - Potential superset
 * @returns true if a1 is subset of a2
 */
function setIsSubset(a1: MathCollection, a2: MathCollection): boolean

/**
 * Count multiplicity (occurrences) of element in set
 * @param e - Element to count
 * @param a - Input array
 * @returns Number of occurrences
 */
function setMultiplicity(e: any, a: MathCollection): number
```

**Usage Examples:**

```javascript
import { setDistinct, setSize, setIsSubset, setMultiplicity } from 'mathjs'

// Remove duplicates
setDistinct([1, 2, 2, 3, 3, 3])      // [1, 2, 3]
setDistinct([1, 1, 1, 1])            // [1]

// Set size (cardinality)
setSize([1, 2, 3, 4, 5])             // 5
setSize([1, 2, 2, 3])                // 3 (counts unique elements)

// Subset test
setIsSubset([1, 2], [1, 2, 3, 4])    // true
setIsSubset([1, 5], [1, 2, 3, 4])    // false (5 not in superset)
setIsSubset([1, 2, 3], [1, 2, 3])    // true (equal sets)

// Multiplicity (count occurrences)
setMultiplicity(2, [1, 2, 2, 3, 2])  // 3
setMultiplicity(5, [1, 2, 3])        // 0
```

### Advanced Set Operations

Additional set-theoretic operations.

```javascript { .api }
/**
 * Calculate the Cartesian product (A × B)
 * Returns all ordered pairs (a, b) where a ∈ A and b ∈ B
 * @param a1 - First set
 * @param a2 - Second set
 * @returns Cartesian product as array of pairs
 */
function setCartesian(a1: MathCollection, a2: MathCollection): MathCollection

/**
 * Calculate the power set (all subsets)
 * Returns all possible subsets of the input set
 * @param a - Input set
 * @returns Power set (array of all subsets)
 */
function setPowerset(a: MathCollection): MathCollection
```

**Usage Examples:**

```javascript
import { setCartesian, setPowerset } from 'mathjs'

// Cartesian product
setCartesian([1, 2], ['a', 'b'])
// [[1, 'a'], [1, 'b'], [2, 'a'], [2, 'b']]

setCartesian([1, 2, 3], [4, 5])
// [[1,4], [1,5], [2,4], [2,5], [3,4], [3,5]]

// Power set (all subsets)
setPowerset([1, 2])
// [[], [1], [2], [1, 2]]

setPowerset([1, 2, 3])
// [[], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]]

// Size of power set is 2^n
setPowerset([1, 2, 3, 4]).length     // 16 (2^4)
```

## Set Theory Applications

### Venn Diagram Operations

```javascript
import { setUnion, setIntersect, setDifference, setSymDifference } from 'mathjs'

const A = [1, 2, 3, 4]
const B = [3, 4, 5, 6]
const C = [4, 5, 6, 7]

// Elements in A or B
setUnion(A, B)                       // [1, 2, 3, 4, 5, 6]

// Elements in all three sets
const AB = setIntersect(A, B)        // [3, 4]
const ABC = setIntersect(AB, C)      // [4]

// Elements in A but not in B or C
const BC = setUnion(B, C)            // [3, 4, 5, 6, 7]
setDifference(A, BC)                 // [1, 2]

// Elements in exactly one set
const AorB = setUnion(A, B)
const AandB = setIntersect(A, B)
setDifference(AorB, AandB)           // [1, 2, 5, 6]
```

### Set Relationships

```javascript
import { setIsSubset, setIntersect, setSize, setEqual } from 'mathjs'

const A = [1, 2, 3]
const B = [1, 2, 3, 4, 5]
const C = [4, 5, 6]

// Check subset relationship
setIsSubset(A, B)                    // true (A ⊆ B)
setIsSubset(B, A)                    // false

// Check proper subset (A ⊂ B means A ⊆ B and A ≠ B)
function isProperSubset(a1, a2) {
  return setIsSubset(a1, a2) && setSize(a1) < setSize(a2)
}
isProperSubset(A, B)                 // true
isProperSubset(A, A)                 // false

// Check disjoint sets (A ∩ B = ∅)
function areDisjoint(a1, a2) {
  return setSize(setIntersect(a1, a2)) === 0
}
areDisjoint(A, C)                    // true
areDisjoint(A, B)                    // false

// Check set equality
function setEqual(a1, a2) {
  return setIsSubset(a1, a2) && setIsSubset(a2, a1)
}
setEqual(A, [1, 2, 3])               // true
setEqual(A, [1, 2, 3, 3])            // true (duplicates ignored)
```

### Set Builder Patterns

```javascript
import { setDistinct, setIntersect, setDifference, filter } from 'mathjs'

// Create sets with conditions
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// Even numbers: {x ∈ numbers | x mod 2 = 0}
const evens = filter(numbers, x => x % 2 === 0)  // [2, 4, 6, 8, 10]

// Odd numbers
const odds = setDifference(numbers, evens)       // [1, 3, 5, 7, 9]

// Prime numbers (simple sieve)
function isPrime(n) {
  if (n < 2) return false
  for (let i = 2; i * i <= n; i++) {
    if (n % i === 0) return false
  }
  return true
}
const primes = filter(numbers, isPrime)          // [2, 3, 5, 7]

// Composite numbers
const composites = setDifference(
  setDifference(numbers, primes),
  [1]
)  // [4, 6, 8, 9, 10]
```

### Partition Operations

```javascript
import { setUnion, setIntersect, setSize } from 'mathjs'

// Check if sets form a partition of a universal set
function isPartition(universalSet, partitions) {
  // 1. Union of all partitions equals universal set
  let union = partitions[0]
  for (let i = 1; i < partitions.length; i++) {
    union = setUnion(union, partitions[i])
  }
  if (setSize(union) !== setSize(universalSet)) return false

  // 2. All partitions are pairwise disjoint
  for (let i = 0; i < partitions.length; i++) {
    for (let j = i + 1; j < partitions.length; j++) {
      if (setSize(setIntersect(partitions[i], partitions[j])) > 0) {
        return false
      }
    }
  }

  return true
}

const universal = [1, 2, 3, 4, 5, 6]
const partition1 = [[1, 2], [3, 4], [5, 6]]      // Valid partition
const partition2 = [[1, 2, 3], [3, 4, 5]]        // Invalid (3 in both)

isPartition(universal, partition1)               // true
isPartition(universal, partition2)               // false
```

## Working with Different Data Types

Set operations work with various data types:

```javascript
import { setUnion, setIntersect, setDistinct } from 'mathjs'

// Numbers
setUnion([1, 2, 3], [3, 4, 5])       // [1, 2, 3, 4, 5]

// Strings
setIntersect(['a', 'b', 'c'], ['b', 'c', 'd'])  // ['b', 'c']

// Mixed types (compared using deep equality)
setDistinct([1, '1', 1, '1'])        // [1, '1']

// Objects (by reference)
const obj1 = { a: 1 }
const obj2 = { a: 1 }
setDistinct([obj1, obj2, obj1])      // [obj1, obj2] (different references)
```

## Mathematical Set Notation

Common set operations in mathematical notation and Math.js:

| Mathematical Notation | Math.js Function | Description |
|----------------------|------------------|-------------|
| A ∪ B | `setUnion(A, B)` | Union |
| A ∩ B | `setIntersect(A, B)` | Intersection |
| A \ B or A - B | `setDifference(A, B)` | Difference |
| A Δ B or A ⊕ B | `setSymDifference(A, B)` | Symmetric difference |
| A ⊆ B | `setIsSubset(A, B)` | Subset |
| |A| or #A | `setSize(A)` | Cardinality |
| A × B | `setCartesian(A, B)` | Cartesian product |
| P(A) or 2^A | `setPowerset(A)` | Power set |
| {x : x ∈ A, P(x)} | `filter(A, P)` | Set builder |

## Performance Considerations

- Set operations internally create distinct sets, removing duplicates
- For large sets, operations have O(n) to O(n²) complexity
- `setPowerset` generates 2^n subsets; use cautiously for large sets
- `setCartesian` generates |A| × |B| pairs; can be large
- For frequent set operations, consider pre-processing with `setDistinct`

**Example:**

```javascript
import { setPowerset } from 'mathjs'

// Small set: manageable
setPowerset([1, 2, 3]).length        // 8 subsets (2^3)

// Medium set: use caution
setPowerset([1,2,3,4,5]).length      // 32 subsets (2^5)

// Large set: may cause memory issues
// setPowerset([1..20])              // 1,048,576 subsets! (2^20)
```
