# Units

Physical units with automatic conversions and arithmetic operations. Math.js includes a comprehensive unit system covering length, mass, time, temperature, current, luminosity, and derived units.

## Capabilities

### Unit Creation

Create units from values and unit strings.

```javascript { .api }
/**
 * Create a unit
 * @param value - Numeric value (optional for valueless units)
 * @param unit - Unit string (e.g., 'cm', 'm/s', 'kg m/s^2')
 * @returns Unit object
 */
function unit(value?: number | string, unit?: string): Unit
function unit(unit: string): Unit

/**
 * Create a custom unit
 * @param name - Unit name
 * @param definition - Unit definition (string or object)
 * @param options - Additional options
 * @returns Unit constructor
 */
function createUnit(
  name: string,
  definition?: string | UnitDefinition,
  options?: object
): Unit

/**
 * Split a unit into multiple parts
 * @param unit - Unit to split
 * @param parts - Array of unit strings
 * @returns Array of units
 */
function splitUnit(unit: Unit, parts: string[]): Unit[]
```

**Usage Examples:**

```javascript
import { unit, createUnit, splitUnit } from 'mathjs'

// Create units
unit(5, 'cm')                     // 5 cm
unit('10 kg')                     // 10 kg
unit('5.5 m/s')                   // 5.5 m/s
unit('9.81 m/s^2')                // 9.81 m/s^2

// Valueless units (for conversion)
unit('inch')                      // inch (no value)

// Create custom units
createUnit('byte')
createUnit('kilobyte', '1024 byte')
createUnit('lightyear', '9.461e15 m')

// Split compound units
splitUnit(unit('1.75 m'), ['ft', 'inch'])  // [5 ft, 9 inch]
splitUnit(unit('3700 s'), ['hour', 'minute', 'second'])  // [1 hour, 1 minute, 40 second]
```

### Unit Conversion

Convert between compatible units.

```javascript { .api }
/**
 * Convert a unit to another unit
 * @param x - Unit to convert
 * @param unit - Target unit
 * @returns Converted unit
 */
function to(x: Unit, unit: string | Unit): Unit

/**
 * Convert to the best unit prefix
 * @param units - Units to try (optional)
 * @param options - Conversion options
 * @returns Unit with optimal prefix
 */
function toBest(units?: Unit[], options?: object): Unit
```

**Usage Examples:**

```javascript
import { unit, evaluate } from 'mathjs'

// Basic conversion
unit('5 cm').to('inch')           // 1.9685 inch
unit('10 kg').to('lb')            // 22.046 lbm
unit('100 degC').to('degF')       // 212 degF
unit('60 mph').to('m/s')          // 26.822 m/s

// Via evaluate
evaluate('5 cm to inch')          // 1.9685 inch
evaluate('10 km to mile')         // 6.214 mile

// Best unit selection
unit('5000 mm').toBest()          // 5 m
unit('0.000001 m').toBest()       // 1 μm
```

### Unit Arithmetic

Perform arithmetic operations with units.

```javascript { .api }
/**
 * Add units (must have compatible dimensions)
 * @param x - First unit
 * @param y - Second unit
 * @returns Sum
 */
function add(x: Unit, y: Unit): Unit

/**
 * Subtract units (must have compatible dimensions)
 * @param x - First unit
 * @param y - Second unit
 * @returns Difference
 */
function subtract(x: Unit, y: Unit): Unit

/**
 * Multiply units
 * @param x - First unit
 * @param y - Second unit or scalar
 * @returns Product
 */
function multiply(x: Unit, y: Unit | MathNumericType): Unit

/**
 * Divide units
 * @param x - Numerator
 * @param y - Denominator
 * @returns Quotient
 */
function divide(x: Unit, y: Unit | MathNumericType): Unit

/**
 * Raise unit to a power
 * @param x - Base unit
 * @param y - Exponent
 * @returns Power
 */
function pow(x: Unit, y: number): Unit

/**
 * Square root of unit
 * @param x - Unit
 * @returns Square root
 */
function sqrt(x: Unit): Unit

/**
 * Absolute value of unit
 * @param x - Unit
 * @returns Absolute value
 */
function abs(x: Unit): Unit
```

**Usage Examples:**

