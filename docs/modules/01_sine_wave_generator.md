# 01 Sine-Wave Generator

## Role In The Full Chain

This module generates the sinusoidal excitation used to drive the AC bridge and to define the phase reference of the whole signal-conditioning system.

## Schematic

To be inserted: schematic screenshot of the RC bridge sine-wave oscillator.

Suggested asset path:

- `images/module_figures/sine_wave_generator_schematic.png`

## Working Principle

The current implementation adopts an RC bridge sine-wave oscillator rather than an LC oscillator. The reason is practical: the target carrier is around 5 kHz, where an RC implementation is simpler and more convenient for this design than a larger LC resonant network.

The oscillator contains four basic functions:

- amplification
- frequency selection
- positive feedback
- amplitude stabilization

The RC bridge determines the oscillation frequency, while the nonlinear stabilization network limits the gain so that the oscillation settles into a stable sine wave instead of growing into clipping.

## Design Parameters And Calculation

### Known Design Targets

- excitation frequency: about `5 kHz`
- excitation amplitude: about `5.5 V`

### Frequency Formula

For the equal-value RC bridge case:

`f_0 = 1 / (2 pi R C)`

This formula provides the first-pass resistor-capacitor selection for the oscillator.

### Design Meaning

This module is not only a source block. Its frequency directly affects:

- bridge excitation condition
- square-wave reference frequency
- PSD operating condition
- low-pass filter rejection margin

So any frequency drift here propagates through the full signal chain.

## Key Devices

The report mentions these device choices in this stage:

- `LM741CH`
- `SS34`

Those choices should be kept in the module record unless the circuit is later redesigned.

## Simulation Result

To be inserted:

- output waveform screenshot
- measured frequency screenshot
- whether the oscillator starts reliably and stabilizes near 5 kHz

Expected simulation conclusion:

- sinusoidal waveform is stable
- frequency is near the design target
- distortion is acceptable for downstream synchronous detection

## Practical Result

To be inserted:

- oscilloscope screenshot of the actual sine-wave output
- measured amplitude and frequency
- stability observation under practical load

## Comparison And Conclusion

The final comparison should answer:

- Is the actual frequency close to 5 kHz?
- Is the amplitude sufficient for the bridge stage?
- Is the waveform clean enough for stable reference conversion?

## To Add Next

- exact resistor values
- exact capacitor values
- actual measured frequency and amplitude
- schematic screenshot

