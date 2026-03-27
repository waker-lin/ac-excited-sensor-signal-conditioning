# 06 Key Parameters

## Scope

This document collects the key design parameters of the whole circuit and explains how they are chosen. It is intended to bridge the gap between schematic reading and full mathematical derivation.

The emphasis is on two levels:

- module-level design targets
- end-to-end gain, frequency, and output allocation

## 1. Parameter Overview Table

| Module | Parameter | Current Value / Target | Meaning |
| --- | --- | --- | --- |
| Sine-wave generator | Excitation amplitude | about 5.5 V | Drives the AC bridge and defines reference amplitude basis |
| Sine-wave generator | Excitation frequency | 5 kHz | Moves the measurement away from low-frequency drift and power-frequency interference |
| Full bridge | Gauge resistance | 350 ohm | Selected strain-gauge nominal resistance |
| Full bridge | Simulated resistance change | `Delta R = 0.44 ohm` equivalent scale in the report | Represents force-induced bridge imbalance |
| Full bridge | Zero residual after adjustment | less than 1 mV | Balancing target at no-load condition |
| High-CMRR amplifier | Front-end task | weak differential AC to usable level | Raises bridge output while suppressing common mode |
| Square-wave converter | Reference frequency | same as 5 kHz carrier | Keeps demodulation reference synchronized |
| Low-pass filter | Cutoff frequency | 100 Hz | Removes carrier-related residue after PSD |
| DC amplifier | Output target example | from about -144 mV to about 2 V | Final scaling for readable output |

## 2. Excitation Source Selection

### 2.1 Why AC Excitation Is Used

The report explicitly states that power-frequency interference in the low-frequency range can cause zero drift. For that reason, the sensing chain is excited by an AC carrier rather than being measured as a direct DC bridge output.

The design logic is:

- useful signal is modulated onto a higher-frequency carrier
- low-frequency drift and DC offset become less dominant
- synchronous demodulation can later recover the signed measurement term

### 2.2 Frequency Choice

The current implementation chooses:

`f_exc = 5 kHz`

This is consistent with the design idea that the excitation should be much higher than the disturbance band around mains-frequency interference.

### 2.3 Wien-Bridge / RC-Bridge Oscillator Frequency

For an equal-value RC bridge oscillator, the nominal oscillation frequency is:

`f_0 = 1 / (2 pi R C)`

This formula gives the starting point for component selection. In practical adjustment, the resistor values are then trimmed to push the actual oscillation frequency toward the 5 kHz target.

## 3. Full-Bridge Sensor Parameters

### 3.1 Bridge Resistance Basis

The current implementation uses four strain gauges with nominal resistance:

`R_g = 350 ohm`

This is the electrical basis of the full-bridge stage.

### 3.2 Resistance Variation

The report gives the small resistance variation relation:

`Delta R / R = K epsilon`

and states a calculated example:

`Delta R = 0.44 ohm`

This value is then used to represent the effect of force on the bridge.

### 3.3 Full-Bridge Output Interpretation

For a symmetric full bridge under small-signal conditions, the bridge output is proportional to excitation and normalized resistance variation:

`V_bridge ∝ V_exc * (Delta R / R)`

The exact coefficient depends on the active-arm arrangement, but the important engineering point is unchanged:

- larger excitation increases bridge output
- smaller imbalance produces millivolt-level signals
- zero balancing is essential before amplification

### 3.4 Balance And Compensation

Because AC bridge wiring introduces distributed capacitance, the zero point is not corrected by resistance adjustment alone. The current design therefore includes:

- resistive balance
- capacitive compensation

The reported target is:

`|V_zero| < 1 mV`

This is a critical parameter because any residual zero is later amplified and demodulated.

## 4. Front-End Amplifier Allocation

### 4.1 Purpose

The front-end amplifier must handle a weak differential signal under high common-mode interference conditions, especially when long leads connect the sensor to the processing circuit.

### 4.2 Gain Objective

The report indicates that the signal-processing chain needs to lift the signal on the order of:

`40 mV -> 4 V`

This means the overall conditioning path requires approximately:

`G_total ≈ 4 / 0.04 = 100`

This should not be interpreted as a single-stage gain requirement. In practice, the total gain is distributed across:

- differential front-end amplification
- synchronous detection conversion
- final DC amplification

### 4.3 Why Gain Distribution Matters

If too much gain is placed before demodulation:

- offset and common-mode residue become harder to control
- switching spikes in the PSD stage become more severe

If too little gain is placed before demodulation:

- useful signal becomes too small relative to detector errors

So the design problem is not just “how much gain,” but “where the gain should be applied.”

## 5. Square-Wave Reference Parameters

The square-wave reference must satisfy:

- same frequency as the carrier
- controlled phase relation to the carrier
- sufficient edge quality for stable switching

For this design:

`f_ref = f_exc = 5 kHz`

The most important parameter is not amplitude but timing, because PSD output scales with the phase relation between the signal and the reference.

## 6. Phase-Sensitive Demodulation Parameter Meaning

The key mathematical result is:

`V_out ∝ A(x)cos(phi)`

Therefore the demodulation stage is directly sensitive to:

- signal amplitude `A(x)`
- phase difference `phi`

This means the reference path must be treated as a design parameter source, not a passive helper block.

If:

- `phi = 0`, output is maximum positive
- `phi = pi`, output is maximum negative
- `phi = pi/2`, output ideally approaches zero

## 7. Low-Pass Filter Design Target

### 7.1 Cutoff Selection

The report chooses:

`f_c = 100 Hz`

This is much lower than the 5 kHz carrier and also much lower than the carrier-related residue after demodulation.

### 7.2 Why 100 Hz Is Reasonable

The low-pass filter must:

- preserve the slow measurement trend
- suppress switching ripple and carrier products

The design logic is:

- carrier frequency: 5 kHz
- low-pass cutoff: 100 Hz

so the filter strongly rejects the high-frequency residue while still allowing a slowly varying measurement output.

### 7.3 Chosen Topology

The report specifies a second-order MFB active low-pass filter. The purpose of choosing an active second-order filter is:

- stronger suppression than a single RC stage
- less signal loss than a passive-only implementation
- easier gain and frequency control

## 8. Final DC Amplifier Parameters

### 8.1 Input Level

The report gives one representative low-pass output level:

`V_LPF ≈ -144 mV`

### 8.2 Output Target

The final DC stage aims to scale this toward:

`V_final ≈ 2 V`

### 8.3 Required Gain

The report states the required gain is approximately:

`G_DC ≈ 13.9`

This is consistent with the ratio:

`|2 / 0.144| ≈ 13.9`

This stage therefore acts as the final output-range adaptation block rather than the primary information-extraction block.

## 9. Practical Parameter Checklist

When the repository is extended with schematic screenshots and measured data, each module should record these parameter fields explicitly:

- nominal value
- calculated value
- simulated value
- measured value
- deviation explanation

This will turn the repository into a proper circuit archive rather than a theory-only note set.

## 10. Suggested Next Additions

The next useful additions to this document are:

- exact oscillator resistor and capacitor values
- exact gain-setting resistor values of the three-op-amp stage
- exact MFB low-pass resistor-capacitor set
- final DC amplifier resistor ratio
- a single summary table comparing design, simulation, and measurement
