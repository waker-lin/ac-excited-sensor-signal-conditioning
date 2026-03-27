# 04 Square-Wave Converter

## Design Task

This module belongs to the reference path. Its function is to convert the sinusoidal excitation reference into a square-wave control signal with the same frequency and a well-defined phase relationship, so that the following switch-type phase-sensitive demodulator can be driven reliably.

The design objective is therefore not amplitude amplification, but **accurate zero-crossing timing**.

## Schematic

![Square-wave converter schematic](../../images/module_figures/square_wave_converter_schematic.png)

## Design Logic

### 1. Comparator-based zero-crossing conversion

The circuit uses `LM393` as a zero-crossing comparator. The sinusoidal input is applied to the non-inverting input, while the inverting input is tied to `0 V`. Therefore the switching threshold is

$$
v_{\mathrm{th}} = 0
$$

If the input sinusoid is written as

$$
v_i(t) = V_m \cos(\omega t)
$$

then the comparator output changes state whenever $v_i(t)$ crosses zero. Ignoring propagation delay, the output square wave satisfies

$$
v_r(t) =
\begin{cases}
V_H, & v_i(t) \ge 0 \\
V_L, & v_i(t) < 0
\end{cases}
$$

so the output frequency is identical to the input frequency:

$$
f_r = f_i, \qquad T_r = T_i = \frac{2\pi}{\omega}
$$

This is exactly the requirement of synchronous detection.

### 2. Why the LM393 output stage matters

`LM393` uses an open-collector output. In this circuit, `R15 = 1 k\Omega` provides the pull-up path, so the output amplitude is determined by the supply and pull-up configuration, while the switching instant is determined by the input zero crossing.

This is an important design point: the subsequent phase-sensitive demodulator does not need an accurate analog copy of the sinusoid. It needs a reference with:

- correct frequency
- stable polarity change at the zero crossings
- enough logic swing to drive the transistor switch cleanly

## Parameter Design

### Zero-crossing condition

The circuit is designed so that the reference reversal occurs when

$$
v_i(t) = 0
$$

For a sinusoidal carrier, this guarantees that the reference square wave is phase-locked to the excitation source.

### Phase error caused by comparator delay

If the comparator introduces a propagation delay $\tau_d$, the reference phase error is

$$
\Delta \varphi = \omega \tau_d
$$

At the design frequency of `5 kHz`, the signal period is

$$
T = \frac{1}{f} = \frac{1}{5 \times 10^3} = 200\,\mu\text{s}
$$

So the design requirement is simply

$$
\tau_d \ll 200\,\mu\text{s}
$$

When this condition is satisfied, the phase error introduced by the comparator remains small enough that the demodulated output is not noticeably reduced.

### Engineering interpretation

The importance of this module is captured by the synchronous-demodulation law:

$$
V_{\mathrm{LPF}} \propto \cos \varphi
$$

A timing error here does not stay local to the reference path. It directly changes the scale of the final DC output after demodulation and low-pass filtering.

## Key Device Choice

The report selects `LM393` because it is suitable for zero-crossing detection:

- low input offset improves threshold accuracy near `0 V`
- the input range and comparator structure suit reference conversion
- the open-collector output is convenient for generating a square-wave control signal with a defined pull-up level

## Design Notes

- This stage should be evaluated by switching instant, not by analog waveform fidelity.
- The comparator threshold must remain close to `0 V`; otherwise the reference phase will shift with input amplitude.
- The output amplitude only needs to be sufficient for reliable switching in the following demodulator.

## Simulation Result

![Square-wave converter simulation](../../images/simulation_waveforms/square_wave_converter_output_simulation.png)

The simulated output is a symmetric square wave with the same repetition rate as the sinusoidal reference. That verifies the intended zero-crossing conversion mechanism:

- the comparator is switching once per half-cycle
- the output frequency matches the carrier frequency
- the reference path is now suitable for driving the electronic switch in the demodulator

## Practical Result

![Square-wave converter measured](../../images/measured_waveforms/square_wave_converter_output_measured.jpeg)

The measured waveform also shows a stable square-wave output with a large voltage swing relative to the oscilloscope scale. From the engineering point of view, the practical result confirms that the hardware comparator stage can provide a usable switching reference rather than a marginal or noisy transition.

The practical criterion here is simple: if the output edges remain stable and the period tracks the excitation source, the module has completed its task.

## Comparison And Engineering Conclusion

This module is successful when three conditions are met:

- the output frequency is equal to the excitation frequency
- the output state changes at the sinusoidal zero crossings
- the output swing is sufficient to control the transistor in the phase-sensitive demodulator

Under that criterion, the design is appropriate. The converter does not carry measurement amplitude information; it establishes the timing reference that determines whether the following demodulator produces a positive, negative, or near-zero average output.
