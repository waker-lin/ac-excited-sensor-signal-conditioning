# 01 Sine-Wave Generator

## Role In The Full Chain

This module generates the sinusoidal excitation used to drive the AC bridge and to define the phase reference of the whole signal-conditioning system.

Without this stage, the following modules lose their common time base:

- the AC full bridge has no carrier excitation
- the square-wave converter has no reference input
- the phase-sensitive detector has no synchronous basis

So this is both an excitation source and a reference source.

## Schematic

The schematic has already been extracted from the Word report:

![Sine-wave generator schematic](../../images/module_figures/sine_wave_generator_schematic.png)

Source figure in the report:

- `图4-1 正弦驱动电路`

## Working Principle

The current implementation adopts an RC bridge sine-wave oscillator rather than an LC oscillator. The reason is practical: the target carrier is around 5 kHz, where an RC implementation is simpler and more convenient for this design than a larger LC resonant network.

The oscillator contains four basic functions:

1. Amplification
2. Frequency selection
3. Positive feedback
4. Amplitude stabilization

The RC bridge network determines the oscillation frequency, while the nonlinear stabilization branch limits the loop gain after start-up so that the output settles into a stable sine wave instead of growing into clipping.

From the extracted schematic, the frequency-selective branch is formed mainly by:

- `R4 = 2.05 kOhm`
- `R5 = 2.05 kOhm`
- `C1 = 0.015 uF`
- `C2 = 0.015 uF`

The gain-setting and amplitude-limiting branch contains:

- `R1 = 23.7 kOhm`
- `R2 = 44 kOhm`
- `R3 = 5.1 kOhm`
- `D3`, `D4` as nonlinear amplitude-limiting elements
- `LM741CH` as the active amplifier

## Design Parameters And Calculation

### Known Design Targets

- excitation frequency: about `5 kHz`
- excitation amplitude: about `5.5 V`

### Formula (4-1): Oscillation Frequency

The formula extracted from the report corresponds to the RC bridge oscillation frequency:

`f_0 = 1 / (2 pi sqrt(R4 R5 C1 C2))`

Under the equal-value design assumption also stated in the report:

- `C1 = C2`
- `R4 = R5`

this reduces to:

`f_0 = 1 / (2 pi R C)`

### Substitution For 5 kHz Design

For the target frequency:

`f_0 = 5 kHz`

we obtain the required time constant:

`RC = 1 / (2 pi f_0) = 1 / (2 pi x 5000) ≈ 3.18 x 10^-5 s`

If:

`C = 0.015 uF = 15 x 10^-9 F`

then the corresponding resistor is approximately:

`R ≈ (3.18 x 10^-5) / (15 x 10^-9) ≈ 2.12 kOhm`

The actual selected standard values shown in the schematic are:

- `R4 = R5 = 2.05 kOhm`
- `C1 = C2 = 0.015 uF`

This is consistent with the report statement that the initial calculated values were later fine-tuned according to simulation behavior and then standardized.

### Formula (4-2): Amplifier Gain Condition

The extracted gain formula in the report corresponds to the non-inverting amplifier section:

`A = 1 + (R2 + R3) / R1`

Using the schematic values:

- `R1 = 23.7 kOhm`
- `R2 = 44 kOhm`
- `R3 = 5.1 kOhm`

we get the nominal small-signal gain:

`A ≈ 1 + (44 + 5.1) / 23.7 ≈ 3.07`

This is close to the classical Wien-bridge oscillation requirement that the loop gain around the start-up condition should be slightly above the steady-state threshold.

### Formula (4-3): Feedback Coefficient

The extracted report formula for the RC bridge feedback factor is:

`beta = 1 / (1 + R4 / R5 + C2 / C1)`

Under the equal-value assumption:

- `R4 = R5`
- `C1 = C2`

this becomes:

`beta = 1 / (1 + 1 + 1) = 1 / 3`

So the steady oscillation condition is consistent with:

`A beta = 1`

which implies:

`A ≈ 3`

That matches the gain target of the amplifier branch and explains why the diode-limited gain network is necessary: the oscillator needs sufficient gain to start, but not so much gain that the output clips severely in steady state.

## Engineering Meaning Of The Calculation

This module is not only a source block. Its frequency directly affects:

- bridge excitation condition
- square-wave reference frequency
- PSD operating condition
- low-pass filter rejection margin

So any frequency drift here propagates through the full signal chain.

The amplitude-stabilization network is equally important. A sine source with strong clipping or unstable amplitude would directly degrade:

- synchronous reference quality
- demodulation accuracy
- final DC stability

## Key Devices

The report mentions these device choices in this stage:

- `LM741CH`
- `SS34`

The extracted schematic figure for `图4-1` shows `LM741CH` and a diode limiting branch. The text section of the report also mentions SS34 as the practical diode choice used in the oscillator discussion.

## Simulation Result

The report states that the oscillator simulation result is shown in:

- `图4-2 正弦波发生器产生波形`
- `图4-3 正弦波发生器频率`

To be attached later in the repository:

- output waveform screenshot
- measured frequency screenshot

Expected simulation conclusion:

- sinusoidal waveform is stable
- frequency is near the design target of 5 kHz
- distortion is acceptable for downstream synchronous detection

## Practical Result

The report later mentions the practical waveform in:

- `图5-3 正弦发生器波形图`

To be attached later in the repository:

- oscilloscope screenshot of the actual sine-wave output
- measured amplitude and frequency
- stability observation under practical load

## Comparison And Conclusion

The final comparison for this module should answer:

- Is the actual frequency close to 5 kHz?
- Is the amplitude sufficient for the bridge stage?
- Is the waveform clean enough for stable reference conversion?
- Is the practical circuit close to the simulated oscillator behavior?

## Extracted Formula Assets

The original report stores several formulas in this section as embedded equation objects. They have been extracted and archived here:

- `calculations/extracted_formulas/eq_4_1_raw.png`
- `calculations/extracted_formulas/eq_4_1_condition_c_raw.png`
- `calculations/extracted_formulas/eq_4_1_condition_r_raw.png`
- `calculations/extracted_formulas/eq_4_1_substitution_raw.png`
- `calculations/extracted_formulas/eq_4_2_gain_raw.png`
- `calculations/extracted_formulas/eq_4_3_feedback_raw.png`

These extracted images serve as traceable evidence for the Markdown formulas written above.
