# Mathematical and Physical Constants

Math.js provides over 50 mathematical and physical constants for scientific computing. All constants are available as named exports and can be used in expressions.

## Capabilities

### Mathematical Constants

Fundamental mathematical constants.

```javascript { .api }
/**
 * Euler's number (base of natural logarithm)
 * e ≈ 2.718281828459045
 */
const e: number

/**
 * Pi (ratio of circle's circumference to diameter)
 * π ≈ 3.141592653589793
 */
const pi: number

/**
 * Tau (2π, full circle constant)
 * τ ≈ 6.283185307179586
 */
const tau: number

/**
 * Golden ratio
 * φ = (1 + √5) / 2 ≈ 1.618033988749895
 */
const phi: number

/**
 * Imaginary unit
 * i = √(-1)
 */
const i: Complex

/**
 * Positive infinity
 */
const Infinity: number

/**
 * Not a Number
 */
const NaN: number
```

**Usage Examples:**

```javascript
import { e, pi, tau, phi, i, evaluate } from 'mathjs'

// Euler's number
console.log(e)                  // 2.718281828459045
evaluate('e^2')                 // 7.389...

// Pi
console.log(pi)                 // 3.141592653589793
evaluate('sin(pi/2)')           // 1

// Tau (full circle)
console.log(tau)                // 6.283185307179586
evaluate('cos(tau)')            // 1

// Golden ratio
console.log(phi)                // 1.618033988749895
evaluate('phi^2 - phi - 1')     // 0 (golden ratio property)

// Imaginary unit
console.log(i)                  // Complex(0, 1)
evaluate('i^2')                 // -1
```

### JavaScript Math Constants

Constants from JavaScript's Math object.

```javascript { .api }
/**
 * Natural logarithm of 2
 * ln(2) ≈ 0.693
 */
const LN2: number

/**
 * Natural logarithm of 10
 * ln(10) ≈ 2.303
 */
const LN10: number

/**
 * Base-2 logarithm of e
 * log₂(e) ≈ 1.443
 */
const LOG2E: number

/**
 * Base-10 logarithm of e
 * log₁₀(e) ≈ 0.434
 */
const LOG10E: number

/**
 * Square root of 1/2
 * √(1/2) ≈ 0.707
 */
const SQRT1_2: number

/**
 * Square root of 2
 * √2 ≈ 1.414
 */
const SQRT2: number
```

**Usage Examples:**

```javascript
import { LN2, LN10, LOG2E, LOG10E, SQRT1_2, SQRT2, evaluate } from 'mathjs'

console.log(LN2)                // 0.6931471805599453
console.log(LN10)               // 2.302585092994046
console.log(LOG2E)              // 1.4426950408889634
console.log(LOG10E)             // 0.4342944819032518
console.log(SQRT1_2)            // 0.7071067811865476
console.log(SQRT2)              // 1.4142135623730951

// Use in calculations
evaluate('LN2 * log(8, 2)')     // ln(8) ≈ 2.079
```

## Physical Constants

### Universal Constants

Fundamental constants of nature.

```javascript { .api }
/**
 * Speed of light in vacuum
 * c = 299792458 m/s (exact)
 */
const speedOfLight: Unit

/**
 * Newtonian gravitational constant
 * G ≈ 6.674e-11 m³/(kg·s²)
 */
const gravitationConstant: Unit

/**
 * Planck constant
 * h ≈ 6.626e-34 J·s
 */
const planckConstant: Unit

/**
 * Reduced Planck constant (h-bar)
 * ℏ = h/(2π) ≈ 1.055e-34 J·s
 */
const reducedPlanckConstant: Unit
```

**Usage Examples:**

```javascript
import { speedOfLight, gravitationConstant, planckConstant, unit, evaluate } from 'mathjs'

// Speed of light
evaluate('speedOfLight to km/s')  // 299792.458 km/s

// Calculate relativistic energy: E = mc²
evaluate('1 kg * speedOfLight^2 to MJ')  // 89875517873.68 MJ

// Gravitational force: F = G*m1*m2/r²
evaluate('gravitationConstant * (100 kg * 100 kg) / (1 m)^2')  // Force in Newtons

// Planck energy
evaluate('planckConstant * speedOfLight / 500 nm to eV')  // Photon energy
```

