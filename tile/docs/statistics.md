# mathjs Statistics and Probability

mathjs v15.1.0 — Statistics, Probability, Combinatorics, and Special Functions.

---

## Statistics Functions

### cumsum

Compute the cumulative sum of a matrix or a list with values. In case of a multi-dimensional array or matrix, the cumulative sums along a specified dimension (defaulting to the first) will be calculated.

```typescript { .api }
// Variadic: cumulative sums of scalar values
cumsum(...args: MathType[]): MathType[]

// Single matrix with optional dimension (returns matrix/array)
cumsum(array: MathCollection, dim?: number): MathCollection
```

### mad

Compute the median absolute deviation of a matrix or a list with values. The median absolute deviation is defined as the median of the absolute deviations from the median.

```typescript { .api }
mad(array: MathCollection): any
```

### max

Compute the maximum value of a matrix or a list with values. In case of a multi-dimensional array, the maximum of the flattened array will be calculated. When `dimension` is provided, the maximum over the selected dimension will be calculated. Parameter `dimension` is zero-based.

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
max<T extends MathScalarType>(...args: T[]): T

// Mixed variadic scalars
max(...args: MathScalarType[]): MathScalarType

// Typed array/matrix with optional dimension
max<T extends MathScalarType>(A: T[] | T[][], dimension?: number | BigNumber): T

// Generic collection with optional dimension
max(A: MathCollection, dimension?: number | BigNumber): MathScalarType
```

### mean

Compute the mean value of a matrix or a list with values. In case of a multi-dimensional array, the mean of the flattened array will be calculated. When `dimension` is provided, the mean over the selected dimension will be calculated. Parameter `dimension` is zero-based.

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
mean<T extends MathScalarType>(...args: T[]): T

// Mixed variadic scalars
mean(...args: MathScalarType[]): MathScalarType

// Typed array/matrix with optional dimension
mean<T extends MathScalarType>(A: T[] | T[][], dimension?: number | BigNumber): T

// Generic collection with optional dimension
mean(A: MathCollection, dimension?: number | BigNumber): MathScalarType
```

### median

Compute the median of a matrix or a list with values. The values are sorted and the middle value is returned. In case of an even number of values, the average of the two middle values is returned. Supported types: Number, BigNumber, Unit.

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
median<T extends MathScalarType>(...args: T[]): T

// Mixed variadic scalars
median(...args: MathScalarType[]): MathScalarType

// Typed array/matrix
median<T extends MathScalarType>(A: T[] | T[][]): T

// Generic collection
median(A: MathCollection): MathScalarType
```

### min

Compute the minimum value of a matrix or a list of values. In case of a multi-dimensional array, the minimum of the flattened array will be calculated. When `dimension` is provided, the minimum over the selected dimension will be calculated. Parameter `dimension` is zero-based.

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
min<T extends MathScalarType>(...args: T[]): T

// Mixed variadic scalars
min(...args: MathScalarType[]): MathScalarType

// Typed array/matrix with optional dimension
min<T extends MathScalarType>(A: T[] | T[][], dimension?: number | BigNumber): T

// Generic collection with optional dimension
min(A: MathCollection, dimension?: number | BigNumber): MathScalarType
```

### mode

Computes the mode of a set of numbers or a list with values (numbers or characters). If there are more than one modes, it returns a list of those values.

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
mode<T extends MathScalarType>(...args: T[]): T[]

// Mixed variadic scalars
mode(...args: MathScalarType[]): MathScalarType[]

// Typed array/matrix
mode<T extends MathScalarType>(A: T[] | T[][]): T[]

// Generic collection
mode(A: MathCollection): MathScalarType[]
```

### prod

Compute the product of a matrix or a list with values. In case of a multi-dimensional array or matrix, the product of all elements will be calculated.

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
prod<T extends MathScalarType>(...args: T[]): T

// Mixed variadic scalars
prod(...args: MathScalarType[]): MathScalarType

// Typed array/matrix
prod<T extends MathScalarType>(A: T[] | T[][]): T

// Generic collection
prod(A: MathCollection): MathScalarType
```

### quantileSeq

Compute the `prob` order quantile of a matrix or a list with values. The sequence is sorted and the quantile value is returned. Supported types of sequence values are: Number, BigNumber, Unit. Supported types of probability are: Number, BigNumber.

