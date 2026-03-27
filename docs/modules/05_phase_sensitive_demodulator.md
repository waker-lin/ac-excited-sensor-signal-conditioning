# 05 Phase-Sensitive Demodulator

## Design Task

This module is the core of the AC signal-conditioning chain. Its job is to convert the amplified AC carrier signal into a polarity-sensitive pulsating waveform whose average value is proportional to the in-phase component of the sensor signal.

In engineering terms, this stage must do two things at once:

- preserve amplitude information
- preserve direction information through phase discrimination

That is why this circuit uses switch-type synchronous demodulation rather than ordinary rectification.

## Schematic

![PSD schematic](../../images/module_figures/phase_sensitive_demodulator_schematic.png)

## Design Logic

### 1. Two operating states of the circuit

Let the amplified sensor signal applied at `IN6` be denoted by $v_i(t)$, and let the square-wave reference applied at `IN7` be denoted by $v_r(t)$.

The circuit has two operating states controlled by transistor `Q1`.

#### State A: transistor on

When the reference drives `Q1` into conduction, the non-inverting input of the op amp is pulled to approximately `0 V`. At that moment the circuit becomes an inverting amplifier. Since

$$
R_{20} = R_{22} = 1\,\text{k}\Omega
$$

the closed-loop gain is

$$
A_{\mathrm{inv}} = -\frac{R_{22}}{R_{20}} = -1
$$

so the output is

$$
v_o(t) = -v_i(t)
$$

#### State B: transistor off

When `Q1` is cut off, the non-inverting input follows the signal through `R21`. Under the virtual-short condition of the op amp, the circuit becomes a unity-gain follower, so

$$
v_o(t) = v_i(t)
$$

Therefore the demodulator does not change the magnitude scale of the signal. It only changes the sign according to the state of the reference switch.

### 2. Equivalent switching description

The two operating states above can be written compactly with a normalized switching function $s(t)$:

$$
s(t) \in \{+1,-1\}
$$

and

$$
v_o(t) = v_i(t)\,s(t)
$$

With the polarity convention used in this project, the switching function can be written as

$$
s(t) = \operatorname{sgn}[\cos(\omega t)]
$$

which means the circuit multiplies the input signal by a same-frequency square-wave reference.

## Parameter Calculation

### Input signal model

Write the amplified sensor signal as

$$
v_i(t) = A(x)\cos(\omega t + \varphi)
$$

where:

- $A(x)$ is the amplitude determined by the measured quantity $x$
- $\omega$ is the excitation angular frequency
- $\varphi$ is the phase difference between the sensor signal and the reference path

Then the switch-type demodulator output is

$$
v_o(t) = A(x)\cos(\omega t + \varphi)\,\operatorname{sgn}[\cos(\omega t)]
$$

### Low-pass output of the switch-type demodulator

The square-wave switching function has the Fourier series

$$
\operatorname{sgn}[\cos(\omega t)] = \frac{4}{\pi}\left(\cos \omega t + \frac{1}{3}\cos 3\omega t + \frac{1}{5}\cos 5\omega t + \cdots \right)
$$

Multiplying $v_i(t)$ by the fundamental term of the switching function gives

$$
v_o(t) \approx \frac{4A(x)}{\pi}\cos(\omega t + \varphi)\cos(\omega t) + \text{higher-frequency terms}
$$

Using

$$
\cos \alpha \cos \beta = \frac{1}{2}\left[\cos(\alpha-\beta) + \cos(\alpha+\beta)\right]
$$

we obtain

$$
v_o(t) \approx \frac{2A(x)}{\pi}\cos \varphi + \frac{2A(x)}{\pi}\cos(2\omega t + \varphi) + \text{higher-frequency terms}
$$

After the low-pass filter removes the AC terms, the retained DC component is

$$
V_{\mathrm{LPF}} = \frac{2A(x)}{\pi}\cos \varphi
$$

This expression is the key design result for the actual switch-type circuit. Compared with ideal sinusoidal multiplication, the proportional coefficient is different, but the phase law is unchanged: the output depends on the in-phase component of the input signal.

### Physical meaning of the phase term

The demodulator therefore distinguishes three important cases:

$$
\varphi = 0 \quad \Rightarrow \quad V_{\mathrm{LPF}} = \frac{2A(x)}{\pi} > 0
$$

$$
\varphi = \pi \quad \Rightarrow \quad V_{\mathrm{LPF}} = -\frac{2A(x)}{\pi} < 0
$$

$$
\varphi = \frac{\pi}{2} \quad \Rightarrow \quad V_{\mathrm{LPF}} = 0
$$

This is exactly the reason the system can distinguish positive displacement, negative displacement, and quadrature interference.

## Key Device Choice

The report uses:

- `LM741CH` as the op amp
- `2N3392` as the transistor switch

The design idea is clear: the op amp provides the linear closed-loop inversion/following behavior, while the transistor performs the state switching under control of the reference square wave.

## Design Notes

- The reference frequency must match the carrier frequency; otherwise the average output collapses.
- The reference phase must be controlled; otherwise the demodulated amplitude is reduced by the factor $\cos \varphi$.
- Because $R_{20} = R_{22}$, the switched stage preserves input amplitude while changing only the sign. This is a deliberate design choice.
- The output of this stage is not yet the final DC result. It is a pulsating waveform that must still be filtered.

## Simulation Result

![PSD reference and modulated input](../../images/simulation_waveforms/psd_reference_and_modulated_signal_simulation.png)

![PSD output simulation](../../images/simulation_waveforms/psd_output_simulation.png)

The first simulation figure shows that the amplified carrier signal and the square-wave reference have the same frequency and a defined phase relationship. The second figure shows the expected demodulator output: a full-wave pulsating waveform whose average value is positive for the selected phase convention.

This is exactly the waveform that should appear before low-pass filtering. It is not a failure to remove ripple; it is the intended output of synchronous switching demodulation.

## Practical Result

![PSD measured](../../images/measured_waveforms/phase_sensitive_demodulator_output_measured.png)

The measured waveform on hardware is also a pulsating demodulated waveform rather than a simple sine or a random chopped signal. That confirms that the transistor switching and the op-amp sign reversal are working together as designed.

Because the circuit is built on a practical prototype board, waveform edges and pulse uniformity are less ideal than in simulation. That does not change the engineering conclusion: the stage is performing polarity-sensitive synchronous demodulation and providing the correct type of signal to the low-pass filter.

## Comparison And Engineering Conclusion

This module is successful when the following conditions hold:

- the reference path controls sign reversal synchronously with the carrier
- the output becomes a polarity-sensitive pulsating waveform
- after low-pass filtering, the retained component follows $V_{\mathrm{LPF}} = \dfrac{2A(x)}{\pi}\cos \varphi$

Under that criterion, the circuit design is sound. The equal-resistor switching structure keeps the gain magnitude under control, while the phase relation between $v_i(t)$ and $v_r(t)$ determines the sign and magnitude of the final DC output.
