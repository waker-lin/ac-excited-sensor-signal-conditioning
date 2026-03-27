# 03 Module Principles

## Purpose

This document is now the index of the unit-circuit section of the repository.

The project is no longer described mainly as a generic signal-conditioning topic. It is now organized primarily around individual circuit units, and only then around the overall chain explanation.

## Main Structure

The repository is divided into two layers:

### Part I: Unit Circuits

This is the main body of the project. Each circuit unit is documented independently so that schematic, principle, calculation, simulation, measurement, and conclusion remain in one place.

### Part II: Overall Explanation

This includes the block diagram, signal flow, waveform evolution, and mathematical analysis of the full system.

## Unit-Circuit Documentation Rule

Each unit record follows the same engineering structure:

- role in the full chain
- schematic
- working principle
- design parameters and calculation process
- simulation result
- practical result
- comparison and conclusion

This keeps every circuit file aligned with the project objective:

`principle -> calculation -> simulation -> practice -> summary`

## Part I: Core Unit Circuits

These six files are the main unit-circuit records of the repository:

- [02_ac_full_bridge.md](./modules/02_ac_full_bridge.md): AC full bridge, balance, and compensation
- [03_three_op_amp_amplifier.md](./modules/03_three_op_amp_amplifier.md): three-op-amp high-CMRR amplifier
- [04_square_wave_converter.md](./modules/04_square_wave_converter.md): zero-crossing square-wave converter
- [05_phase_sensitive_demodulator.md](./modules/05_phase_sensitive_demodulator.md): switch-mode phase-sensitive detector
- [06_low_pass_filter.md](./modules/06_low_pass_filter.md): second-order MFB low-pass filter
- [07_dc_amplifier.md](./modules/07_dc_amplifier.md): final DC amplification stage

## Supporting Circuit Files

These files are still useful, but they are now treated as supporting records around the six main unit circuits:

- [01_sine_wave_generator.md](./modules/01_sine_wave_generator.md): excitation source and reference basis
- [08_display_stage.md](./modules/08_display_stage.md): output/display stage placeholder

## How To Read The Unit Files

Two reading modes are recommended.

### 1. By Circuit Unit

If you want to study, debug, or complete one circuit block, open only the corresponding module file.

### 2. By Signal Flow

If you want to understand the full analog path, read the six core circuit files in order from bridge to DC amplifier.

## Current Status

The unit files already include:

- role definition
- principle explanation
- key known parameters from the report
- extracted schematics for the main circuits
- extracted simulation and measured waveforms for several units

The next stage is to strengthen each unit file with:

- fuller parameter derivation
- explicit substitution steps
- comparison between simulation and practical measurement
- tighter conclusions for each circuit block

## Extracted Assets

A first batch of figures and formula objects has already been extracted from the project report.
See:

- [../calculations/report_extraction_index.md](../calculations/report_extraction_index.md)
