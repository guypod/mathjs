# Constants

Math.js provides a comprehensive set of mathematical and physical constants. Mathematical constants are primitive JavaScript types (number, Complex, boolean, etc.), while physical constants return Unit objects that include both value and physical dimensions.

## Capabilities

### Mathematical Constants

Core mathematical constants used in computations.

```typescript { .api }
/**
 * Euler's number (base of natural logarithm)
 * Value: 2.718281828459045...
 */
const e: number;

/**
 * Pi (ratio of circle circumference to diameter)
 * Value: 3.141592653589793...
 */
const pi: number;

/**
 * Tau (2 * pi, full circle constant)
 * Value: 6.283185307179586...
 */
const tau: number;

/**
 * Golden ratio ((1 + sqrt(5)) / 2)
 * Value: 1.618033988749895...
 */
const phi: number;

/**
 * Imaginary unit (sqrt(-1))
 * Type: Complex number with re=0, im=1
 */
const i: Complex;

/**
 * Natural logarithm of 2
 * Value: 0.6931471805599453
 */
const LN2: number;

/**
 * Natural logarithm of 10
 * Value: 2.302585092994046
 */
const LN10: number;

/**
 * Base-2 logarithm of e
 * Value: 1.4426950408889634
 */
const LOG2E: number;

/**
 * Base-10 logarithm of e
 * Value: 0.4342944819032518
 */
const LOG10E: number;

/**
 * Square root of 1/2
 * Value: 0.7071067811865476
 */
const SQRT1_2: number;

/**
 * Square root of 2
 * Value: 1.4142135623730951
 */
const SQRT2: number;

/**
 * Library version string
 * Format: "MAJOR.MINOR.PATCH"
 * Example: "15.1.0"
 */
const version: string;

/**
 * Positive infinity
 */
const Infinity: number;

/**
 * Not a Number value
 */
const NaN: number;

/**
 * Boolean true value
 */
const true: boolean;

/**
 * Boolean false value
 */
const false: boolean;

/**
 * Null value
 */
const null: null;

/**
 * Library version string (e.g., "15.1.0")
 */
const version: string;
```

#### Deprecated Mathematical Constants

```typescript { .api }
/**
 * Deprecated alias for pi
 * @deprecated Use `pi` instead
 */
const PI: number;

/**
 * Deprecated alias for e
 * @deprecated Use `e` instead
 */
const E: number;
```

### Physical Constants

Physical constants representing fundamental values in physics. All physical constants return Unit objects containing both the numeric value and physical dimensions.

#### Atomic and Particle Constants

```typescript { .api }
/**
 * Atomic mass constant (unified atomic mass unit)
 * Returns: Unit (approximately 1.66053906660e-27 kg)
 */
const atomicMass: Unit;

/**
 * Avogadro's number (number of particles per mole)
 * Returns: Unit (6.02214076e23 mol^-1)
 */
const avogadro: Unit;

/**
 * Electron mass
 * Returns: Unit (9.1093837015e-31 kg)
 */
const electronMass: Unit;

/**
 * Proton mass
 * Returns: Unit (1.67262192369e-27 kg)
 */
const protonMass: Unit;

/**
 * Neutron mass
 * Returns: Unit (1.67492749804e-27 kg)
 */
const neutronMass: Unit;

/**
 * Deuteron mass (deuterium nucleus)
 * Returns: Unit (3.3435837724e-27 kg)
 */
const deuteronMass: Unit;

/**
 * Elementary charge (charge of a proton)
 * Returns: Unit (1.602176634e-19 C)
 */
const elementaryCharge: Unit;

/**
 * Molar mass constant
 * Returns: Unit (0.99999999965e-3 kg/mol)
 */
const molarMass: Unit;
```

#### Electromagnetic Constants

```typescript { .api }
/**
 * Bohr magneton (magnetic moment of electron)
 * Returns: Unit (9.2740100783e-24 J/T)
 */
const bohrMagneton: Unit;

/**
 * Nuclear magneton
 * Returns: Unit (5.0507837461e-27 J/T)
 */
const nuclearMagneton: Unit;

/**
 * Conductance quantum (2e^2/h)
 * Returns: Unit (7.748091729e-5 S)
 */
const conductanceQuantum: Unit;

/**
 * Inverse conductance quantum (h/2e^2)
 * Returns: Unit (12906.40372 ohm)
 */
const inverseConductanceQuantum: Unit;

/**
 * Coulomb's constant (1/(4*pi*epsilon_0))
 * Returns: Unit (8.9875517923e9 N·m^2/C^2)
 */
const coulomb: Unit;

/**
 * Electric constant (vacuum permittivity, epsilon_0)
 * Returns: Unit (8.8541878128e-12 F/m)
 */
const electricConstant: Unit;

/**
 * Magnetic constant (vacuum permeability, mu_0)
 * Returns: Unit (1.25663706212e-6 N/A^2)
 */
const magneticConstant: Unit;

/**
 * Magnetic flux quantum (h/2e)
 * Returns: Unit (2.067833848e-15 Wb)
 */
const magneticFluxQuantum: Unit;

/**
 * Von Klitzing constant (h/e^2)
 * Returns: Unit (25812.80745 ohm)
 */
const klitzing: Unit;

/**
 * Vacuum impedance (mu_0*c)
 * Returns: Unit (376.730313668 ohm)
 */
const vacuumImpedance: Unit;
```

