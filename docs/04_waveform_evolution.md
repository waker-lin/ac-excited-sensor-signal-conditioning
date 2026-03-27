# 04 Waveform Evolution

## Purpose

This document tracks how the waveform changes along the complete signal chain. The emphasis is not only on waveform shape, but also on what information is being preserved, amplified, translated, or removed at each node.

## Recommended Reading Order

Observe the signal in the same order as the actual hardware path:

1. excitation sinusoid
2. sensor differential AC output
3. amplified sensor waveform
4. square-wave reference
5. demodulated pulsating waveform
6. low-pass filtered output
7. final DC output to display

## Node 1: Excitation Sinusoid

### Signal Form

- sinusoidal voltage at the carrier frequency
- stable amplitude and frequency

### Meaning

This waveform is the energy source for the sensor stage and the timing source for the reference path. It defines the phase origin of the entire system.

### What To Verify

- frequency stability
- low distortion
- sufficient drive amplitude

## Node 2: Sensor Differential Output

### Signal Form

- weak AC signal
- usually still sinusoidal or approximately sinusoidal
- amplitude depends on measured quantity
- phase may reverse across the balance point

### Meaning

The sensor has now transferred physical variation into the carrier domain. The useful information is no longer simply the presence of AC, but the relationship of amplitude and phase to the excitation.

### Typical Observation

- near the null point, the waveform amplitude is very small
- on one side of the null point, it is in phase with the reference
- on the other side, it is approximately 180 degrees out of phase

## Node 3: Amplified Sensor Waveform

### Signal Form

- same carrier frequency as the sensor output
- significantly larger amplitude
- ideally little extra distortion

### Meaning

This node should preserve the original measurement information while making it large enough for reliable demodulation. If clipping or noticeable asymmetry appears here, the later detector output will become unreliable.

### What A Good Waveform Looks Like

- larger but still clean AC waveform
- minimal common-mode residue
- no saturation at expected maximum displacement

## Node 4: Square-Wave Reference

### Signal Form

- square wave derived from the excitation path
- polarity changes at the carrier zero crossings

### Meaning

This is the switching command for the phase-sensitive detector. Its analog amplitude is secondary; edge timing and phase alignment are primary.

### What To Verify

- transition points aligned with the intended reference phase
- no excessive jitter or chatter
- duty cycle close to the intended value

## Node 5: Demodulated Pulsating Waveform

### Signal Form

- chopped, rectified, or multiplied waveform
- contains a low-frequency or DC average term plus higher-frequency ripple

### Meaning

The detector has translated phase relation into average polarity:

- in-phase input produces a positive average
- anti-phase input produces a negative average
- quadrature input ideally produces near-zero average

The waveform at this node is therefore the first stage where sign information becomes explicit in the average value rather than hidden in the carrier phase.

### Typical Observation

- a pulsating unipolar or bipolar waveform
- ripple at twice the carrier for ideal sinusoidal multiplication
- switching spikes if the practical detector uses analog switches

## Node 6: Low-Pass Filter Output

### Signal Form

- smooth DC level or slowly varying analog voltage
- residual ripple should be much smaller than the useful level

### Meaning

The low-pass filter removes the high-frequency demodulation products and keeps the average component. This node is the true analog measurement result before final scaling.

### What To Verify

- output polarity is consistent with sensor direction
- ripple is acceptable for the required display stability
- response speed is acceptable for the intended measurement dynamics

## Node 7: Final DC Output

### Signal Form

- scaled DC voltage
- directly suitable for display or external measurement

### Meaning

At this point, the signal is no longer analyzed as a carrier waveform. It is treated as a calibrated output quantity linked to the measured variable.

### What To Verify

- zero point
- full-scale gain
- linearity across operating range
- compatibility with the 3.5-digit display input

## Interpretation Along The Chain

The waveform evolution can be summarized as three successive transformations:

### A. Carrier Generation

The generator creates a stable sinusoidal basis for both sensing and synchronization.

### B. Carrier Modulation And Extraction

The sensor encodes measurement information into amplitude and phase, the amplifier preserves this information at a higher level, and the phase-sensitive detector converts it into an average-valued waveform.

### C. DC Representation

The low-pass filter and DC amplifier turn the demodulated result into a readable DC quantity.

## Suggested Additions When Waveform Images Are Available

When you later add Multisim captures or oscilloscope screenshots, extend each node with:

- figure name
- probe location
- time scale and voltage scale
- operating condition
- one-sentence interpretation of what the waveform proves

That will make the waveform document function as both a learning note and a verification record.
