# 04 Square-Wave Converter

## Role In The Full Chain

This module converts the sine-wave reference into a synchronized square-wave control signal for the phase-sensitive detector.

## Schematic

Extracted from the report:

![Square-wave converter schematic](../../images/module_figures/square_wave_converter_schematic.png)

## Working Principle

The sine-wave excitation contains the phase information needed by the PSD stage, but a switching detector is more conveniently driven by a square-wave reference. This module therefore detects the zero crossings of the sine wave and outputs a same-frequency square wave.

Its main job is timing, not analog amplitude reproduction.

## Design Parameters And Calculation

### Known Target

- reference frequency equals excitation frequency
- nominal operating frequency: `5 kHz`

So the basic synchronous condition is:

`f_ref = f_exc = 5 kHz`

### Engineering Meaning

If this module introduces too much phase delay, the PSD output scales down according to the `cos(phi)` relation discussed in the mathematical analysis.

## Key Devices

The report documents:

- `LM393`

## Design Notes

This module is part of the reference path. Errors here do not just distort a local waveform; they directly change the sign and magnitude of the final demodulated output.

## Simulation Result

Extracted simulation waveform:

![Square-wave converter simulation](../../images/simulation_waveforms/square_wave_converter_output_simulation.png)

Expected simulation conclusion:

- output frequency equals input frequency
- switching points stay near the intended zero crossings

## Practical Result

Extracted practical waveform:

![Square-wave converter measured](../../images/measured_waveforms/square_wave_converter_output_measured.jpeg)

Still to be supplemented later:

- practical output level note
- jitter or chatter observation

## Comparison And Conclusion

The final comparison should answer:

- Is the output frequency correct?
- Is the phase relation acceptable for PSD?
- Does the practical comparator stage switch cleanly?

## To Add Next

- exact threshold network values
- measured duty ratio
- waveform screenshots