```typescript { .api }
// Typed array/matrix with a single probability — return type matches element type
quantileSeq<T extends MathScalarType>(
  A: T[] | T[][],
  prob: number | BigNumber,
  sorted?: boolean
): T

// Generic collection; prob may be a number, BigNumber, or array of probabilities
quantileSeq(
  A: MathCollection,
  prob: number | BigNumber | MathArray,
  sorted?: boolean
): MathScalarType | MathArray
```

### std

Compute the standard deviation of a matrix or a list with values. Defined as `std(A) = sqrt(variance(A))`. In case of a multi-dimensional array or matrix, the standard deviation over all elements will be calculated.

Normalization options:
- `'unbiased'` (default) — divide by `n - 1`
- `'uncorrected'` — divide by `n`
- `'biased'` — divide by `n + 1`

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
std<T extends MathScalarType>(...args: T[]): T

// Mixed variadic scalars
std(...args: MathScalarType[]): MathScalarType

// Collection with optional dimension and normalization — returns array of std values
std(
  array: MathCollection,
  dimension?: number,
  normalization?: 'unbiased' | 'uncorrected' | 'biased'
): MathNumericType[]

// Collection with normalization only — returns single std value
std(
  array: MathCollection,
  normalization: 'unbiased' | 'uncorrected' | 'biased'
): MathNumericType
```

### sum

Compute the sum of a matrix or a list with values. In case of a multi-dimensional array or matrix, the sum of all elements will be calculated.

```typescript { .api }
// Homogeneous variadic scalars — return type matches input type
sum<T extends MathScalarType>(...args: T[]): T

// Mixed variadic scalars
sum(...args: MathScalarType[]): MathScalarType

// Typed array/matrix with optional dimension
sum<T extends MathScalarType>(A: T[] | T[][], dimension?: number | BigNumber): T

// Generic collection with optional dimension
sum(A: MathCollection, dimension?: number | BigNumber): MathScalarType
```

### variance

Compute the variance of a matrix or a list with values. In case of a multi-dimensional array or matrix, the variance over all elements will be calculated.

Normalization options:
- `'unbiased'` (default) — divide by `n - 1`
- `'uncorrected'` — divide by `n`
- `'biased'` — divide by `n + 1`

Note: `math['var']` is the legacy alias for this function.

```typescript { .api }
// Variadic scalars
variance(...args: MathNumericType[]): MathNumericType

// Collection with optional dimension and normalization — returns array of variance values
variance(
  array: MathCollection,
  dimension?: number,
  normalization?: 'unbiased' | 'uncorrected' | 'biased'
): MathNumericType[]

