# Units

Math.js provides comprehensive support for physical units, enabling calculations with quantities that have dimensions (length, mass, time, etc.) and automatic unit conversion. Units can be combined, converted, and used in arithmetic operations while maintaining dimensional correctness.

## Core Imports

```javascript { .api }
import {
  unit, createUnit, to, toBest, splitUnit,
  add, subtract, multiply, divide, pow, sqrt
} from 'mathjs';
```

CommonJS:

```javascript
const {
  unit, createUnit, to, toBest, splitUnit,
  add, subtract, multiply, divide, pow, sqrt
} = require('mathjs');
```

## Capabilities

### Unit Creation

Create units with values and unit strings.

```typescript { .api }
/**
 * Create unit with value and unit string
 * @param value - Numeric value or string with value and unit
 * @param unit - Unit string (optional if value contains unit)
 * @returns Unit instance with specified value and unit
 */
function unit(value: number | string | BigNumber | Fraction | Complex | null, unit?: string | Unit): Unit;
```

**Parameters:**
- `value`: Numeric value (number, BigNumber, Fraction, Complex) or string containing both value and unit, or null for valueless unit
- `unit`: Unit string (e.g., 'm', 'kg', 'm/s') or Unit object (optional if value is a string)

**Returns:** Unit instance

**Usage Examples:**

```javascript
import { unit, bignumber, fraction } from 'mathjs';

// Basic unit creation with value and unit
unit(45, 'cm');                      // Unit 450 mm (auto-normalized)
unit(5, 'kg');                       // Unit 5 kg
unit(100, 'm/s');                    // Unit 100 m / s

// String notation with value and unit
unit('0.1 kilogram');                // Unit 100 gram
unit('2 inch');                      // Unit 2 inch
unit('90 km/h');                     // Unit 90 km / h
unit('101325 kg/(m s^2)');           // Unit 101325 kg / (m s^2)

// Valueless units (for conversion targets)
const kph = unit('km/h');            // Valueless Unit km / h
const mps = unit('m/s');             // Valueless Unit m / s

// Using valueless units as conversion targets
const speed = unit(36, kph);         // Unit 36 km / h
speed.toNumber(mps);                 // Number 10

// Units with different numeric types
unit(bignumber('9.81'), 'm/s^2');    // Unit with BigNumber value
unit(fraction(1, 3), 'm');           // Unit with Fraction value

// Compound units with multiple terms
unit('8.314 m^3 Pa / mol / K');      // Gas constant
unit('8.314 (m^3 Pa) / (mol K)');    // Same with explicit grouping
unit('1 N m');                       // Newton-meter (torque)
unit('60 mi/h');                     // Miles per hour

// Temperature units
unit(273.15, 'K');                   // Kelvin
unit(0, 'degC');                     // Celsius (converts to Kelvin internally)
unit(32, 'degF');                    // Fahrenheit

// Using prefixes
unit(1, 'km');                       // 1 kilometer = 1000 m
unit(500, 'mm');                     // 500 millimeters
unit(5, 'MHz');                      // 5 megahertz
unit(2, 'GB');                       // 2 gigabytes
```

**Important Notes:**

Be careful with implicit multiplication in compound units. Use explicit parentheses to avoid ambiguity:

```javascript
// Correct - with explicit division for each term
unit('8.314 m^3 Pa / mol / K');      // (m^3 Pa) / (mol K)

// Incorrect - missing second division
unit('8.314 m^3 Pa / mol K');        // (m^3 Pa K) / mol (wrong!)

// Better - use explicit grouping
unit('8.314 (m^3 * Pa) / (mol * K)');
```

### Unit Conversion

Convert units to different representations.

```typescript { .api }
/**
 * Convert value to specified unit
 * @param value - Value with unit to convert
 * @param unit - Target unit string or Unit object
 * @returns New Unit with value converted to target unit
 */
function to(value: Unit, unit: string | Unit): Unit;
```