### Electromagnetic Constants

Constants related to electromagnetism.

```javascript { .api }
/**
 * Vacuum magnetic permeability
 * μ₀ ≈ 1.257e-6 H/m
 */
const magneticConstant: Unit

/**
 * Vacuum electric permittivity
 * ε₀ ≈ 8.854e-12 F/m
 */
const electricConstant: Unit

/**
 * Characteristic impedance of vacuum
 * Z₀ ≈ 376.73 Ω
 */
const vacuumImpedance: Unit

/**
 * Coulomb's constant
 * k ≈ 8.988e9 N·m²/C²
 */
const coulomb: Unit

/**
 * Elementary charge (electron charge)
 * e ≈ 1.602e-19 C
 */
const elementaryCharge: Unit

/**
 * Bohr magneton
 * μB ≈ 9.274e-24 J/T
 */
const bohrMagneton: Unit

/**
 * Magnetic flux quantum
 * Φ₀ ≈ 2.068e-15 Wb
 */
const magneticFluxQuantum: Unit

/**
 * von Klitzing constant
 * RK ≈ 25812.807 Ω
 */
const klitzing: Unit

/**
 * Conductance quantum
 * G₀ ≈ 7.748e-5 S
 */
const conductanceQuantum: Unit

/**
 * Inverse conductance quantum
 * G₀⁻¹ ≈ 12906.403 Ω
 */
const inverseConductanceQuantum: Unit

/**
 * Josephson constant
 * KJ ≈ 4.836e14 Hz/V
 */
const josephson: Unit
```

### Atomic and Nuclear Constants

Constants for atomic and subatomic physics.

```javascript { .api }
/**
 * Bohr radius
 * a₀ ≈ 5.292e-11 m
 */
const bohrRadius: Unit

/**
 * Classical electron radius
 * re ≈ 2.818e-15 m
 */
const classicalElectronRadius: Unit

/**
 * Electron mass
 * me ≈ 9.109e-31 kg
 */
const electronMass: Unit

/**
 * Proton mass
 * mp ≈ 1.673e-27 kg
 */
const protonMass: Unit

/**
 * Neutron mass
 * mn ≈ 1.675e-27 kg
 */
const neutronMass: Unit

/**
 * Fine-structure constant
 * α ≈ 7.297e-3 (dimensionless)
 */
const fineStructure: number

/**
 * Rydberg constant
 * R∞ ≈ 1.097e7 m⁻¹
 */
const rydberg: Unit

/**
 * Hartree energy
 * Eh ≈ 4.360e-18 J
 */
const hartreeEnergy: Unit

/**
 * Nuclear magneton
 * μN ≈ 5.051e-27 J/T
 */
const nuclearMagneton: Unit

/**
 * Deuteron mass
 * md ≈ 3.344e-27 kg
 */
const deuteronMass: Unit

/**
 * Fermi coupling constant
 * GF ≈ 1.166e-5 GeV⁻²
 */
const fermiCoupling: Unit

/**
 * Weak mixing angle
 * θW ≈ 0.2229
 */
const weakMixingAngle: Unit

/**
 * Efimov factor
 * ≈ 22.694
 */
const efimovFactor: Unit

/**
 * Thomson cross section
 * σe ≈ 6.652e-29 m²
 */
const thomsonCrossSection: Unit

/**
 * Quantum of circulation
 * h/(2me) ≈ 3.637e-4 m²/s
 */
const quantumOfCirculation: Unit
```

**Usage Examples:**

```javascript
import { bohrRadius, electronMass, protonMass, rydberg, evaluate } from 'mathjs'

// Hydrogen atom ground state radius
console.log(bohrRadius)         // 5.29177210903e-11 m

// Mass ratio
evaluate('protonMass / electronMass')  // 1836.15... (proton/electron mass ratio)

// Rydberg formula for hydrogen spectral lines
evaluate('rydberg * (1/2^2 - 1/3^2) to 1/nm')  // Wavelength for Hα line
```

### Physico-chemical Constants

Constants for chemistry and thermodynamics.

