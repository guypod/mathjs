# mathjs Constants

mathjs v15.1.0 ships with a comprehensive set of built-in constants available as properties on any `MathJsInstance`. Mathematical constants automatically switch to `BigNumber` representation when `config.number === 'BigNumber'`. Physical constants are always of type `Unit` and represent CODATA/NIST values.

---

## Section 1 — Mathematical Constants

```typescript { .api }
// Euler's number
const e: number          // 2.718281828459045…

// NOTE: math.E is NOT exported as a direct property (math.E === undefined).
// Only lowercase math.e works. E is accessible via the expression parser: math.evaluate('E')

// Pi
const pi: number         // 3.141592653589793…

// NOTE: math.PI is NOT exported as a direct property (math.PI === undefined).
// Only lowercase math.pi works. PI is accessible via the expression parser: math.evaluate('PI')

// Tau — twice pi
const tau: number        // 6.283185307179586…

// Golden ratio (1 + √5) / 2
const phi: number        // 1.618033988749895…

// Imaginary unit
const i: Complex         // 0 + 1i

// Natural logarithm of 2
const LN2: number        // 0.6931471805599453…

// Natural logarithm of 10
const LN10: number       // 2.302585092994046…

// Base-2 logarithm of e
const LOG2E: number      // 1.4426950408889634…

// Base-10 logarithm of e
const LOG10E: number     // 0.4342944819032518…

// Square root of 1/2
const SQRT1_2: number    // 0.7071067811865476…

// Square root of 2
const SQRT2: number      // 1.4142135623730951…

// Positive infinity
// NOTE: math.Infinity is NOT accessible as a direct property (math.Infinity === undefined).
// Use the expression parser instead: math.evaluate('Infinity')  // returns Infinity

// Not a Number
// NOTE: math.NaN is NOT accessible as a direct property (math.NaN === undefined).
// Use the expression parser instead: math.evaluate('NaN')  // returns NaN

// Boolean true constant
const true: boolean      // true

// Boolean false constant
const false: boolean     // false

// Null constant
const null: null         // null
```

---

## Section 2 — Physical Constants

All physical constants are of type `Unit`. They represent CODATA/NIST recommended values.

```typescript { .api }
// Atomic mass constant (u)
const atomicMass: Unit

// Avogadro constant (mol⁻¹)
const avogadro: Unit

// Bohr magneton (J T⁻¹)
const bohrMagneton: Unit

// Bohr radius (m)
const bohrRadius: Unit

// Boltzmann constant (J K⁻¹)
const boltzmann: Unit

// Classical electron radius (m)
const classicalElectronRadius: Unit

// Conductance quantum (S)
const conductanceQuantum: Unit

// Coulomb's constant (N m² C⁻²)
const coulomb: Unit

// Deuteron mass (kg)
const deuteronMass: Unit

// Efimov factor (dimensionless)
const efimovFactor: Unit

// Electric constant / permittivity of free space (F m⁻¹)
const electricConstant: Unit

// Electron mass (kg)
const electronMass: Unit

// Elementary charge (C)
const elementaryCharge: Unit

// Faraday constant (C mol⁻¹)
const faraday: Unit

// Fermi coupling constant (GeV⁻²)
const fermiCoupling: Unit

// Fine-structure constant (dimensionless)
const fineStructure: Unit

// First radiation constant (W m²)
const firstRadiation: Unit

// Molar gas constant (J mol⁻¹ K⁻¹)
const gasConstant: Unit

// Gravitational constant (m³ kg⁻¹ s⁻²)
const gravitationConstant: Unit

// Standard gravity (m s⁻²)
const gravity: Unit

// Hartree energy (J)
const hartreeEnergy: Unit

// Inverse conductance quantum (Ω)
const inverseConductanceQuantum: Unit

// Von Klitzing constant (Ω)
const klitzing: Unit

// Loschmidt constant at 273.15 K and 101.325 kPa (m⁻³)
const loschmidt: Unit

// Magnetic constant / permeability of free space (N A⁻²)
const magneticConstant: Unit

// Magnetic flux quantum (Wb)
const magneticFluxQuantum: Unit

// Molar mass constant (kg mol⁻¹)
const molarMass: Unit

// Molar mass of carbon-12 (kg mol⁻¹)
const molarMassC12: Unit

// Molar Planck constant (J s mol⁻¹)
const molarPlanckConstant: Unit

// Molar volume of ideal gas at STP (m³ mol⁻¹)
const molarVolume: Unit

// Neutron mass (kg)
const neutronMass: Unit

// Nuclear magneton (J T⁻¹)
const nuclearMagneton: Unit

// Planck charge (C)
const planckCharge: Unit

// Planck constant (J s)
const planckConstant: Unit

// Planck length (m)
const planckLength: Unit

// Planck mass (kg)
const planckMass: Unit

// Planck temperature (K)
const planckTemperature: Unit

// Planck time (s)
const planckTime: Unit

// Proton mass (kg)
const protonMass: Unit

// Quantum of circulation (m² s⁻¹)
const quantumOfCirculation: Unit

// Reduced Planck constant ℏ (J s)
const reducedPlanckConstant: Unit

// Rydberg constant (m⁻¹)
const rydberg: Unit

// Sackur-Tetrode constant (dimensionless)
const sackurTetrode: Unit

// Second radiation constant (m K)
const secondRadiation: Unit

// Speed of light in vacuum (m s⁻¹)
const speedOfLight: Unit

// Stefan-Boltzmann constant (W m⁻² K⁻⁴)
const stefanBoltzmann: Unit

// Thomson cross section (m²)
const thomsonCrossSection: Unit

// Impedance of free space (Ω)
const vacuumImpedance: Unit

// Weak mixing angle (dimensionless)
const weakMixingAngle: Unit

// Wien displacement law constant (m K)
const wienDisplacement: Unit
```
