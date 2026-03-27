# 03 Three-Op-Amp High-CMRR Amplifier

## Role In The Full Chain

This module amplifies the weak differential bridge output while suppressing common-mode interference. It is the key transition from weak sensing signal to processable analog signal.

## Schematic

Extracted from the report:

![Three-op-amp amplifier schematic](../../images/module_figures/three_op_amp_amplifier_schematic.png)

## Working Principle

The stage follows the instrumentation-amplifier idea: two input amplifiers buffer and condition the differential input, and a differential subtraction stage extracts the useful signal while rejecting common-mode voltage.

This topology is used because the bridge signal is weak and the report assumes long-wire transmission between the sensor and the signal-processing circuit. Under that condition, common-mode pickup is unavoidable, so ordinary single-ended amplification would be unreliable.

## Design Parameters And Calculation

### Known Design Objective

The report indicates that the processing chain must handle a signal lift on the order of:

`40 mV -> 4 V`

So the full chain requires an overall effective magnification of about:

`G_total ≈ 100`

This stage provides the first important part of that lift.

### Engineering Meaning

The precise gain of this stage should later be verified from resistor values, but the current design intent is already clear:

- amplify the weak bridge signal
- keep common-mode residue small enough for the PSD stage
- avoid saturation before synchronous detection

## Key Devices

The report documents:

- `LM358`

## Design Notes

The most important practical issue is resistor matching. High CMRR is not obtained by topology alone. It depends on the symmetry of the resistor network and any common-mode compensation branch used in the circuit.

## Simulation Result

Extracted simulation waveform:

![Three-op-amp amplifier simulation](../../images/simulation_waveforms/three_op_amp_amplifier_output_simulation.png)

Still to be supplemented later:

- input/output amplitude table
- dedicated CMRR verification note

Expected simulation conclusion:

- differential signal is amplified as expected
- no serious clipping or distortion appears in the working range

## Practical Result

Extracted practical figures:

![Three-op-amp measured 1](../../images/measured_waveforms/three_op_amp_amplifier_output_measured_1.png)

![Three-op-amp measured 2](../../images/measured_waveforms/three_op_amp_amplifier_output_measured_2.png)

Still to be supplemented later:

- observed sensitivity to wire pickup or external interference
- tuning record if gain or compensation is adjustable

## Comparison And Conclusion

The final comparison should answer:

- Does the stage provide sufficient gain?
- Is common-mode interference acceptably suppressed?
- Does the practical circuit remain stable under long-wire conditions?

## To Add Next

- exact resistor network values
- actual gain expression from the schematic
- simulation and measurement screenshots