```javascript { .api }
/**
 * Avogadro constant (particles per mole)
 * NA ≈ 6.022e23 mol⁻¹
 */
const avogadro: Unit

/**
 * Boltzmann constant
 * k ≈ 1.381e-23 J/K
 */
const boltzmann: Unit

/**
 * Faraday constant
 * F ≈ 96485.3 C/mol
 */
const faraday: Unit

/**
 * Molar gas constant
 * R ≈ 8.314 J/(mol·K)
 */
const gasConstant: Unit

/**
 * Molar volume of ideal gas (STP)
 * Vm ≈ 22.414 L/mol
 */
const molarVolume: Unit

/**
 * Stefan-Boltzmann constant
 * σ ≈ 5.670e-8 W/(m²·K⁴)
 */
const stefanBoltzmann: Unit

/**
 * Wien displacement law constant
 * b ≈ 2.898e-3 m·K
 */
const wienDisplacement: Unit

/**
 * Atomic mass constant
 * mu ≈ 1.661e-27 kg
 */
const atomicMass: Unit

/**
 * Loschmidt constant (STP)
 * n₀ ≈ 2.687e25 m⁻³
 */
const loschmidt: Unit

/**
 * First radiation constant
 * c₁ ≈ 3.742e-16 W·m²
 */
const firstRadiation: Unit

/**
 * Second radiation constant
 * c₂ ≈ 0.01439 m·K
 */
const secondRadiation: Unit

/**
 * Molar Planck constant
 * NAh ≈ 3.990e-10 J·s/mol
 */
const molarPlanckConstant: Unit

/**
 * Sackur-Tetrode constant (at 1 K, 100 kPa)
 * S₀/R ≈ -1.152
 */
const sackurTetrode: Unit
```

**Usage Examples:**

```javascript
import { avogadro, boltzmann, gasConstant, stefanBoltzmann, evaluate } from 'mathjs'

// Number of molecules in a mole
console.log(avogadro)           // 6.02214076e23 mol⁻¹

// Ideal gas law: PV = nRT
evaluate('gasConstant * 300 K * 1 mol to J')  // 2494.2 J

// Relate Boltzmann to gas constant
evaluate('gasConstant / avogadro to J/K')  // Boltzmann constant

// Stefan-Boltzmann law: radiant power
evaluate('stefanBoltzmann * (300 K)^4 * 1 m^2 to W')  // 459.3 W

// Thermal energy at room temperature
evaluate('boltzmann * 300 K to eV')  // 0.0259 eV
```

### Standard Values

Adopted standard values.

```javascript { .api }
/**
 * Standard gravity (acceleration)
 * g ≈ 9.80665 m/s²
 */
const gravity: Unit

/**
 * Molar mass constant
 * Mu ≈ 1 g/mol
 */
const molarMass: Unit

/**
 * Molar mass of carbon-12
 * M(¹²C) = 12 g/mol (exact)
 */
const molarMassC12: Unit
```

**Usage Examples:**

```javascript
import { gravity, evaluate } from 'mathjs'

// Standard gravity
console.log(gravity)            // 9.80665 m/s²

// Gravitational potential energy: mgh
evaluate('10 kg * gravity * 5 m to J')  // 490.3 J

// Free fall velocity: v = √(2gh)
evaluate('sqrt(2 * gravity * 10 m) to m/s')  // 14.0 m/s
```

### Planck Units

Natural units based on fundamental constants.

```javascript { .api }
/**
 * Planck length
 * lP ≈ 1.616e-35 m
 */
const planckLength: Unit

/**
 * Planck mass
 * mP ≈ 2.176e-8 kg
 */
const planckMass: Unit

/**
 * Planck time
 * tP ≈ 5.391e-44 s
 */
const planckTime: Unit

/**
 * Planck charge
 * qP ≈ 1.876e-18 C
 */
const planckCharge: Unit

/**
 * Planck temperature
 * TP ≈ 1.417e32 K
 */
const planckTemperature: Unit
```

**Usage Examples:**

```javascript
import { planckLength, planckTime, speedOfLight, evaluate } from 'mathjs'

// Planck scales represent quantum gravity scale
console.log(planckLength)       // 1.616255e-35 m

// Verify relationship: lP = c * tP
evaluate('speedOfLight * planckTime to m')  // Approximately planckLength

// Planck energy density
evaluate('planckMass * speedOfLight^2 / planckLength^3 to J/m^3')
```

## Using Constants in Expressions

All constants can be used directly in string expressions:

