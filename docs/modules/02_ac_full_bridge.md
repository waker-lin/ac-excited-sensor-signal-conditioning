# 02 AC Full Bridge

## Role In The Full Chain

This module converts the physical change of the sensor into a weak AC differential signal. In the current implementation, it is the actual sensing front end of the system.

## Schematic

To be inserted: AC full-bridge schematic with balance and capacitive compensation.

Suggested asset path:

- `images/module_figures/ac_full_bridge_schematic.png`

## Working Principle

The circuit uses four strain gauges to form an AC full bridge. Under force, the bridge leaves the balance state and generates a small differential output. Because the bridge is AC excited, the useful signal appears as a small carrier-domain response rather than a direct DC level.

In practical AC bridge operation, wiring capacitance and distributed parameters prevent ideal zero balance. For this reason, the module contains:

- resistive balancing
- capacitive compensation

These adjustments reduce zero residual and make the downstream amplification and PSD stages usable.

## Design Parameters And Calculation

### Known Parameters

- bridge arm nominal resistance: `350 ohm`
- small resistance variation example: `Delta R = 0.44 ohm`
- zero residual target after balancing: less than `1 mV`

### Small-Signal Relation

The report gives:

`Delta R / R = K epsilon`

which links the mechanical strain to bridge resistance change.

For system interpretation, the bridge output satisfies the proportional relation:

`V_bridge ∝ V_exc * (Delta R / R)`

This means bridge sensitivity depends on both excitation amplitude and fractional resistance change.

## Design Notes

This stage is extremely sensitive to imperfect balance. If the bridge zero is not corrected well enough, downstream gain stages will amplify the residual error together with the useful signal.

## Simulation Result

To be inserted:

- bridge output waveform at balance
- bridge output waveform under simulated load
- effect of resistive and capacitive compensation

Expected simulation conclusion:

- near-zero residual at balance
- clear AC differential output when imbalance is introduced

## Practical Result

To be inserted:

- measured bridge output at no-load condition
- measured bridge output under representative loading
- practical adjustment method for the balancing network

## Comparison And Conclusion

The final comparison should answer:

- Can the bridge be adjusted below 1 mV residual?
- Is the output magnitude consistent with the design estimate?
- Does the practical wiring condition introduce additional imbalance?

## To Add Next

- exact bridge component values
- compensation network values
- measured residual zero value
- waveform screenshots at null and loaded conditions