```javascript
import { unit, add, subtract, multiply, divide, pow, sqrt } from 'mathjs'

// Addition/subtraction (automatic conversion)
add(unit('2 m'), unit('50 cm'))   // 2.5 m
subtract(unit('1 hour'), unit('30 min'))  // 0.5 hour

// Multiplication
multiply(unit('5 m'), unit('2 m')) // 10 m^2
multiply(unit('10 kg'), 2)         // 20 kg
multiply(unit('5 m'), unit('2 s')) // 10 m s

// Division
divide(unit('100 km'), unit('2 hour'))  // 50 km/hour
divide(unit('10 m'), 2)            // 5 m

// Power and roots
pow(unit('5 m'), 2)                // 25 m^2
pow(unit('8 m^3'), 1/3)            // 2 m
sqrt(unit('16 m^2'))               // 4 m
```

## Built-in Units

### Base Units

- **Length**: m (meter), inch, foot, yard, mile, mil
- **Mass**: g (gram), kg, ton, pound, ounce
- **Time**: s (second), min, hour, day, week, month, year
- **Current**: A (ampere)
- **Temperature**: K (kelvin), degC, degF, degR
- **Amount**: mol (mole)
- **Luminosity**: cd (candela)
- **Angle**: rad (radian), deg (degree), grad, cycle, arcsec, arcmin

### Derived Units

**Force and Energy:**
- N (newton = kg⋅m/s²)
- J (joule = N⋅m)
- cal (calorie)
- eV (electron volt)
- BTU (British thermal unit)

**Power:**
- W (watt = J/s)
- hp (horsepower)

**Pressure:**
- Pa (pascal = N/m²)
- bar, atm (atmosphere)
- psi (pounds per square inch)
- mmHg, torr

**Electricity:**
- V (volt)
- ohm (Ω)
- F (farad)
- H (henry)
- S (siemens)
- Wb (weber)
- T (tesla)

**Frequency:**
- Hz (hertz = 1/s)

**Data:**
- bit, byte
- B (byte)

### Prefixes

SI prefixes are supported for most units:

- Y (yotta, 10²⁴)
- Z (zetta, 10²¹)
- E (exa, 10¹⁸)
- P (peta, 10¹⁵)
- T (tera, 10¹²)
- G (giga, 10⁹)
- M (mega, 10⁶)
- k (kilo, 10³)
- h (hecto, 10²)
- da (deca, 10¹)
- d (deci, 10⁻¹)
- c (centi, 10⁻²)
- m (milli, 10⁻³)
- μ (micro, 10⁻⁶)
- n (nano, 10⁻⁹)
- p (pico, 10⁻¹²)
- f (femto, 10⁻¹⁵)
- a (atto, 10⁻¹⁸)
- z (zepto, 10⁻²¹)
- y (yocto, 10⁻²⁴)

**Usage Examples:**

```javascript
import { unit } from 'mathjs'

unit('5 km')                      // 5 kilometer
unit('100 MHz')                   // 100 megahertz
unit('1 GB')                      // 1 gigabyte (binary: 1024³ bytes)
unit('50 μA')                     // 50 microampere
unit('3.5 ns')                    // 3.5 nanosecond
```

## Unit Interface

```javascript { .api }
interface Unit {
  /**
   * Numeric value (can be number, BigNumber, Fraction, or Complex)
   */
  value: number | BigNumber | Fraction | Complex | null

  /**
   * Convert to another unit
   * @param unit - Target unit
   * @returns Converted unit
   */
  to(unit: string | Unit): Unit

  /**
   * Convert to base SI units
   * @returns Unit in base units
   */
  toSI(): Unit

  /**
   * Convert unit value to a number
   * @param unit - Optional target unit
   * @returns Numeric value
   */
  toNumber(unit?: string | Unit): number

  /**
   * Convert to string representation
   * @returns String representation
   */
  toString(): string

  /**
   * Format unit with options
   * @param options - Formatting options
   * @returns Formatted string
   */
  format(options?: object): string

  /**
   * Check if unit has a value
   * @returns True if unit has a value
   */
  hasValue(): boolean

  /**
   * Check if units are equal
   * @param other - Unit to compare
   * @returns True if equal
   */
  equals(other: Unit): boolean

  /**
   * Compare with another unit
   * @param other - Unit to compare
   * @returns -1, 0, or 1
   */
  compare(other: Unit): number
}
```