**Parameters:**
- `value`: Unit object to convert
- `unit`: Target unit as string (e.g., 'cm', 'kg') or Unit object

**Returns:** New Unit converted to specified unit

**Usage Examples:**

```javascript
import { unit, to } from 'mathjs';

// Basic conversions
const distance = unit('2 inch');
to(distance, 'cm');                  // Unit 5.08 cm

// Method syntax (equivalent)
distance.to('cm');                   // Unit 5.08 cm

// Length conversions
to(unit('1 mile'), 'km');            // Unit 1.609344 km
to(unit('100 cm'), 'm');             // Unit 1 m
to(unit('5 ft'), 'inch');            // Unit 60 inch

// Mass conversions
to(unit('1 kg'), 'lbs');             // Unit 2.204622... lbs
to(unit('1 tonne'), 'kg');           // Unit 1000 kg

// Time conversions
to(unit('90 minutes'), 'hour');      // Unit 1.5 hour
to(unit('1 week'), 'day');           // Unit 7 day

// Speed conversions
to(unit('60 mph'), 'km/h');          // Unit 96.56064 km / h
to(unit('100 m/s'), 'km/h');         // Unit 360 km / h

// Temperature conversions
to(unit('0 degC'), 'K');             // Unit 273.15 K
to(unit('32 degF'), 'degC');         // Unit 0 degC

// Energy conversions
to(unit('1 kWh'), 'J');              // Unit 3600000 J
to(unit('1 cal'), 'J');              // Unit 4.184 J

// Convert to SI base units
unit('1 N').toSI();                  // Unit 1 (kg m) / s^2
unit('1 Pa').toSI();                 // Unit 1 kg / (m s^2)

// Get numeric value in target unit
unit('5 km').toNumber('m');          // Number 5000
unit('2.5 hour').toNumber('minute'); // Number 150

// Preserve numeric type during conversion
const u = unit(fraction(10), 'inch');
u.toNumeric('cm');                   // Fraction 127/5
```

### Best Unit Selection

Convert to the most appropriate unit from a list or automatically.

```typescript { .api }
/**
 * Convert to most appropriate display unit
 * @param value - Value with unit to convert
 * @param options - Options for conversion (optional)
 * @returns Unit converted to best representation for display
 */
function toBest(value: Unit, options?: { unitList?: (string | Unit)[] }): Unit;
```

**Parameters:**
- `value`: Unit object to convert
- `options`: Optional configuration object
  - `unitList`: Array of candidate units to choose from (if omitted, automatic selection)

**Returns:** Unit converted to most appropriate unit for display

**Usage Examples:**

```javascript
import { unit, toBest } from 'mathjs';

// Automatic best unit selection
toBest(unit('0.001 m'));             // Unit 1 mm (chooses better prefix)
toBest(unit('5000 m'));              // Unit 5 km
toBest(unit('3600 s'));              // Unit 1 hour

// Method syntax (equivalent)
unit('0.001 m').toBest();            // Unit 1 mm

// Select from specific unit list
const distance = unit('5280 ft');
toBest(distance, { unitList: ['ft', 'mile'] });
// Unit 1 mile (5280 ft = 1 mile)

// Choose most appropriate time unit
const time = unit('7200 s');
toBest(time, { unitList: ['s', 'minute', 'hour', 'day'] });
// Unit 2 hour

// Choose most appropriate length unit
const length = unit('254 cm');
toBest(length, { unitList: ['mm', 'cm', 'm', 'km'] });
// Unit 2.54 m

// Choose most appropriate data size unit
const data = unit('1500000 bytes');
toBest(data, { unitList: ['bytes', 'KB', 'MB', 'GB'] });
// Unit 1.5 MB

// Automatic selection finds best metric prefix
toBest(unit('0.000001 m'));          // Unit 1 um (micrometer)
toBest(unit('1000000 W'));           // Unit 1 MW (megawatt)
```