// Collection with normalization only — returns single variance value
variance(
  array: MathCollection,
  normalization: 'unbiased' | 'uncorrected' | 'biased'
): MathNumericType
```

### count

Count the number of elements of a matrix, array, or string.

```typescript { .api }
count(x: MathCollection | string): number
```

### corr

Calculate the correlation coefficient between two matrices or arrays.

```typescript { .api }
corr(x: MathCollection, y: MathCollection): MathType
```

---

## Probability Functions

### bernoulli

Compute the nth Bernoulli number.

```typescript { .api }
bernoulli<T extends number | Fraction | BigNumber>(n: T): NoLiteralType<T>
bernoulli(n: bigint): Fraction
```

### combinations

Compute the number of ways of picking `k` unordered outcomes from `n` possibilities (binomial coefficient C(n, k)). Only takes integer arguments. Requires `k <= n`.

```typescript { .api }
combinations<T extends number | BigNumber>(
  n: T,
  k: number | BigNumber
): NoLiteralType<T>
```

### combinationsWithRep

Compute the number of ways of picking `k` unordered outcomes from `n` possibilities, allowing repetition. Only takes integer arguments. Requires `k >= 1` and `n >= 1`.

Note: this function exists in the mathjs runtime but is not present in the official `types/index.d.ts` TypeScript definitions. Use a type assertion if needed.

```typescript { .api }
combinationsWithRep<T extends number | BigNumber>(
  n: T,
  k: number | BigNumber
): NoLiteralType<T>
```

### factorial

Compute the factorial of a value. Only supports integer arguments. For matrices, the function is evaluated element wise.

```typescript { .api }
factorial<T extends number | BigNumber | MathCollection>(n: T): NoLiteralType<T>
```

### gamma

Compute the gamma function of a value using Lanczos approximation for small values and an extended Stirling approximation for large values.

```typescript { .api }
gamma<T extends number | BigNumber | Complex>(n: T): NoLiteralType<T>
```

### lgamma

Compute the log gamma function of a value, using Lanczos approximation for numbers and Stirling series for complex numbers. Equivalent to `ln(Γ(n))`.

```typescript { .api }
lgamma<T extends number | Complex>(n: T): NoLiteralType<T>
```

### kldivergence

Calculate the Kullback-Leibler (KL) divergence between two distributions.

```typescript { .api }
kldivergence(q: MathCollection, p: MathCollection): number
```

### multinomial

Compute the multinomial coefficient: the number of ways of picking `a1, a2, ..., ai` unordered outcomes from `n` possibilities. Takes one array of integers as an argument. Every `ai` must be >= 0.

```typescript { .api }
multinomial<T extends number | BigNumber>(a: T[]): NoLiteralType<T>
```

### permutations

Compute the number of ways of obtaining an ordered subset of `k` elements from a set of `n` elements (P(n, k)). Only takes integer arguments. Requires `k <= n`. If `k` is omitted, computes `n!`.

```typescript { .api }
permutations<T extends number | BigNumber>(
  n: T,
  k?: number | BigNumber
): NoLiteralType<T>
```

### pickRandom

Randomly pick one or more values from a one-dimensional array using a uniform distribution. Supports weighted sampling.

```typescript { .api }
// Pick a single random element
pickRandom<T>(array: T[]): T

// Pick `number` random elements (without weights)
pickRandom<T>(array: T[], number: number): T[]

// Pick `number` random elements with associated weights
pickRandom<T>(array: T[], number: number, weights: number[]): T[]
```

### random

Return a random number in the range `[min, max)` using a uniform distribution. Defaults to `[0, 1)` when no bounds are provided.

```typescript { .api }
// Scalar random value
random(min?: number, max?: number): number

// Collection filled with random values of matching shape
random<T extends MathCollection>(size: T, min?: number, max?: number): T
```

### randomInt

Return a random integer in the range `[min, max)` using a uniform distribution.

```typescript { .api }
// Scalar random integer
randomInt(min: number, max?: number): number

// Collection filled with random integers of matching shape
randomInt<T extends MathCollection>(size: T, min?: number, max?: number): T
```

---

## Combinatorics Functions

### bellNumbers

The Bell Numbers count the number of partitions of a set. A partition is a collection of pairwise disjoint subsets of S whose union is S. Only takes integer arguments. Requires `n >= 0`.

```typescript { .api }
bellNumbers<T extends number | BigNumber>(n: T): T
```

### catalan

The Catalan Numbers enumerate combinatorial structures of many different types. Only takes integer arguments. Requires `n >= 0`.

```typescript { .api }
catalan<T extends number | BigNumber>(n: T): T
```

### composition

The composition counts the number of ways of writing `n` as an ordered sum of `k` positive integers. Only takes integer arguments. Requires `k <= n`.

```typescript { .api }
composition<T extends number | BigNumber>(
  n: T,
  k: number | BigNumber
): NoLiteralType<T>
```

### stirlingS2

The Stirling numbers of the second kind count the number of ways to partition a set of `n` labelled objects into `k` nonempty unlabelled subsets. Only takes integer arguments. Requires `k <= n`. If `n = k` or `k = 1`, then `S(n, k) = 1`.

```typescript { .api }
stirlingS2<T extends number | BigNumber>(
  n: T,
  k: number | BigNumber
): NoLiteralType<T>
```

---

## Special Functions

### erf

Compute the error function of a value using rational Chebyshev approximations for different intervals of x.

```typescript { .api }
erf<T extends number | MathCollection>(x: T): NoLiteralType<T>
```

### zeta

Compute the Riemann Zeta function of a value using an infinite series and Riemann's functional equation.

```typescript { .api }
zeta<T extends number | Complex | BigNumber>(s: T): T
```
