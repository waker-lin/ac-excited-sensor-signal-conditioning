# 03 Module Principles

## Purpose

This document is the module index of the repository.

The project is not intended to keep all module details in one long file. Instead, each module is documented as an individual circuit record so that schematic, calculation, simulation, measurement, and conclusion can stay together.

## Documentation Rule

Each module record follows the same engineering structure:

- role in the full chain
- schematic
- working principle
- design parameters and calculation process
- simulation result
- practical result
- comparison and conclusion

This rule keeps the repository aligned with the core objective:

`principle -> calculation -> simulation -> practice -> summary`

## Module List

- [01_sine_wave_generator.md](./modules/01_sine_wave_generator.md): RC bridge sine-wave generator
- [02_ac_full_bridge.md](./modules/02_ac_full_bridge.md): AC full bridge, balance, and compensation
- [03_three_op_amp_amplifier.md](./modules/03_three_op_amp_amplifier.md): three-op-amp high-CMRR amplifier
- [04_square_wave_converter.md](./modules/04_square_wave_converter.md): zero-crossing square-wave converter
- [05_phase_sensitive_demodulator.md](./modules/05_phase_sensitive_demodulator.md): switch-mode phase-sensitive detector
- [06_low_pass_filter.md](./modules/06_low_pass_filter.md): second-order MFB low-pass filter
- [07_dc_amplifier.md](./modules/07_dc_amplifier.md): final DC amplification stage
- [08_display_stage.md](./modules/08_display_stage.md): display/output stage placeholder

## How To Use These Module Files

Read them in two ways:

### 1. By Signal Flow

If you want to understand the whole circuit from input to output, read the modules in the listed order.

### 2. By Engineering Task

If you want to debug or improve one stage, open only the corresponding module file. Each file is meant to contain everything directly related to that circuit block.

## Current Status

The first pass of these module files already includes:

- circuit role definition
- principle explanation
- key known parameters from the report
- calculation anchors
- placeholders for simulation and measured evidence

The next stage is to attach:

- schematic screenshots
- waveform screenshots
- measured values
- discrepancy explanations

## Extracted Assets

A first batch of figures and formula objects has already been extracted from the project report.
See:

- [../calculations/report_extraction_index.md](../calculations/report_extraction_index.md)

