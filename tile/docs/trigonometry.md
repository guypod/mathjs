# mathjs Trigonometry, Bitwise, and Logical

mathjs v15.1.0 — Trigonometric, Bitwise, Logical, and Complex Number Functions.

---

## Trigonometry Functions

All trigonometric functions operate element-wise on arrays and matrices. Functions that accept a `Unit` input evaluate on the numeric value of the unit in its base unit.

### Standard Trigonometric Functions

#### sin

Calculate the sine of a value.

```typescript { .api }
sin(x: number | Unit): number
sin<T extends BigNumber | Complex>(x: T): T
```

#### cos

Calculate the cosine of a value.

```typescript { .api }
cos(x: number | Unit): number
cos<T extends BigNumber | Complex>(x: T): T
```

#### tan

Calculate the tangent of a value. Defined as `tan(x) = sin(x) / cos(x)`.

```typescript { .api }
tan(x: number | Unit): number
tan<T extends BigNumber | Complex>(x: T): T
```

#### cot

Calculate the cotangent of a value. Defined as `cot(x) = 1 / tan(x)`.

```typescript { .api }
cot(x: number | Unit): number
cot<T extends BigNumber | Complex>(x: T): T
```

#### sec

Calculate the secant of a value. Defined as `sec(x) = 1 / cos(x)`.

```typescript { .api }
sec(x: number | Unit): number
sec<T extends BigNumber | Complex>(x: T): T
```

#### csc

Calculate the cosecant of a value. Defined as `csc(x) = 1 / sin(x)`.

```typescript { .api }
csc(x: number | Unit): number
csc<T extends BigNumber | Complex>(x: T): T
```

---

### Inverse Trigonometric Functions

#### asin

Calculate the inverse sine (arc sine) of a value. Returns a Complex number when `|x| > 1`.

```typescript { .api }
asin(x: number): number | Complex
asin<T extends BigNumber | Complex>(x: T): T
```

#### acos

Calculate the inverse cosine (arc cosine) of a value. Returns a Complex number when `|x| > 1`.

```typescript { .api }
acos(x: number): number | Complex
acos<T extends BigNumber | Complex>(x: T): T
```

#### atan

Calculate the inverse tangent (arc tangent) of a value.

```typescript { .api }
atan<T extends number | BigNumber | Complex>(x: T): T
```

#### atan2

Calculate the four-quadrant inverse tangent of `y / x`. By providing two arguments, the correct quadrant of the computed angle can be determined. For matrices, the function is evaluated element wise.

```typescript { .api }
atan2<T extends number | MathCollection>(y: T, x: T): T
```

#### acot

Calculate the inverse cotangent (arc cotangent) of a value.

```typescript { .api }
acot(x: number): number
acot<T extends BigNumber | Complex>(x: T): T
```

#### asec

Calculate the inverse secant (arc secant) of a value. Returns a Complex number when `|x| < 1`.

```typescript { .api }
asec(x: number): number | Complex
asec<T extends BigNumber | Complex>(x: T): T
```

#### acsc

Calculate the inverse cosecant (arc cosecant) of a value. Returns a Complex number when `|x| < 1`.

```typescript { .api }
acsc(x: number): number | Complex
acsc<T extends BigNumber | Complex>(x: T): T
```

---

### Hyperbolic Functions

#### sinh

Calculate the hyperbolic sine of a value. Defined as `sinh(x) = (e^x - e^{-x}) / 2`.

```typescript { .api }
sinh(x: number | Unit): number
sinh<T extends BigNumber | Complex>(x: T): T
```

#### cosh

Calculate the hyperbolic cosine of a value. Defined as `cosh(x) = (e^x + e^{-x}) / 2`.

```typescript { .api }
cosh(x: number | Unit): number
cosh<T extends BigNumber | Complex>(x: T): T
```

#### tanh

Calculate the hyperbolic tangent of a value. Defined as `tanh(x) = (e^{2x} - 1) / (e^{2x} + 1)`.