### Unit Splitting

Split a unit into multiple parts (e.g., feet and inches).

```typescript { .api }
/**
 * Split unit into specified parts
 * @param unit - Unit to split
 * @param parts - Array of unit strings to split into
 * @returns Array of Units representing the split parts
 */
function splitUnit(unit: Unit, parts: string[]): Unit[];
```

**Parameters:**
- `unit`: Unit object to split
- `parts`: Array of unit strings defining the split parts (from largest to smallest)

**Returns:** Array of Units, one for each part

**Usage Examples:**

```javascript
import { unit, splitUnit } from 'mathjs';

// Split meters into feet and inches
const distance = unit('1.5 m');
splitUnit(distance, ['ft', 'in']);
// [Unit 4 ft, Unit 11.055118110236232 in]

// Method syntax (equivalent)
distance.splitUnit(['ft', 'in']);
// [Unit 4 ft, Unit 11.055118110236232 in]

// Split time into hours, minutes, seconds
const duration = unit('7384 s');
splitUnit(duration, ['hour', 'minute', 'second']);
// [Unit 2 hour, Unit 3 minute, Unit 4 second]

// Split days into weeks and days
const days = unit('17 day');
splitUnit(days, ['week', 'day']);
// [Unit 2 week, Unit 3 day]

// Split large distance into km and m
const longDistance = unit('5432 m');
splitUnit(longDistance, ['km', 'm']);
// [Unit 5 km, Unit 432 m]

// Common use case: feet and inches
const height = unit('175 cm');
const [feet, inches] = splitUnit(height, ['ft', 'in']);
console.log(`${feet.toNumber('ft')}' ${inches.toNumber('in')}"`);
// "5' 8.897637795275591""

// Practical example: format duration
const timeInSeconds = unit('3665 s');
const [h, m, s] = splitUnit(timeInSeconds, ['hour', 'minute', 'second']);
console.log(`${h.toNumber('hour')}:${m.toNumber('minute')}:${s.toNumber('second')}`);
// "1:1:5"
```

### Custom Unit Definition

Create new user-defined units.

```typescript { .api }
/**
 * Define new custom unit
 * @param name - Name of new unit or object mapping names to definitions
 * @param definition - Definition as string, Unit, or config object (optional for base units)
 * @param options - Options for unit creation (optional)
 * @returns The created Unit
 */
function createUnit(
  name: string | Record<string, string | UnitConfig>,
  definition?: string | Unit | UnitConfig,
  options?: { override?: boolean }
): Unit;
```

**Parameters:**
- `name`: String name for new unit, or object mapping multiple unit names to definitions
- `definition`: Definition of unit (optional):
  - String: definition in terms of existing units (e.g., '220 yards')
  - Unit: unit object
  - UnitConfig object with properties:
    - `definition`: string or Unit defining the new unit
    - `prefixes`: 'none' | 'short' | 'long' | 'binary_short' | 'binary_long' (default: 'none')
    - `offset`: offset value for temperature-like scales (default: 0)
    - `aliases`: array of string aliases for the unit
    - `baseName`: name for new dimension if creating base unit
  - Omit to create a new base unit
- `options`: Options object (optional)
  - `override`: if true, allows redefining existing units (default: false)

**Returns:** The created Unit (or last unit if creating multiple)

**Usage Examples:**

```javascript
import { createUnit, evaluate, unit } from 'mathjs';

// Define simple unit based on existing unit
createUnit('furlong', '220 yards');
evaluate('1 mile to furlong');       // 8 furlong
unit(1, 'furlong').to('m');          // Unit 201.168 m

// Create a new base unit (no definition)
createUnit('foo');
evaluate('8 foo * 4 feet');          // 32 foo feet
unit(2, 'foo').toString();           // "2 foo"

