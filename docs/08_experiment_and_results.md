# 08 Experiment and Results

## Scope

This document is intended to collect both simulation results and practical measurements for each module, and then summarize the overall circuit performance. The goal is not only to show pictures, but to explain whether the circuit behaves as expected.

The final form of this document should answer two questions:

1. Does each module work according to its design target?
2. Does the full signal chain produce the expected final output?

## Extracted Evidence

A first batch of report evidence has already been extracted into the repository:

- schematics: `images/module_figures/`
- simulation waveforms: `images/simulation_waveforms/`
- measured waveforms: `images/measured_waveforms/`
- extraction traceability: `calculations/report_extraction_index.md`

## 1. Recommended Evidence Structure

Each module should eventually be documented in the same structure:

### 1. Schematic Snapshot

- one clear circuit image
- probe node labels
- operating condition notes

### 2. Simulation Result

- waveform screenshot
- key measured values such as amplitude, frequency, ripple, or DC level
- short statement of whether the result matches the design target

### 3. Practical Measurement

- oscilloscope or meter result
- test condition
- measured values
- difference from simulation

### 4. Short Conclusion

- whether this module is qualified for integration into the full chain

## 2. Module-Level Record Template

The following template should be reused for each module.

```md
## Module Name

### Purpose
### Schematic
### Simulation Setup
### Simulation Result
### Measurement Setup
### Measured Result
### Comparison
### Conclusion
```

## 3. Current Module Result Plan

For this project, the recommended order of evidence collection is:

1. sine-wave generator
2. AC full bridge and zero balancing
3. three-op-amp high-CMRR amplifier
4. square-wave converter
5. phase-sensitive demodulator
6. low-pass filter
7. DC amplifier
8. full-chain integration result

## 4. Expected Result Summary By Module

### 4.1 Sine-Wave Generator

Expected result:

- stable sine wave
- frequency close to 5 kHz
- amplitude close to the design target around 5.5 V

What simulation should prove:

- oscillator starts and stabilizes
- waveform distortion is acceptable

What practical measurement should prove:

- real output frequency is near the target
- amplitude is stable under load

### 4.2 AC Full Bridge And Compensation

Expected result:

- near-zero output at balance
- bridge output increases with simulated or applied load

What simulation should prove:

- bridge imbalance generates the expected AC differential signal
- balancing and capacitive compensation reduce zero residual

What practical measurement should prove:

- bridge can be adjusted near zero
- residual zero remains small in actual wiring conditions

Known design target from the report:

- residual zero after adjustment less than 1 mV

### 4.3 Three-Op-Amp High-CMRR Amplifier

Expected result:

- useful differential signal is amplified
- common-mode interference is strongly suppressed

What simulation should prove:

- amplitude gain matches design expectation
- no clipping under expected input range

What practical measurement should prove:

- output is larger and cleaner than raw bridge output
- long-wire or interference conditions do not dominate the signal

### 4.4 Square-Wave Converter

Expected result:

- same-frequency square wave derived from the sine reference
- clean switching around zero crossing

What simulation should prove:

- square wave has correct frequency and polarity timing

What practical measurement should prove:

- output is stable
- no severe chatter at zero crossing

### 4.5 Phase-Sensitive Demodulator

Expected result:

- pulsating output whose average depends on phase relation
- in-phase and anti-phase conditions give opposite average polarity

What simulation should prove:

- detector output changes sign correctly with reference phase
- switching process produces the expected full-wave effect

What practical measurement should prove:

- demodulator output follows the sign logic observed in simulation
- switching ripple is acceptable before low-pass filtering

### 4.6 Low-Pass Filter

Expected result:

- ripple reduced strongly
- DC or slowly varying term preserved

What simulation should prove:

- 5 kHz-related residue is rejected
- output follows the expected average level

What practical measurement should prove:

- ripple is reduced compared with the demodulator output
- output settles at a stable DC level

Known reference point from the report:

- one representative low-pass output is about `-144 mV`

### 4.7 DC Amplifier

Expected result:

- filtered DC level is scaled to the display input range

What simulation should prove:

- gain close to the required value
- no unacceptable offset growth

What practical measurement should prove:

- output reaches the target level without instability

Known reference point from the report:

- final target is around `2 V`
- required gain is about `13.9`

## 5. Full-Chain Integration Result

This is the most important final section of the repository.

It should summarize the complete circuit behavior in one place:

- excitation enters the bridge
- bridge converts the physical change into weak AC imbalance
- amplifier lifts the small differential signal
- square-wave path provides synchronous polarity reference
- PSD converts phase-related AC into an average-valued waveform
- low-pass filter extracts the useful low-frequency term
- DC amplifier scales the result for display

The final evidence should include:

- one complete-chain waveform map
- key node amplitudes
- final output under representative input conditions
- short conclusion on whether the chain works end to end

## 6. Simulation And Practice Comparison Table

This table should be filled as images and measurements are added.

| Module | Design Target | Simulation | Measurement | Difference | Comment |
| --- | --- | --- | --- | --- | --- |
| Sine-wave generator | 5 kHz, about 5.5 V | to be filled | to be filled | to be filled | |
| Full bridge | near-zero at balance | to be filled | to be filled | to be filled | |
| High-CMRR amplifier | amplify weak differential AC | to be filled | to be filled | to be filled | |
| Square-wave converter | synchronous square wave | to be filled | to be filled | to be filled | |
| PSD | signed pulsating output | to be filled | to be filled | to be filled | |
| Low-pass filter | suppress carrier residue | to be filled | to be filled | to be filled | |
| DC amplifier | about 2 V target level | to be filled | to be filled | to be filled | |

## 7. Current Status

At the current stage, this repository already has:

- the signal-chain structure
- module-level principle documents
- key mathematical interpretation
- extracted implementation targets from the course-design report

What is still needed for this document to become complete is:

- schematic snapshots for each module
- simulation waveform screenshots
- practical oscilloscope or multimeter records
- one end-to-end comparison summary

## 8. Final Summary Format

When all results are added, the final summary should be written in this order:

1. Which modules matched design calculations closely.
2. Which modules differed between simulation and practice.
3. What caused the differences.
4. Whether the complete circuit met the overall design goal.
5. What should be optimized in the next iteration.
