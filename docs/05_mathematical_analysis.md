# 05 Mathematical Analysis

## Scope

This document summarizes the mathematical backbone of the AC-excited sensor signal-conditioning chain, with emphasis on phase-sensitive demodulation. The goal is to connect waveform behavior with the final DC output expression.

## 1. Sensor Signal Model

Assume the sensor output after the front end can be described by:

`v_s(t) = A(x)cos(omega t + phi)`

where:

- `A(x)` is the signal amplitude related to the measured quantity `x`
- `omega` is the excitation angular frequency
- `phi` is the phase difference between the sensor signal and the reference

This expression captures two important facts:

- the measurement result appears on a carrier rather than directly as DC
- the sign or direction information may be carried by phase, not by amplitude alone

## 2. Reference Signal Model

For theoretical derivation, first use an ideal sinusoidal reference:

`v_r(t) = cos(omega t)`

In the practical circuit, this reference is usually converted into a square wave for switching demodulation. That square wave can still be understood as a synchronous reference, and its fundamental component plays the same phase-discriminating role.

## 3. Multiplication In The Phase-Sensitive Detector

The ideal multiplier output is:

`v_m(t) = v_s(t) * v_r(t)`

Substitute the two expressions:

`v_m(t) = A(x)cos(omega t + phi)cos(omega t)`

Use the identity:

`cos(alpha)cos(beta) = 1/2[cos(alpha-beta)+cos(alpha+beta)]`

Let:

- `alpha = omega t + phi`
- `beta = omega t`

Then:

`v_m(t) = 1/2 A(x)[cos(phi) + cos(2omega t + phi)]`

This is the key result. The product contains:

- a low-frequency or DC term: `1/2 A(x)cos(phi)`
- a high-frequency term at `2omega`: `1/2 A(x)cos(2omega t + phi)`

## 4. Low-Pass Filtering

The low-pass filter removes the `2omega` term and retains only the average component:

`V_LPF = 1/2 A(x)cos(phi)`

Therefore:

`V_out ∝ A(x)cos(phi)`

If the later DC amplifier has gain `G_dc`, then more explicitly:

`V_out = G_dc * 1/2 A(x)cos(phi)`

The proportionality is the essential system conclusion: the final DC output depends on both signal amplitude and phase alignment with the reference.

## 5. Interpretation Of Phase Cases

### In-Phase Case

If `phi = 0`:

`cos(phi) = 1`

then:

`V_LPF = 1/2 A(x)`

The output is positive and reaches its maximum magnitude for the given amplitude. This corresponds to the sensor signal being aligned with the reference.

### Anti-Phase Case

If `phi = pi`:

`cos(phi) = -1`

then:

`V_LPF = -1/2 A(x)`

The output becomes negative. This is why phase-sensitive demodulation can distinguish direction or side relative to the balance point, which a simple magnitude rectifier cannot do.

### Quadrature Case

If `phi = pi/2` or `-pi/2`:

`cos(phi) = 0`

then:

`V_LPF = 0`

The average output ideally vanishes even though an AC signal exists. This demonstrates that the detector responds only to the component correlated with the reference phase.

## 6. Why This Matters In The Actual Circuit

The mathematical result explains the role of the main hardware blocks:

- the sensor stage creates `A(x)` and possibly changes `phi`
- the instrumentation amplifier enlarges `A(x)` without destroying phase information
- the square-wave reference establishes the switching polarity for synchronous detection
- the low-pass filter extracts the `cos(phi)`-weighted average term

So the final display is not showing raw amplitude alone. It is showing the in-phase projection of the sensor signal onto the chosen reference.

## 7. Practical Note About Square-Wave Demodulation

The actual implementation often uses a square-wave reference instead of an analog multiplier. In that case, the detector effectively multiplies the sensor signal by `sign(cos(omega t))`. The exact scale factor differs from the ideal sinusoidal multiplier, but the core conclusion remains unchanged:

- the output is still proportional to the component of the sensor signal that is phase-aligned with the reference
- in-phase and anti-phase conditions give opposite polarities
- quadrature components are ideally rejected in the average sense

Therefore, for system-level analysis, the expression

`V_out ∝ A(x)cos(phi)`

remains the most useful summary.

## 8. Design Consequences

The derivation gives several direct engineering consequences:

- any phase error in the reference path reduces output by the factor `cos(phi)`
- if `phi` drifts toward 90 degrees, sensitivity collapses
- increasing front-end gain helps only if phase integrity is preserved
- low-pass cutoff must reject the `2omega` term without overly slowing the measurement response

These points should be checked against simulation and experiment as the repository grows.