#### Quantum Mechanics Constants

```typescript { .api }
/**
 * Planck constant
 * Returns: Unit (6.62607015e-34 J·s)
 */
const planckConstant: Unit;

/**
 * Reduced Planck constant (h-bar, h/(2*pi))
 * Returns: Unit (1.054571817e-34 J·s)
 */
const reducedPlanckConstant: Unit;

/**
 * Molar Planck constant
 * Returns: Unit (3.990312712e-10 J·s/mol)
 */
const molarPlanckConstant: Unit;

/**
 * Bohr radius (most probable electron-nucleus distance in hydrogen)
 * Returns: Unit (5.29177210903e-11 m)
 */
const bohrRadius: Unit;

/**
 * Classical electron radius
 * Returns: Unit (2.8179403262e-15 m)
 */
const classicalElectronRadius: Unit;

/**
 * Hartree energy (atomic unit of energy)
 * Returns: Unit (4.3597447222071e-18 J)
 */
const hartreeEnergy: Unit;

/**
 * Quantum of circulation (h/(2*m_e))
 * Returns: Unit (3.6369475516e-4 m^2/s)
 */
const quantumOfCirculation: Unit;

/**
 * Rydberg constant (wavelength of hydrogen spectral lines)
 * Returns: Unit (10973731.568160 m^-1)
 */
const rydberg: Unit;

/**
 * Thomson cross section (classical electron scattering)
 * Returns: Unit (6.6524587321e-29 m^2)
 */
const thomsonCrossSection: Unit;
```

#### Thermodynamics Constants

```typescript { .api }
/**
 * Boltzmann constant (relates temperature to energy)
 * Returns: Unit (1.380649e-23 J/K)
 */
const boltzmann: Unit;

/**
 * Stefan-Boltzmann constant (blackbody radiation)
 * Returns: Unit (5.670374419e-8 W/(m^2·K^4))
 */
const stefanBoltzmann: Unit;

/**
 * Gas constant (universal gas constant, R)
 * Returns: Unit (8.314462618 J/(mol·K))
 */
const gasConstant: Unit;

/**
 * Molar volume of ideal gas at STP
 * Returns: Unit (0.02271095464 m^3/mol)
 */
const molarVolume: Unit;

/**
 * Sackur-Tetrode constant (entropy of ideal gas)
 * Returns: Unit (-1.15170753706)
 */
const sackurTetrode: Unit;
```

#### Radiation and Light Constants

```typescript { .api }
/**
 * Speed of light in vacuum
 * Returns: Unit (299792458 m/s)
 */
const speedOfLight: Unit;

/**
 * First radiation constant (2*pi*h*c^2)
 * Returns: Unit (3.741771852e-16 W·m^2)
 */
const firstRadiation: Unit;

/**
 * Second radiation constant (h*c/k)
 * Returns: Unit (0.01438776877 m·K)
 */
const secondRadiation: Unit;

/**
 * Wien displacement constant (peak wavelength in blackbody spectrum)
 * Returns: Unit (2.897771955e-3 m·K)
 */
const wienDisplacement: Unit;

/**
 * Loschmidt constant (number density at STP)
 * Returns: Unit (2.686780111e25 m^-3)
 */
const loschmidt: Unit;
```

#### Fundamental Physics Constants

```typescript { .api }
/**
 * Gravitational constant (Newton's constant, G)
 * Returns: Unit (6.67430e-11 m^3/(kg·s^2))
 */
const gravitationConstant: Unit;

/**
 * Standard gravity (acceleration due to gravity on Earth)
 * Returns: Unit (9.80665 m/s^2)
 */
const gravity: Unit;

/**
 * Fine structure constant (fundamental dimensionless constant)
 * Returns: Unit (0.0072973525693)
 */
const fineStructure: Unit;

/**
 * Fermi coupling constant (weak force)
 * Returns: Unit (1.1663787e-5 GeV^-2)
 */
const fermiCoupling: Unit;

/**
 * Weak mixing angle (Weinberg angle)
 * Returns: Unit (0.22290)
 */
const weakMixingAngle: Unit;

/**
 * Efimov factor (three-body quantum system)
 * Returns: Unit (22.7)
 */
const efimovFactor: Unit;
```

#### Planck Units

Natural units based on fundamental constants.