// Define unit with configuration object
createUnit('knot', {
  definition: '0.514444 m/s',
  aliases: ['knots', 'kt', 'kts']
});
unit(10, 'knots').to('m/s');         // Unit 5.14444 m / s
unit(10, 'kt').to('km/h');           // Unit 18.51984 km / h

// Define unit with prefixes
createUnit('byte', {
  definition: '8 bit',
  prefixes: 'binary_short'            // enables Ki, Mi, Gi, etc.
});
unit(1, 'KiB').to('byte');           // Unit 1024 byte
unit(1, 'MiB').to('KiB');            // Unit 1024 KiB

// Define temperature scale with offset
createUnit('fahrenheit', {
  definition: '0.555556 kelvin',
  offset: 459.67
});

// Create multiple units at once
createUnit({
  widget: {
    prefixes: 'long',
    baseName: 'essence-of-widget'
  },
  gadget: '40 widget',
  gizmo: {
    definition: '1 gadget/hour',
    prefixes: 'long'
  }
});
evaluate('50000 kilowidget/s');      // Result in gigagizmos

// Override existing unit (use with caution)
createUnit('mile', '1609.347218694 m', { override: true });

// Use createUnit in expression evaluation
evaluate('45 mile/hour to createUnit("knot", "0.514444 m/s")');
// 39.103964668651976 knot

// Create custom physical unit
createUnit('parsec', '3.0857e16 m'); // astronomical unit
unit(1, 'parsec').to('lightyear');   // Convert to light years

// Create custom compound unit
createUnit('mph', 'mile/hour');
unit(60, 'mph').to('m/s');           // Unit 26.8224 m / s
```

## Unit Arithmetic

Units can be used with standard arithmetic operations. Operations maintain dimensional correctness and automatically handle unit conversion.

**Supported Operations:**
- Addition and subtraction: `add()`, `subtract()` - units must have compatible dimensions
- Multiplication and division: `multiply()`, `divide()` - creates compound units
- Powers and roots: `pow()`, `sqrt()`, `square()`, `cube()`
- Other: `abs()`, `sign()`
- Trigonometric: `sin()`, `cos()`, `tan()`, etc. when argument is an angle

**Usage Examples:**

```javascript
import { unit, add, subtract, multiply, divide, pow, cos } from 'mathjs';

// Addition - units must be compatible
const a = unit(45, 'cm');
const b = unit('0.1 m');
add(a, b);                           // Unit 0.55 m
add(unit('5 cm'), unit('2 cm'));     // Unit 7 cm

// Subtraction
subtract(unit('10 m'), unit('3 m')); // Unit 7 m
subtract(unit('1 hour'), unit('30 minute'));
                                     // Unit 0.5 hour

// Multiplication - creates compound units
multiply(unit('5 m'), unit('3 m'));  // Unit 15 m^2 (area)
multiply(unit('10 m'), 2);           // Unit 20 m (scalar multiply)
multiply(unit('60 km'), unit('2 h'));// Unit 120 km hour

// Division - creates ratios
divide(unit('100 km'), unit('2 hour'));
                                     // Unit 50 km / hour
divide(unit('10 m'), unit('2 s'));   // Unit 5 m / s
divide(unit('1 J'), unit('1 s'));    // Unit 1 W (joule/second = watt)

// Powers and roots
pow(unit('5 m'), 2);                 // Unit 25 m^2
pow(unit('2 cm'), 3);                // Unit 8 cm^3
sqrt(unit('25 m^2'));                // Unit 5 m

// Physics calculations
// Kinetic energy: KE = 0.5 * m * v^2
const velocity = unit('80 mi/h');
const mass = unit('2 tonne');
const ke = multiply(0.5, multiply(pow(velocity, 2), mass));
// Result in joules (convert to MJ for readability)

// Force calculation: F = m * a
const m = unit('10 kg');
const a = unit('9.8 m/s^2');
multiply(m, a);                      // Unit 98 (kg m) / s^2 = 98 N

