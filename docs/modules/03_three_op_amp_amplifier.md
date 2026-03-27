# 03 Three-Op-Amp High-CMRR Amplifier

## Design Task

This stage is the first active signal-conditioning block after the bridge. Its task is not simply to make the signal larger. It must amplify the weak differential bridge output while preventing long-wire common-mode interference from being converted into false useful signal.

So the design target of this module is:

- keep the bridge output differential
- provide the first controlled gain stage
- suppress common-mode pickup before synchronous demodulation

## Schematic

![Three-op-amp amplifier schematic](../../images/module_figures/three_op_amp_amplifier_schematic.png)

## Design Logic

### 1. Why a three-op-amp topology is used

The report states that the sensor signal has to be transmitted over long wires to the signal-processing unit. Under that condition, common-mode interference is unavoidable, while the useful sensor signal is only from a few millivolts to several tens of millivolts. A single-ended amplifier would therefore amplify interference together with the useful signal.

The three-op-amp instrumentation-amplifier topology solves this problem in two steps:

- the first two op amps provide high-input-impedance differential pre-amplification
- the third op amp performs subtraction so that the common-mode part is largely rejected and the differential part is preserved

That is why this stage sits immediately after the bridge and before the square-wave conversion and PSD chain.

### 2. How this particular circuit is arranged

The circuit can be read as two cascaded functions.

#### Input gain stage

`U1A` and `U2A` form the differential pre-amplifier. Their feedback arms use:

- `R6 = 25 kΩ`
- `R8 = 25 kΩ`
- `R7 = 50 kΩ` potentiometer

This is the standard adjustable-gain front end of a three-op-amp instrumentation amplifier. The shared resistor `R7` determines the differential gain of the first stage.

#### Output subtraction stage

`U3A` and the surrounding resistor network perform the differential subtraction. The main matched network is:

- `R9 = 40 kΩ`
- `R10 = 40 kΩ`
- `R14 = 40 kΩ`
- `R11 = 40 kΩ`

With equal ratios on the two sides, the subtraction stage gives nominal unity differential gain while maintaining high common-mode rejection.

The branch formed by `R12`, `R13`, and the trimming resistor `R15` is the common-mode compensation network. Its purpose is not to create the main gain, but to correct residual mismatch so that practical CMRR remains high after component tolerance and wiring asymmetry are included.

## Parameter Calculation

### Overall Gain Requirement

The report gives the system-level amplitude target:

`40 mV -> 4 V`

So the full chain must eventually provide about `100x` voltage gain. This amplifier stage does not need to complete that entire gain alone. Its role is to perform the **first clean lift** of the weak differential signal while keeping enough headroom for later synchronous demodulation and DC amplification.

### First-Stage Differential Gain

For the symmetric three-op-amp input stage,

`A_d1 = 1 + 2R / R_g`

where:

- `R = R6 = R8 = 25 kΩ`
- `R_g` is the effective resistance of the adjustable resistor `R7`

So,

`A_d1 = 1 + 50 kΩ / R_g`

If this stage is designed for a nominal `10x` first amplification, then

`10 = 1 + 50 kΩ / R_g`

which gives

`R_g ≈ 5.56 kΩ`

This result explains the design choice of using a `50 kΩ` potentiometer: the gain can be trimmed around the required operating point rather than being frozen by a single fixed resistor.

### Output-Stage Differential Gain

The second stage is built with equal resistor ratios:

`R10 / R9 = 40 kΩ / 40 kΩ = 1`

`R11 / R14 = 40 kΩ / 40 kΩ = 1`

So the nominal differential gain of the subtraction stage is approximately:

`A_d2 ≈ 1`

Therefore the overall closed-loop gain is mainly set by the first stage:

`K ≈ A_d1 * A_d2 ≈ A_d1`

Under the nominal design condition,

`K ≈ 10`

This is consistent with the report statement that the selected resistor values make the closed-loop gain `K = 10`.

### Why Common-Mode Compensation Is Added

A three-op-amp topology only achieves high CMRR when the resistor ratios on both sides are closely matched. In real hardware, resistor tolerance, lead impedance, and layout asymmetry create a small conversion path from common-mode voltage to differential output.

That is why this design adds the trim branch using:

- `R12 = 20 kΩ`
- `R13 = 20 kΩ`
- `R15 = 10 kΩ` adjustable resistor

The engineering purpose of this branch is to inject a small correction term so that the output common-mode gain can be nulled as closely as possible in the real circuit. In other words, `R15` is used to tune **CMRR in practice**, not just gain on paper.

## Key Device Choice

The report selects `LM358`. The selection logic is straightforward:

- it provides dual op amps, which suits the multi-stage instrumentation topology
- it is widely available and easy to power in practical lab conditions
- its offset and common-mode characteristics are adequate for the first-stage weak-signal amplification task in this design

## Design Notes

- Gain should be added in stages. If the entire `100x` amplification is forced into one front-end stage, offset, noise, and residual common-mode leakage will become harder to control.
- High CMRR depends on resistor-ratio symmetry, not only on the topology name.
- The trim resistor in the compensation branch is part of the design logic, because practical CMRR always needs calibration.
- This stage should be evaluated by waveform linearity, gain, and resistance to common-mode disturbance together.

## Simulation Result

![Three-op-amp amplifier simulation](../../images/simulation_waveforms/three_op_amp_amplifier_output_simulation.png)

The simulation output remains a clean sinusoidal waveform at the carrier frequency, which means the amplifier is operating in the linear region rather than clipping or rectifying the signal. From the design perspective, the simulation verifies that:

- the weak bridge signal can be lifted without changing its carrier nature
- the amplifier bandwidth is sufficient for the `5 kHz` excitation signal
- the gain stage is suitable as a front end for the following square-wave conversion and synchronous detection stages

## Practical Result

![Three-op-amp measured waveform](../../images/measured_waveforms/three_op_amp_amplifier_output_measured_1.png)

![Three-op-amp hardware build](../../images/measured_waveforms/three_op_amp_amplifier_output_measured_2.png)

The measured result shows that the practical circuit still outputs a sinusoidal amplified signal, which means the stage is functionally correct. Compared with simulation, the hardware waveform shows more non-ideal behavior, which is reasonable for a hand-wired analog prototype with multiple op amps and long signal paths.

The hardware photo is also meaningful here: it explains why CMRR trimming is necessary. In a dense prototype-board implementation, resistor mismatch, wiring asymmetry, and coupling between nearby traces all reduce the ideal CMRR predicted by the schematic.

## Comparison And Engineering Conclusion

This module meets its engineering role when the following three conditions are satisfied:

- the bridge output is amplified by a controlled factor of about `10x`
- the waveform remains sinusoidal and does not saturate before demodulation
- common-mode pickup is suppressed strongly enough that the next stages still respond mainly to the useful differential signal

Under that criterion, the design is reasonable. The first stage provides the required front-end gain, the second stage preserves subtraction symmetry, and the trim branch gives a practical way to recover high CMRR in real hardware.

So the key contribution of this module is not merely gain. It is the combination of **weak-signal amplification + common-mode rejection + practical trim capability**, which makes the rest of the AC signal-conditioning chain possible.