## Common Unit Conversions

### Length

```javascript
import { evaluate } from 'mathjs'

evaluate('1 m to cm')             // 100 cm
evaluate('1 mile to km')          // 1.609 km
evaluate('6 feet to m')           // 1.829 m
evaluate('1 inch to mm')          // 25.4 mm
evaluate('1 lightyear to m')      // 9.461e15 m
```

### Mass

```javascript
import { evaluate } from 'mathjs'

evaluate('1 kg to g')             // 1000 g
evaluate('1 lb to kg')            // 0.454 kg
evaluate('1 ton to kg')           // 1000 kg
evaluate('1 oz to g')             // 28.35 g
```

### Time

```javascript
import { evaluate } from 'mathjs'

evaluate('1 hour to s')           // 3600 s
evaluate('1 day to hour')         // 24 hour
evaluate('1 week to day')         // 7 day
evaluate('90 min to hour')        // 1.5 hour
```

### Temperature

```javascript
import { unit } from 'mathjs'

// Absolute temperature conversions
unit('273.15 K').to('degC')       // 0 degC
unit('0 degC').to('degF')         // 32 degF
unit('100 degC').to('K')          // 373.15 K

// Note: Temperature differences use different conversion
unit('10 degC').to('degF')        // For differences, not absolute temps
```

### Speed

```javascript
import { evaluate } from 'mathjs'

evaluate('100 km/hour to m/s')    // 27.78 m/s
evaluate('60 mph to km/hour')     // 96.56 km/hour
evaluate('343 m/s to mph')        // 767.3 mph (speed of sound)
```

### Energy and Power

```javascript
import { evaluate } from 'mathjs'

evaluate('1 kWh to J')            // 3.6e6 J
evaluate('1 cal to J')            // 4.184 J
evaluate('1 eV to J')             // 1.602e-19 J
evaluate('1 hp to W')             // 745.7 W
```

### Pressure

```javascript
import { evaluate } from 'mathjs'

evaluate('1 atm to Pa')           // 101325 Pa
evaluate('1 bar to Pa')           // 100000 Pa
evaluate('14.7 psi to atm')       // 1.0007 atm
evaluate('760 mmHg to atm')       // 1 atm
```

## Advanced Unit Usage

### Compound Units

Create complex compound units:

```javascript
import { unit, evaluate } from 'mathjs'

// Velocity
unit('60 mile/hour')              // 60 mile/hour
evaluate('9.81 m/s^2')            // 9.81 m/s^2 (acceleration)

// Force
evaluate('1 N to kg m/s^2')       // 1 kg m/s^2

// Energy
evaluate('100 W hour to J')       // 360000 J

// Density
unit('1000 kg/m^3')               // 1000 kg/m^3
```

### Custom Units

Define domain-specific units:

```javascript
import { createUnit, unit } from 'mathjs'

// Create custom unit
createUnit('furlong', '220 yard')
createUnit('fortnight', '14 day')

// Use custom units
unit('5 furlong').to('km')        // 1.006 km

// Speed in furlongs per fortnight
const speed = unit('60 mph')
speed.to('furlong/fortnight')     // 1451520 furlong/fortnight

// Bitcoin (custom currency unit)
createUnit('BTC')
createUnit('satoshi', '1e-8 BTC')
unit('1 BTC').to('satoshi')       // 1e8 satoshi
```

### Unit Dimensions

Check unit dimensions and compatibility:

```javascript
import { unit } from 'mathjs'

const a = unit('5 m')
const b = unit('10 kg')
const c = unit('500 cm')

a.equalBase(c)                    // true (both length)
a.equalBase(b)                    // false (different dimensions)

// This will work (same dimension)
a.to('cm')                        // 500 cm

// This will throw an error (incompatible)
// a.to('kg')                     // Error: Cannot convert
```

### Valueless Units

Use valueless units for conversion:

```javascript
import { unit, multiply } from 'mathjs'

// Create valueless unit for conversion
const converter = unit('inch')

// Convert multiple values
multiply(5, converter).to('cm')   // 12.7 cm
multiply(10, converter).to('cm')  // 25.4 cm

// Or use directly
const value = 5
unit(value, 'inch').to('cm')      // 12.7 cm
```
