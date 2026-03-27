# 02 AC Full Bridge

## Design Task

The bridge is the sensing front end of the whole system. Its job is not to provide a large voltage, but to convert the small resistance change of the strain-sensitive element into a stable AC differential signal that can still be amplified and synchronously demodulated later.

For this stage, the engineering problem is twofold:

- the bridge output is only at the `mV` level
- under AC excitation, lead capacitance and distributed parameters make the bridge difficult to balance with resistance trimming alone

So this circuit must solve both **signal conversion** and **zero-residual suppression**.

## Schematic

![AC full bridge schematic](../../images/module_figures/ac_full_bridge_schematic.png)

## Design Logic

### 1. Why an AC full bridge is used

The sensor side is driven by a sinusoidal excitation source. The bridge therefore does not directly produce a DC level. Instead, it modulates the carrier amplitude according to the resistance change caused by the measured mechanical quantity.

That choice is important for the whole architecture:

- the useful information stays on the carrier and can be amplified as an AC signal
- later, the phase-sensitive demodulator can separate the in-phase useful component from drift and out-of-phase interference
- bridge output polarity and phase can still be tracked after amplification

### 2. Why compensation is necessary

In an ideal purely resistive bridge, zero output is obtained by resistance balance alone. In the practical circuit described in the report, the strain-gauge leads introduce distributed capacitance, which means the bridge arms are no longer purely resistive. The result is a nonzero residual voltage even when no load is applied.

For that reason the bridge includes two balancing mechanisms:

- resistive balance, used to correct static arm mismatch
- capacitive balance, used to correct impedance-angle mismatch caused by lead capacitance

The design goal given in the report is to reduce the residual zero output to less than `1 mV` before the signal enters the high-gain stages.

## Parameter Calculation

### Known Design Data

- nominal bridge arm resistance: `R = 350 ohm`
- report result for resistance change under the design condition: `Delta R = 0.44 ohm`
- bridge relation: `Delta R / R = K epsilon`
- zero-residual target after balancing: `< 1 mV`

### Fractional Resistance Change

Using the given values,

`Delta R / R = 0.44 / 350 ≈ 1.26 x 10^-3`

This number is the core electrical quantity produced by the sensor. It tells us that the bridge is working with a fractional resistance variation on the order of `10^-3`, so the bridge output must also be expected to remain small.

### Small-Signal Interpretation

For an AC bridge, the output is proportional to excitation amplitude and fractional arm mismatch:

`v_bridge(t) = A_bridge * cos(omega t)`

with

`A_bridge ∝ V_exc * (Delta R / R)`

The exact proportional coefficient depends on which arms are active and how the resistance changes are distributed, but the engineering conclusion is stable: with `Delta R / R ≈ 1.26 x 10^-3`, the bridge only produces a weak carrier-domain differential voltage. That is why a dedicated high-CMRR differential amplifier is mandatory in the next stage.

If the excitation amplitude is about `5.5 V`, the expected bridge output is only in the `mV` range. This is consistent with the report statement that the sensor-side useful signal is only from a few millivolts to several tens of millivolts.

### How the Simulation Model Implements the Bridge

Because Multisim cannot directly reproduce the mechanical deformation process, the report models the bridge electrically:

- four nominal `349.5 ohm` resistors represent the bridge arms near the `350 ohm` operating point
- `1 ohm` trimming resistors are used to introduce small imbalance and emulate the effect of loading
- the `Rp-C` compensation branch is used to tune the phase angle of the arm impedance so that the unloaded bridge can be nulled again under AC excitation

This is a sound engineering simplification: the simulation does not attempt to model mechanics, but it preserves the electrical effect that matters to the signal chain.

## Design Notes

- The bridge must be balanced before high-gain amplification. Otherwise the residual zero component will be amplified together with the useful signal.
- AC bridge balance is an impedance-balance problem, not only a resistance-balance problem. That is why the RC compensation branch is structurally necessary.
- This stage should be judged by residual suppression and signal symmetry, not by absolute amplitude alone.

## Simulation Result

![AC full bridge simulation](../../images/simulation_waveforms/ac_full_bridge_output_simulation.png)

The simulated waveform remains a small sinusoidal signal centered near the zero line, which is exactly what this stage should produce. From an engineering point of view, the simulation verifies three things:

- the bridge still outputs a carrier-frequency differential signal rather than a DC quantity
- the imbalance introduced in the model is small, so the output stays in the weak-signal regime
- after compensation and balance adjustment, the residual offset does not dominate the useful AC component

## Practical Result

![AC full bridge measured](../../images/measured_waveforms/ac_full_bridge_output_measured.png)

The measured oscilloscope result shows that the practical bridge output is also sinusoidal under AC excitation. Compared with simulation, the real waveform carries more amplitude mismatch and practical uncertainty, which is expected because the hardware includes wiring capacitance, probe loading, contact resistance, and environmental pickup.

The important practical conclusion is not that the waveform is perfectly ideal, but that the bridge can still provide a usable AC differential response after balancing. That is the condition required for the next amplifier stage to work correctly.

## Comparison And Engineering Conclusion

Both simulation and experiment support the same design conclusion:

- the bridge successfully converts the sensor resistance change into an AC differential signal
- the output remains in the `mV` range, so front-end amplification is necessary by design
- RC compensation is not optional decoration; it is required to suppress zero residual under real wiring conditions

So for this module, the key design achievement is **not high output amplitude**, but **a controllable, balanceable, and phase-consistent weak AC signal source for the downstream chain**.
