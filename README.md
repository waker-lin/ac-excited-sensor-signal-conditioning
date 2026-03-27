# AC-Excited Sensor Signal Conditioning

Analysis and implementation archive for an AC-excited sensor signal-conditioning chain, including differential amplification, phase-sensitive demodulation, low-pass filtering, and DC output conversion.

## Project Overview
This repository is organized as a learning-oriented project archive rather than a single schematic dump. It is intended to document the full signal chain from circuit principle to simulation, waveform interpretation, parameter calculation, and practical implementation.

## Project Goal
Convert weak AC sensor signals into stable DC voltages proportional to displacement or input variation.

## Signal Chain
Sinusoidal Excitation -> Sensor / Bridge Output -> Differential Amplifier -> Square-Wave Reference -> Phase-Sensitive Detector -> Low-Pass Filter -> DC Amplifier -> Display / Measurement

## Core Principle
The measured quantity modulates the amplitude and phase of an AC carrier. A synchronous reference is used for phase-sensitive demodulation, and low-pass filtering extracts the useful DC component.

## Key Formula
`v_s(t) = A(x) cos(omega t + phi)`

`v_m(t) = v_s(t) * v_r(t)`

`V_out ∝ A(x) cos(phi)`

## Repository Structure
- `docs/`: system overview, architecture, module principles, waveform evolution, mathematics, parameters, debugging, results, and future work
- `images/`: overall circuit, block diagrams, waveform figures, and module screenshots
- `calculations/`: gain budget, demodulation derivation, low-pass design, and error analysis
- `simulations/`: Multisim, LTspice, Proteus, and exported waveforms
- `hardware/`: schematic, BOM, PCB, and implementation notes
- `data/`: raw and processed data
- `reports/`: progress notes and final report
- `refs/`: textbooks, papers, and datasheet references

## Current Status
This is the repository scaffold. Detailed content will be filled in module by module.