```javascript
import { evaluate } from 'mathjs'

// Mathematical constants
evaluate('e^(i * pi)')          // -1 (Euler's identity)
evaluate('sin(tau/4)')          // 1

// Physical constants
evaluate('speedOfLight * 1 hour to km')  // 1079252848.8 km
evaluate('planckConstant * speedOfLight / 500 nm to eV')  // Photon energy

// Combined calculations
evaluate('electronMass * speedOfLight^2 to MeV')  // Rest energy of electron
evaluate('boltzmann * 300 K to eV')  // Thermal energy at room temp
```

## Unit Conversion with Constants

Many physical constants are Unit types and support conversions:

```javascript
import { speedOfLight, electronMass, boltzmann, evaluate } from 'mathjs'

// Convert to different units
evaluate('speedOfLight to km/s')               // 299792.458 km/s
evaluate('speedOfLight to c')                  // 1 c (speed of light)

evaluate('electronMass to MeV/c^2')            // 0.511 MeV/c²

evaluate('boltzmann to eV/K')                  // 8.617e-5 eV/K
evaluate('boltzmann * 300 K to meV')           // 25.9 meV
```

## Precision Considerations

Constants are stored as JavaScript numbers with ~15-16 significant digits:

```javascript
import { pi, e, bignumber, config } from 'mathjs'

// Standard precision
console.log(pi)                 // 3.141592653589793

// High precision with BigNumber
config({ number: 'BigNumber', precision: 64 })
const piHigh = bignumber('3.1415926535897932384626433832795028841971693993751')

// Physical constants maintain SI precision
// Values match CODATA 2018 recommendations
```

## Constant Categories Summary

| Category | Count | Examples |
|----------|-------|----------|
| Mathematical | 7 | e, pi, tau, phi, i |
| JavaScript Math | 6 | LN2, SQRT2, LOG10E |
| Universal | 4 | speedOfLight, gravitationConstant, planckConstant |
| Electromagnetic | 12 | elementaryCharge, magneticConstant, coulomb |
| Atomic/Nuclear | 15 | electronMass, protonMass, bohrRadius, rydberg |
| Physico-chemical | 13 | avogadro, boltzmann, gasConstant, faraday |
| Standard Values | 3 | gravity, molarMass |
| Planck Units | 5 | planckLength, planckMass, planckTime |

## Common Physics Calculations

### Particle Physics

```javascript
import { evaluate } from 'mathjs'

// Electron rest energy
evaluate('electronMass * speedOfLight^2 to MeV')  // 0.511 MeV

// Compton wavelength
evaluate('planckConstant / (electronMass * speedOfLight) to pm')  // 2.426 pm

// Fine structure constant
evaluate('elementaryCharge^2 / (4 * pi * electricConstant * reducedPlanckConstant * speedOfLight)')  // ~1/137
```

### Thermodynamics

```javascript
import { evaluate } from 'mathjs'

// Thermal energy at room temperature
evaluate('boltzmann * 300 K to eV')  // 0.0259 eV

// Blackbody radiation
evaluate('stefanBoltzmann * (5778 K)^4 * pi * (6.96e8 m)^2 to W')  // Solar luminosity

// Wien's law for peak wavelength
evaluate('wienDisplacement / 5778 K to nm')  // 501 nm (Sun's peak)
```

### Quantum Mechanics

```javascript
import { evaluate } from 'mathjs'

// De Broglie wavelength of electron at 1 keV
evaluate('planckConstant / sqrt(2 * electronMass * 1 keV) to pm')  // 38.8 pm

// Heisenberg uncertainty: ΔE·Δt ≥ ℏ/2
evaluate('reducedPlanckConstant / 2 to eV fs')  // 0.329 eV·fs

// Bohr model energy levels
evaluate('hartreeEnergy / 2^2 to eV')  // -3.40 eV (n=2 hydrogen level)
```

### Astronomy

```javascript
import { evaluate } from 'mathjs'

// Schwarzschild radius of Earth
evaluate('2 * gravitationConstant * 5.972e24 kg / speedOfLight^2 to mm')  // 8.87 mm

// Escape velocity from Earth
evaluate('sqrt(2 * gravitationConstant * 5.972e24 kg / 6.371e6 m) to km/s')  // 11.2 km/s
```