```typescript { .api }
/**
 * Planck length (sqrt(h-bar*G/c^3))
 * Returns: Unit (1.616255e-35 m)
 */
const planckLength: Unit;

/**
 * Planck mass (sqrt(h-bar*c/G))
 * Returns: Unit (2.176434e-8 kg)
 */
const planckMass: Unit;

/**
 * Planck time (sqrt(h-bar*G/c^5))
 * Returns: Unit (5.391247e-44 s)
 */
const planckTime: Unit;

/**
 * Planck charge (sqrt(4*pi*epsilon_0*h-bar*c))
 * Returns: Unit (1.87554603778e-18 C)
 */
const planckCharge: Unit;

/**
 * Planck temperature (sqrt(h-bar*c^5/(G*k^2)))
 * Returns: Unit (1.416784e32 K)
 */
const planckTemperature: Unit;
```

#### Derived Constants

```typescript { .api }
/**
 * Faraday constant (charge per mole of electrons)
 * Returns: Unit (96485.33212 C/mol)
 */
const faraday: Unit;
```

## Usage Examples

### Mathematical Constants

```javascript
import { pi, e, tau, phi, i, sqrt } from 'mathjs';

// Using pi in calculations
const circumference = 2 * pi * 5;  // 31.41592653589793

// Using e for exponential calculations
const expValue = Math.pow(e, 2);   // 7.38905609893065

// Using tau instead of 2*pi
const fullCircle = tau;             // 6.283185307179586

// Using golden ratio
const goldenRectangle = phi * 100;  // 161.8033988749895

// Using imaginary unit
const complex1 = i;                 // Complex {re: 0, im: 1}
const complex2 = sqrt(-1);          // Complex {re: 0, im: 1}
```

### Physical Constants

```javascript
import { speedOfLight, planckConstant, boltzmann, avogadro, electronMass } from 'mathjs';

// Physical constants return Unit objects
console.log(speedOfLight);          // 299792458 m / s
console.log(planckConstant);        // 6.62607015e-34 J s
console.log(boltzmann);             // 1.380649e-23 J / K

// Use in calculations with automatic unit handling
const energy = planckConstant.multiply(speedOfLight);
console.log(energy);                // Energy in J·m

// Access numeric value
const cValue = speedOfLight.toNumber('m/s');  // 299792458

// Number of atoms in 2 moles
const atoms = avogadro.multiply(2);  // 1.204428152e24 / mol
```

### Constants in Expressions

```javascript
import { evaluate } from 'mathjs';

// Mathematical constants in expressions
evaluate('sin(pi/2)');              // 1
evaluate('e^2');                    // 7.38905609893065
evaluate('tau / 2');                // 3.141592653589793

// Physical constants in expressions (use in unit conversions)
evaluate('speedOfLight to km/s');   // 299792.458 km / s
evaluate('electronMass in amu');    // Electron mass in atomic mass units
```

### Combining Constants

```javascript
import { pi, e, i, complex, multiply, add, pow } from 'mathjs';

// Euler's identity: e^(i*pi) + 1 = 0
const eulerIdentity = add(pow(e, multiply(i, pi)), 1);
console.log(eulerIdentity);         // ~0 (within floating point precision)

// Using multiple physical constants
import { electricConstant, coulomb } from 'mathjs';
const relation = multiply(coulomb, multiply(4, multiply(pi, electricConstant)));
console.log(relation);              // Should be close to 1
```

## Types

```typescript { .api }
/**
 * Unit type representing a value with physical dimensions
 */
interface Unit {
  /** Numeric value */
  value: number | BigNumber | Fraction | Complex;

  /** Unit dimensions and powers */
  units: Record<string, number>;

  /** Convert to target unit */
  to(targetUnit: string): Unit;

  /** Get numeric value in specified unit */
  toNumber(unit?: string): number;

  /** Get string representation */
  toString(): string;

  /** Format with options */
  format(options?: Object): string;

  /** Multiply with another value */
  multiply(value: number | Unit): Unit;

  /** Divide by another value */
  divide(value: number | Unit): Unit;
}

/**
 * Complex number type
 */
interface Complex {
  /** Real part */
  re: number;

  /** Imaginary part */
  im: number;

  /** Convert to string */
  toString(): string;

  /** Convert to polar coordinates */
  toPolar(): { r: number; phi: number };
}
```

## Notes

1. **Mathematical Constants**: All mathematical constants (e, pi, tau, phi, LN2, etc.) are standard JavaScript number primitives, except for `i` which is a Complex type.

2. **Physical Constants**: All physical constants return Unit objects, which include both the numeric value and the physical dimensions. This allows automatic unit checking and conversion.

3. **Precision**: Mathematical constants use JavaScript's standard double-precision floating-point format. For higher precision, use BigNumber with `bignumber` function.

4. **Immutability**: Constants are immutable values. Operations on constants return new values without modifying the original.

5. **Unit Arithmetic**: Physical constants can be used in arithmetic operations with automatic unit handling. Math.js will perform unit conversions and dimensional analysis.

6. **Expression Evaluation**: All constants can be used by name in string expressions passed to `evaluate()`, `parse()`, or `compile()`.

7. **Deprecated Constants**: Use lowercase `pi` and `e` instead of uppercase `PI` and `E` for consistency with JavaScript's Math object conventions.

8. **Version String**: The `version` constant returns the current library version as a string (e.g., "15.1.0").