```typescript { .api }
tanh(x: number | Unit): number
tanh<T extends BigNumber | Complex>(x: T): T
```

#### coth

Calculate the hyperbolic cotangent of a value. Defined as `coth(x) = 1 / tanh(x)`.

```typescript { .api }
coth(x: number | Unit): number
coth<T extends BigNumber | Complex>(x: T): T
```

#### sech

Calculate the hyperbolic secant of a value. Defined as `sech(x) = 1 / cosh(x)`.

```typescript { .api }
sech(x: number | Unit): number
sech<T extends BigNumber | Complex>(x: T): T
```

#### csch

Calculate the hyperbolic cosecant of a value. Defined as `csch(x) = 1 / sinh(x)`.

```typescript { .api }
csch(x: number | Unit): number
csch<T extends BigNumber | Complex>(x: T): T
```

---

### Inverse Hyperbolic Functions

#### asinh

Calculate the inverse hyperbolic sine of a value. Defined as `asinh(x) = ln(x + sqrt(x^2 + 1))`.

```typescript { .api }
asinh<T extends number | BigNumber | Complex>(x: T): T
```

#### acosh

Calculate the inverse hyperbolic cosine of a value. Defined as `acosh(x) = ln(sqrt(x^2 - 1) + x)`. Returns a Complex number when `x < 1`.

```typescript { .api }
acosh(x: number): number | Complex
acosh<T extends BigNumber | Complex>(x: T): T
```

#### atanh

Calculate the inverse hyperbolic tangent of a value. Defined as `atanh(x) = ln((1 + x) / (1 - x)) / 2`. Returns a Complex number when `|x| > 1`.

```typescript { .api }
atanh(x: number): number | Complex
atanh<T extends BigNumber | Complex>(x: T): T
```

#### acoth

Calculate the inverse hyperbolic cotangent of a value. Defined as `acoth(x) = (ln((x+1)/x) + ln(x/(x-1))) / 2`.

```typescript { .api }
acoth(x: number): number
acoth<T extends BigNumber | Complex>(x: T): T
```

#### asech

Calculate the inverse hyperbolic secant of a value. Defined as `asech(x) = ln(sqrt(1/x^2 - 1) + 1/x)`. Returns a Complex number when `x > 1`.

```typescript { .api }
asech(x: number): number | Complex
asech<T extends BigNumber | Complex>(x: T): T
```

#### acsch

Calculate the inverse hyperbolic cosecant of a value. Defined as `acsch(x) = ln(1/x + sqrt(1/x^2 + 1))`.

```typescript { .api }
acsch(x: number): number
acsch<T extends BigNumber | Complex>(x: T): T
```

---

## Bitwise Functions

All bitwise functions operate element-wise on arrays and matrices. `bigint` is supported alongside `number` and `BigNumber`.

### bitAnd

Bitwise AND two values: `x & y`.

```typescript { .api }
bitAnd<T extends number | BigNumber | bigint | MathCollection>(
  x: T,
  y: number | BigNumber | bigint | MathCollection
): NoLiteralType<T>
```

### bitNot

Bitwise NOT a value: `~x`.

```typescript { .api }
bitNot<T extends number | BigNumber | bigint | MathCollection>(x: T): T
```

### bitOr

Bitwise OR two values: `x | y`.

```typescript { .api }
bitOr<T extends number | BigNumber | bigint | MathCollection>(x: T, y: T): T
```

### bitXor

Bitwise XOR two values: `x ^ y`.

```typescript { .api }
bitXor<T extends number | BigNumber | bigint | MathCollection>(
  x: T,
  y: number | BigNumber | bigint | MathCollection
): NoLiteralType<T>
```

### leftShift

Bitwise left logical shift: `x << y`.

```typescript { .api }
leftShift<T extends number | BigNumber | bigint | MathCollection>(
  x: T,
  y: number | BigNumber | bigint
): NoLiteralType<T>
```

