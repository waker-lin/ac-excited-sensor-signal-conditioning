# 03 Module Principles

## Usage

This document records each module using the same engineering template:

- Function
- Input
- Output
- Working principle
- Key components
- Design notes

The purpose is to keep the system readable from both directions:

- from left to right: how the signal is conditioned
- from right to left: why each stage is necessary for the final DC display

## 1. Sine-Wave Generator

### Function

Provide the AC excitation required by the sensor and the phase basis used later for synchronous detection.

### Input

- DC power supply
- frequency-setting and amplitude-setting network

### Output

- stable sinusoidal excitation signal

### Working Principle

The oscillator produces a sinusoid at a chosen carrier frequency. This signal excites the sensor structure and also serves as the origin for the demodulation reference path. If its amplitude or phase is unstable, the entire downstream measurement chain becomes inconsistent.

### Key Components

- RC bridge oscillator core
- amplitude stabilization network
- frequency-selective RC components
- buffer stage if load isolation is required

### Design Notes

- the current implementation selects an RC bridge sine oscillator rather than an LC oscillator
- the report indicates a nominal excitation of 5.5 V at 5 kHz
- the report also mentions LM741CH and SS34 in the oscillator implementation, so later hardware notes should preserve that choice unless a redesign is made
- oscillator distortion should be low enough that higher harmonics do not contaminate the sensing and reference paths

## 2. Full Bridge / Differential Transformer Sensor Stage

### Function

Convert the physical variable into a weak differential AC signal.

### Input

- sinusoidal excitation
- physical quantity such as force, displacement, position, or another modulating variable

### Output

- weak differential AC signal whose amplitude and possibly phase depend on the measured quantity

### Working Principle

Under excitation, the sensor does not usually emit a static DC value. Instead, the physical variable changes coupling, impedance balance, or secondary response, causing the output carrier amplitude to change. Around the balance point, the phase may also indicate direction, which is why the later demodulation stage must preserve sign information instead of merely rectifying magnitude.

### Key Components

- four bridge arms or differential transformer windings
- balancing network
- capacitive compensation network
- sensor connection wiring

### Design Notes

- the current implementation uses an AC full bridge built from four 350 ohm strain gauges
- because wiring and distributed capacitance disturb AC bridge balance, the report adds both resistive balance and capacitive compensation
- the documented balancing target is residual zero less than 1 mV
- lead routing and shielding matter because the output amplitude is small

## 3. Three-Op-Amp High-CMRR Amplifier

### Function

Amplify the small differential sensor signal while rejecting common-mode voltage and interference.

### Input

- differential AC sensor output
- possible common-mode disturbance and line pickup

### Output

- amplified AC signal suitable for synchronous detection

### Working Principle

The three-op-amp instrumentation-amplifier structure uses two input buffers and one differential subtraction stage. The front stage presents high input impedance to the sensor, and the differential stage extracts the useful signal. Accurate resistor matching is essential, because common-mode rejection is not achieved by topology alone; it depends on component symmetry.

### Key Components

- three operational amplifiers
- precision resistor network
- gain-setting resistor
- common-mode compensation branch
- input protection or filtering components

### Design Notes

- the report frames this stage as necessary for long-wire transmission and weak bridge output
- the intended signal-lift scale is from tens of millivolts toward volt-level processing
- LM358 is the documented op-amp choice for this stage
- resistor matching strongly affects CMRR, so later hardware records should include tolerance strategy

## 4. Square-Wave Conversion

### Function

Generate a polarity reference synchronized with the excitation phase.

### Input

- sinusoidal reference related to the excitation source

### Output

- synchronized square-wave reference

### Working Principle

A comparator or zero-crossing stage converts the reference sinusoid into a square wave that changes polarity at the carrier zero crossings. The exact amplitude of this square wave is usually unimportant; its timing is what matters. In a switching demodulator, this square wave tells the circuit whether the incoming sensor signal should pass with positive polarity or be inverted.

### Key Components

- zero-crossing comparator
- threshold network
- limiter or buffer stage

### Design Notes

- the documented implementation uses LM393 as the comparator
- this stage also performs level adaptation because the raw sine amplitude is higher than the desired reference-logic level
- switching threshold should be centered to minimize phase error
- propagation delay directly affects demodulation accuracy