// Trigonometric with angles
const angle = unit(45, 'deg');
cos(angle);                          // Number 0.7071067811865476
cos(unit(Math.PI/4, 'rad'));         // Number 0.7071067811865476

// Array operations with units
const forces = [unit('10 N'), unit('20 N'), unit('30 N')];
// Element-wise operations supported
```

**Important Notes for Temperature Units:**

Temperature units like `celsius` and `fahrenheit` represent temperature scales with arbitrary zero points, not absolute temperature. This causes unexpected behavior in arithmetic:

```javascript
// Problematic: multiplying celsius/fahrenheit
const temp = unit('14 degF');        // 14 degF = 263.15 K
multiply(temp, 2);                   // Unit 28 degF = 270.93 K (not 526.3 K!)

// Problematic: absolute value with offset scales
const negative = unit(-13, 'degF');  // -13 degF = 248.15 K
abs(negative);                       // Unit -13 degF (still 248.15 K!)

// Better: use kelvin or rankine for calculations
const tempK = unit(263.15, 'K');
multiply(tempK, 2);                  // Unit 526.3 K (correct!)
```

**Recommendation:** Use `kelvin` (K) or `rankine` (degR) for temperature calculations. Use `celsius` and `fahrenheit` only for display and input.

## Built-in Units

Math.js includes comprehensive built-in units across many domains.

### Unit Categories

**Length:** meter (m), inch (in), foot (ft), yard (yd), mile (mi), link (li), rod (rd), chain (ch), angstrom, mil

**Surface area:** m2, sqin, sqft, sqyd, sqmi, sqrd, sqch, sqmil, acre, hectare

**Volume:** m3, litre (l, L, lt, liter), cc, cuin, cuft, cuyd, teaspoon, tablespoon

**Liquid volume:** minim, fluiddram (fldr), fluidounce (floz), gill (gi), cup (cp), pint (pt), quart (qt), gallon (gal), beerbarrel (bbl), oilbarrel (obl), hogshead, drop (gtt)

**Angles:** rad (radian), deg (degree), grad (gradian), cycle, arcsec (arcsecond), arcmin (arcminute)

**Time:** second (s, secs, seconds), minute (min, mins, minutes), hour (h, hr, hrs, hours), day (days), week (weeks), month (months), year (years), decade (decades), century (centuries), millennium (millennia)

**Frequency:** hertz (Hz)

**Mass:** gram (g), tonne, ton, grain (gr), dram (dr), ounce (oz), poundmass (lbm, lb, lbs), hundredweight (cwt), stick, stone

**Electric current:** ampere (A)

**Temperature:** kelvin (K), celsius (degC), fahrenheit (degF), rankine (degR)

**Amount of substance:** mole (mol)

**Luminous intensity:** candela (cd)

**Force:** newton (N), dyne (dyn), poundforce (lbf), kip

**Energy:** joule (J), erg, Wh, BTU, electronvolt (eV)

**Power:** watt (W), hp

**Pressure:** Pa, psi, atm, torr, bar, mmHg, mmH2O, cmH2O

**Electricity and magnetism:** ampere (A), coulomb (C), watt (W), volt (V), ohm, farad (F), weber (Wb), tesla (T), henry (H), siemens (S), electronvolt (eV)

**Binary:** bits (b), bytes (B)

### Prefixes

**Decimal prefixes:**
- Large: deca (da, 1e1), hecto (h, 1e2), kilo (k, 1e3), mega (M, 1e6), giga (G, 1e9), tera (T, 1e12), peta (P, 1e15), exa (E, 1e18), zetta (Z, 1e21), yotta (Y, 1e24), ronna (R, 1e27), quetta (Q, 1e30)
- Small: deci (d, 1e-1), centi (c, 1e-2), milli (m, 1e-3), micro (u, 1e-6), nano (n, 1e-9), pico (p, 1e-12), femto (f, 1e-15), atto (a, 1e-18), zepto (z, 1e-21), yocto (y, 1e-24), ronto (r, 1e-27), quecto (q, 1e-30)

**Binary prefixes (for bits and bytes):**
- kibi (Ki, 1024), mebi (Mi, 1024²), gibi (Gi, 1024³), tebi (Ti, 1024⁴), pebi (Pi, 1024⁵), exi (Ei, 1024⁶), zebi (Zi, 1024⁷), yobi (Yi, 1024⁸)

**Usage Examples:**

```javascript
import { unit } from 'mathjs';

