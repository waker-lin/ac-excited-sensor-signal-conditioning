# 07 DC Amplifier

## Role In The Full Chain

This module scales the filtered low-level DC signal to the final usable output range.

## Schematic

Extracted from the report:

![DC amplifier schematic](../../images/module_figures/dc_amplifier_schematic.png)

## Working Principle

After low-pass filtering, the useful information has already been extracted, but the voltage level may still be too small for direct display. The DC amplifier therefore performs final gain scaling and may also define output polarity and sensitivity.

## Design Parameters And Calculation

### Known Input Level

The report gives one representative low-pass output level:

`V_LPF ≈ -144 mV`

### Known Output Target

The final target level is approximately:

`V_final ≈ 2 V`

### Required Gain

So the reported gain requirement is:

`G_DC ≈ 13.9`

which matches:

`|2 / 0.144| ≈ 13.9`

### Engineering Meaning

This module is not responsible for extracting the signal from the carrier. That work is already done. Its job is final scaling, so offset and residual ripple now become highly visible.

## Design Notes

The practical risk of this stage is that any residual ripple left by the low-pass filter will also be amplified. Therefore this module must be evaluated together with the previous stage rather than in isolation.

## Simulation Result

Extracted simulation waveform:

![DC amplifier simulation](../../images/simulation_waveforms/dc_amplifier_input_output_simulation.png)

Expected simulation conclusion:

- output reaches the target scale
- polarity is consistent with the intended display meaning

## Practical Result

To be inserted:

- measured amplifier output voltage
- gain error if any
- observed offset and stability

## Comparison And Conclusion

The final comparison should answer:

- Does the stage provide the required gain near 13.9?
- Is the final output close to 2 V under the reference condition?
- Does residual ripple remain acceptable after amplification?

## To Add Next

- exact resistor ratio
- measured output value
- practical waveform screenshot
