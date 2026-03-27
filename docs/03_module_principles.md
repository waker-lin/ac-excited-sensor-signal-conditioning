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

- oscillator core
- amplitude stabilization network
- frequency-selective components
- buffer stage if load isolation is required

### Design Notes

- excitation frequency should stay within the sensor's effective operating range
- oscillator distortion should be low enough that higher harmonics do not contaminate the sensing and reference paths
- output drive capability must be compatible with the bridge or transformer load

## 2. Full Bridge / Differential Transformer Sensor Stage

### Function

Convert the physical variable into a weak differential AC signal.

### Input

- sinusoidal excitation
- physical quantity such as displacement, position, or another modulating variable

### Output

- weak differential AC signal whose amplitude and possibly phase depend on the measured quantity

### Working Principle

Under excitation, the sensor does not usually emit a static DC value. Instead, the physical variable changes coupling, impedance balance, or secondary response, causing the output carrier amplitude to change. Around the balance point, the phase may also indicate direction, which is why the later demodulation stage must preserve sign information instead of merely rectifying magnitude.

### Key Components

- bridge arms or differential transformer windings
- balancing network
- sensor connection wiring

### Design Notes

- balance condition should be defined clearly because it determines the zero point
- lead routing and shielding matter because the output amplitude is small
- sensor source impedance affects amplifier noise and bandwidth

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
- input protection or filtering components

### Design Notes

- resistor matching strongly affects CMRR
- gain must be allocated so that the stage lifts the useful signal without saturating on offsets or spikes
- input bias current and offset voltage matter for small-signal accuracy
- bandwidth must remain sufficient for the excitation frequency and any expected modulation envelope

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

- comparator or Schmitt-trigger stage
- threshold network
- limiter or buffer stage

### Design Notes

- switching threshold should be centered to minimize phase error
- hysteresis must be controlled; too little causes chatter, too much introduces timing shift
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

- analog switch, multiplier, or synchronous rectifier core
- reference driver
- polarity steering network

### Design Notes

- phase alignment is the core requirement of the entire chain
- switching feedthrough and charge injection can appear as ripple or offset
- reference duty ratio should be controlled because asymmetry creates DC error

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

- RC network or active low-pass stage
- optional higher-order filter if ripple suppression is demanding

### Design Notes

- lower cutoff improves ripple suppression but slows response
- active filters add flexibility but may introduce offset and noise
- the filter must be considered together with the final display update behavior

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

- DC offset and drift now directly affect the displayed result
- excessive gain in this stage can amplify residual ripple left by the low-pass filter
- output range should be matched to the display full-scale requirement

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
