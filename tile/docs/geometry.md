# Geometry Functions

Math.js provides geometric functions for calculating distances and intersections in 2D and 3D space. These functions support both arrays and matrices for vector representations.

## Core Imports

```javascript { .api }
import { distance, intersect } from 'mathjs';
```

## Capabilities

### Distance Calculation

Calculate the Euclidean distance between two points in 2D or 3D space.

```typescript { .api }
/**
 * Calculate Euclidean distance between two points
 * @param x - First point (2D or 3D coordinates as array or matrix)
 * @param y - Second point (2D or 3D coordinates as array or matrix)
 * @returns Distance as number, BigNumber, or Fraction
 */
function distance(x: MathArray | Matrix, y: MathArray | Matrix): number | BigNumber | Fraction;
```

**Parameters:**
- `x`: First point as array [x, y] or [x, y, z]
- `y`: Second point as array [x, y] or [x, y, z]

**Returns:** Euclidean distance between the two points

**Usage Examples:**

```javascript
import { distance, bignumber } from 'mathjs';

// 2D distance
distance([0, 0], [3, 4]);              // 5

// 3D distance
distance([0, 0, 0], [1, 2, 2]);        // 3

// More 2D examples
distance([1, 1], [4, 5]);              // 5
distance([-2, 3], [1, 7]);             // 5

// Works with BigNumber
distance(
  [bignumber(0), bignumber(0)],
  [bignumber(3), bignumber(4)]
);                                      // BigNumber 5

// Matrix inputs
import { matrix } from 'mathjs';
distance(
  matrix([0, 0]),
  matrix([3, 4])
);                                      // 5

// Practical example: distance between cities
const city1 = [40.7128, -74.0060];     // NYC (simplified coordinates)
const city2 = [34.0522, -118.2437];    // LA (simplified coordinates)
const dist = distance(city1, city2);
// Note: This gives Euclidean distance, not great circle distance
```

### Line and Plane Intersection

Calculate the intersection point of two lines in 2D or two planes in 3D.

```typescript { .api }
/**
 * Calculate intersection of two lines (2D) or two planes (3D)
 * 
 * 2D: Finds intersection point of lines w + x * t1 and y + z * t2
 * 3D: Finds intersection line of planes w + x * t1 + y * t2 and z + ... 
 * 
 * @param w - Point on first line (2D) or plane (3D)
 * @param x - Direction vector of first line (2D) or plane (3D)
 * @param y - Point on second line (2D) or plane (3D)
 * @param z - Direction vector of second line (2D) or plane (3D)
 * @returns Intersection point (2D) or line (3D), or null if no intersection
 */
function intersect(
  w: MathArray | Matrix,
  x: MathArray | Matrix,
  y: MathArray | Matrix,
  z: MathArray | Matrix
): MathArray | Matrix | null;
```

**Parameters (2D - Line Intersection):**
- `w`: Point on first line [x, y]
- `x`: Direction vector of first line [dx, dy]
- `y`: Point on second line [x, y]
- `z`: Direction vector of second line [dx, dy]

**Parameters (3D - Plane Intersection):**
- `w`: Point on first plane [x, y, z]
- `x`: First direction vector of first plane [dx, dy, dz]
- `y`: Second direction vector of first plane [dx, dy, dz]
- `z`: Point on second plane [x, y, z]

**Returns:** Intersection point for 2D lines, intersection line for 3D planes, or null if parallel/no intersection

**Usage Examples:**

```javascript
import { intersect } from 'mathjs';

// 2D line intersection
// Line 1: passes through [0, 0] with direction [1, 1] (y = x)
// Line 2: passes through [0, 2] with direction [1, -1] (y = -x + 2)
const intersection = intersect(
  [0, 0],  // point on line 1
  [1, 1],  // direction of line 1
  [0, 2],  // point on line 2
  [1, -1]  // direction of line 2
);
// Result: [1, 1]

// Parallel lines (no intersection)
const parallel = intersect(
  [0, 0],  // point on line 1
  [1, 0],  // direction [1, 0] - horizontal
  [0, 1],  // point on line 2
  [1, 0]   // direction [1, 0] - also horizontal
);
// Result: null (parallel lines don't intersect)

// Perpendicular lines
const perp = intersect(
  [0, 0],   // point on line 1
  [1, 0],   // horizontal direction
  [2, -1],  // point on line 2
  [0, 1]    // vertical direction
);
// Result: [2, 0]

// With matrices
import { matrix } from 'mathjs';
intersect(
  matrix([0, 0]),
  matrix([1, 1]),
  matrix([0, 2]),
  matrix([1, -1])
);
// Result: matrix([1, 1])

// Practical example: find where two paths cross
const path1Point = [10, 20];
const path1Direction = [1, 2];     // slope = 2
const path2Point = [0, 30];
const path2Direction = [3, -1];    // slope = -1/3

const meeting = intersect(
  path1Point,
  path1Direction,
  path2Point,
  path2Direction
);
```

**Special Cases:**
- Returns `null` if lines are parallel (no intersection)
- Returns `null` if lines are coincident (infinite intersections)
- For 3D plane intersections, returns the intersection line parameters

## Common Types

```typescript { .api }
type MathArray<T> = T[] | Array<MathArray<T>>;
type MathCollection = MathArray<any> | Matrix;
```