## 5. Phase-Sensitive Demodulator

### Function

Convert the amplified AC sensor signal into a signed low-frequency or DC-representable quantity.

### Input

- amplified sensor AC signal
- synchronized square-wave reference

### Output

- chopped or multiplied waveform with a non-zero average when phase alignment exists

### Working Principle

The detector multiplies or commutates the signal using the reference. If the signal is in phase with the reference, positive half-cycles are mapped to positive output and negative half-cycles are inverted into the same polarity, producing a positive average after filtering. If the signal is 180 degrees out of phase, the average becomes negative. If the signal is in quadrature, the average ideally tends to zero.

### Key Components

- transistor-controlled switching core
- op-amp stage for inversion or following
- reference driver
- polarity steering network

### Design Notes

- the current implementation is a switch-mode full-wave phase-sensitive detector rather than a diode-envelope detector
- the report describes transistor switching controlled by the square-wave reference and mentions LM741CH plus 2N3392 in this section
- changing the reference phase changes the sign of the averaged output, which is the practical basis for direction discrimination
- switching feedthrough can appear as ripple or spikes before the low-pass stage

## 6. Low-Pass Filter

### Function

Remove the carrier-related ripple from the demodulated waveform and retain the slowly varying average component.

### Input

- pulsating demodulated waveform containing DC/low-frequency content plus higher-frequency terms

### Output

- smoothed DC or slowly varying analog signal

### Working Principle

The demodulated waveform contains a desired average component and undesired AC components around the carrier and its harmonics. The low-pass filter suppresses these high-frequency terms. Its cutoff must therefore be lower than the switching-related ripple content but high enough to preserve the measurement bandwidth.

### Key Components

- second-order MFB active low-pass structure
- RC frequency-setting network
- precision op-amp

### Design Notes

- the documented design uses a second-order MFB low-pass filter
- the chosen cutoff is 100 Hz against a 5 kHz carrier-related residue
- OP07D is the documented op-amp choice in the report
- this stage should be evaluated for the ripple-versus-response-speed tradeoff rather than by cutoff alone

## 7. DC Amplifier

### Function

Scale the filtered signal to the level required by the output meter or acquisition circuit.

### Input

- filtered DC or slowly varying analog voltage

### Output

- calibrated DC output with the required sensitivity and polarity

### Working Principle

This stage applies final gain and may also include zeroing, offset trimming, span adjustment, and output buffering. At this point the carrier has already been removed, so the focus shifts from synchronous extraction to calibration accuracy, drift, and load driving.

### Key Components

- op-amp gain stage
- feedback resistor network
- zero/span adjustment components

### Design Notes

- the report gives a representative post-filter level of about -144 mV before this stage
- the final DC stage is designed to amplify that level toward about 2 V
- the reported gain requirement is about 13.9, which should later be cross-checked against the actual resistor values in the schematic
- excessive gain here will expose any residual ripple left by the low-pass stage

## 8. 3.5-Digit Display

### Function

Present the final conditioned result as a readable numerical value.

### Input

- scaled DC voltage from the final amplifier

### Output

- visible numeric reading

### Working Principle

The display stage converts the final analog value into a decimal indication. Whether implemented with a dedicated A/D display driver or an external meter module, it effectively treats the preceding analog chain as a signal source whose zero, gain, and polarity must already be correct.

### Key Components

- 3.5-digit panel meter or A/D display IC
- reference source
- input scaling network

### Design Notes

- the current repository still treats the display block as downstream integration work and has not yet documented its complete circuit details
- display resolution should be consistent with real system noise and stability
- reference accuracy of the display stage can limit the overall credibility of the measurement
- decimal-point placement should align with the intended engineering unit

## Module Dependency Summary

The modules should not be seen as independent blocks. Their dependencies are strong:

- poor excitation quality degrades both sensing and demodulation reference
- insufficient CMRR at the amplifier stage contaminates every downstream waveform
- phase error in the square-wave reference directly reduces `V_out`
- weak filtering leaves ripple that the DC amplifier and display will expose

That is why the repository documents modules individually but always evaluates them in the context of the full chain.