### rightArithShift

Bitwise right arithmetic (sign-preserving) shift: `x >> y`.

```typescript { .api }
rightArithShift<T extends number | BigNumber | bigint | MathCollection>(
  x: T,
  y: number | BigNumber | bigint
): NoLiteralType<T>
```

### rightLogShift

Bitwise right logical (zero-filling) shift: `x >>> y`. Only supports `number` and `MathCollection` (not `BigNumber` or `bigint`).

```typescript { .api }
rightLogShift<T extends number | MathCollection>(
  x: T,
  y: number
): NoLiteralType<T>
```

---

## Logical Functions

All logical functions operate element-wise on arrays and matrices.

### and

Logical AND. Returns `true` when both `x` and `y` are defined with a non-zero / non-empty value.

```typescript { .api }
and(
  x: number | BigNumber | bigint | Complex | Unit | MathCollection,
  y: number | BigNumber | bigint | Complex | Unit | MathCollection
): boolean | MathCollection
```

### not

Logical NOT. Returns `true` when the input is zero or empty.

```typescript { .api }
not(
  x: number | BigNumber | bigint | Complex | Unit | MathCollection
): boolean | MathCollection
```

### or

Logical OR. Returns `true` when at least one of `x` or `y` is defined with a non-zero / non-empty value.

```typescript { .api }
or(
  x: number | BigNumber | bigint | Complex | Unit | MathCollection,
  y: number | BigNumber | bigint | Complex | Unit | MathCollection
): boolean | MathCollection
```

### xor

Logical XOR. Returns `true` when exactly one of `x` or `y` is defined with a non-zero / non-empty value.

```typescript { .api }
xor(
  x: number | BigNumber | bigint | Complex | Unit | MathCollection,
  y: number | BigNumber | bigint | Complex | Unit | MathCollection
): boolean | MathCollection
```

### nullish

Nullish coalescing operator (`??`). Returns `y` (the right-hand operand) when `x` is `null` or `undefined`; otherwise returns `x`. Unlike `or`, a falsy but non-null value (e.g. `0`, `false`, `''`) returns `x`. For matrices, evaluated element-wise.

```typescript { .api }
nullish(x: any, y: any): any
```

**Example:**

```typescript { .api }
math.nullish(null, 42)       // returns 42
math.nullish(undefined, 42)  // returns 42
math.nullish(0, 42)          // returns 0   (0 is not null/undefined)
math.nullish(false, 42)      // returns false
```

---

## Complex Number Functions

### arg

Compute the argument (angle) of a complex value. For a complex number `a + bi`, the argument is `atan2(b, a)`. For matrices, the function is evaluated element wise.

```typescript { .api }
// number or Complex input returns number
arg(x: number | Complex): number

// BigNumber or Complex input returns BigNumber
arg(x: BigNumber | Complex): BigNumber

// Collection input returns collection of matching type
arg<T extends MathCollection>(x: T): T
```

### conj

Compute the complex conjugate of a complex value. If `x = a + bi`, the complex conjugate is `a - bi`. For matrices, the function is evaluated element wise.

```typescript { .api }
conj<T extends number | BigNumber | Complex | MathCollection>(
  x: T
): NoLiteralType<T>
```

### im

Get the imaginary part of a complex number. For a complex number `a + bi`, the function returns `b`. For matrices, the function is evaluated element wise.

```typescript { .api }
// number or Complex input returns number
im(x: number | Complex): number

// BigNumber or MathCollection input returns matching type
im<T extends BigNumber | MathCollection>(x: T): T
```

### re

Get the real part of a complex number. For a complex number `a + bi`, the function returns `a`. For matrices, the function is evaluated element wise.

```typescript { .api }
// number or Complex input returns number
re(x: number | Complex): number

// BigNumber or MathCollection input returns matching type
re<T extends BigNumber | MathCollection>(x: T): T
```