// Decimal prefixes
unit(1, 'km');                       // 1 kilometer = 1000 m
unit(500, 'mg');                     // 500 milligrams = 0.5 g
unit(2.5, 'GHz');                    // 2.5 gigahertz
unit(10, 'um');                      // 10 micrometers

// Binary prefixes
unit(1, 'KiB');                      // 1 kibibyte = 1024 bytes
unit(512, 'MiB').to('GiB');          // 0.5 GiB

// All units support plural forms
unit('5 meters');                    // Same as '5 meter'
unit('10 seconds');                  // Same as '10 second'

// Surface and volume can use power notation
unit('100 in^2');                    // Same as '100 sqin'
unit('5 m^3');                       // Same as '5 m3'
```

## Types

```typescript { .api }
/**
 * Value with physical unit
 */
class Unit {
  /** Numeric value (null for valueless units) */
  value: number | BigNumber | Fraction | Complex | null;

  /** Unit dimensions and prefixes */
  units: UnitDefinition[];

  /** Create unit */
  constructor(value: number | BigNumber | Fraction | Complex | null, unit: string);

  /** Clone this unit */
  clone(): Unit;

  /** Check if equal to another unit (same base and value) */
  equals(other: Unit): boolean;

  /** Check if this unit has the same base as another */
  equalBase(other: Unit): boolean;

  /** Convert to another unit */
  to(unit: string | Unit): Unit;

  /** Convert to SI base units */
  toSI(): Unit;

  /** Convert to number in specified unit */
  toNumber(unit?: string | Unit): number;

  /** Convert to numeric value (preserves type) */
  toNumeric(unit?: string | Unit): number | BigNumber | Fraction | Complex;

  /** Get string representation */
  toString(): string;

  /** Get LaTeX representation */
  toLatex(options?: FormatOptions): string;

  /** Get JSON representation */
  toJSON(): { mathjs: string; value: any; unit: string };

  /** Format with options */
  format(options?: FormatOptions): string;

  /** Simplify unit expression */
  simplify(): Unit;

  /** Split unit into parts */
  splitUnit(parts: string[]): Unit[];

  /** Absolute value */
  abs(): Unit;

  /** Addition */
  add(other: Unit): Unit;

  /** Subtraction */
  sub(other: Unit): Unit;

  /** Multiplication */
  mul(other: Unit | number | BigNumber | Fraction | Complex): Unit;

  /** Division */
  div(other: Unit | number | BigNumber | Fraction | Complex): Unit;

  /** Power */
  pow(p: number | BigNumber | Fraction): Unit;

  /** Square root */
  sqrt(): Unit;
}

/**
 * Unit configuration for createUnit
 */
interface UnitConfig {
  /** Definition in terms of existing units */
  definition?: string | Unit;

  /** Prefix mode: 'none', 'short', 'long', 'binary_short', 'binary_long' */
  prefixes?: string;

  /** Offset for temperature-like scales */
  offset?: number;

  /** Array of alias names for the unit */
  aliases?: string[];

  /** Name for new dimension if creating base unit */
  baseName?: string;
}

/**
 * Format options for unit display
 */
interface FormatOptions {
  /** Number notation: 'fixed', 'exponential', 'auto' */
  notation?: string;

  /** Number of significant digits */
  precision?: number;
}
```
