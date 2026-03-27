# AC-Excited Sensor Signal Conditioning

Project archive for a complete AC-excited sensor signal-conditioning circuit.

Chinese title: 交流激励式传感器信号调理电路分析与实现

This repository is organized as a circuit project record rather than a general study notebook. The emphasis is on documenting one complete circuit system with clear engineering evidence.

## Project Scope

The main analog processing chain is:

`Full Bridge -> Three-Op-Amp High-CMRR Amplifier -> Square-Wave Conversion -> Phase-Sensitive Demodulator -> Low-Pass Filter -> DC Amplifier`

These six blocks form the core of the repository.

The sine-wave excitation source and the final display path are still part of the system, but the repository is now primarily structured around the six main circuit units above.

## Repository Structure By Purpose

The repository is now best understood in two parts.

### Part I: Unit Circuits

Each major circuit unit is documented independently with the same engineering logic:

- principle
- calculation process
- simulation
- practical result
- summary

Main unit circuits:

1. Full bridge
2. Three-op-amp high-CMRR amplifier
3. Square-wave converter
4. Phase-sensitive demodulator
5. Low-pass filter
6. DC amplifier

### Part II: Overall System Explanation

This part explains how the complete circuit works as one system:

- overall block flow
- signal path and waveform evolution
- mathematical relation of synchronous demodulation
- why the final output can represent the measured quantity

## Visual Overview

Overall design flow extracted from the project report:

![Overall design flow](./images/system_design_flow.png)

Overall circuit simulation figure extracted from the project report:

![Overall circuit simulation](./images/overall_circuit_simulation.png)

## Core Working Principle

For the synchronous extraction stage, the key model is:

`v_s(t) = A(x)cos(omega t + phi)`

`v_r(t) = cos(omega t)`

`v_m(t) = v_s(t) * v_r(t)`

After low-pass filtering:

`V_out ∝ A(x)cos(phi)`

This is the theoretical reason the chain can distinguish in-phase, anti-phase, and quadrature behavior.

## Document Map

### Part I: Unit Circuits

- [docs/modules/02_ac_full_bridge.md](./docs/modules/02_ac_full_bridge.md)
- [docs/modules/03_three_op_amp_amplifier.md](./docs/modules/03_three_op_amp_amplifier.md)
- [docs/modules/04_square_wave_converter.md](./docs/modules/04_square_wave_converter.md)
- [docs/modules/05_phase_sensitive_demodulator.md](./docs/modules/05_phase_sensitive_demodulator.md)
- [docs/modules/06_low_pass_filter.md](./docs/modules/06_low_pass_filter.md)
- [docs/modules/07_dc_amplifier.md](./docs/modules/07_dc_amplifier.md)

Supporting circuit record:

- [docs/modules/01_sine_wave_generator.md](./docs/modules/01_sine_wave_generator.md)

### Part II: Overall System Explanation

- [docs/01_project_overview.md](./docs/01_project_overview.md)
- [docs/02_system_architecture.md](./docs/02_system_architecture.md)
- [docs/04_waveform_evolution.md](./docs/04_waveform_evolution.md)
- [docs/05_mathematical_analysis.md](./docs/05_mathematical_analysis.md)
- [docs/06_key_parameters.md](./docs/06_key_parameters.md)
- [docs/08_experiment_and_results.md](./docs/08_experiment_and_results.md)

## Current Status

The repository already contains:

- extracted schematics for the main unit circuits
- extracted simulation waveforms
- extracted measured waveforms
- first-pass module records
- first-pass overall system documents

## What Is Intentionally Excluded

This repository emphasizes reproducible documentation rather than raw design source dumping.

- Multisim source files such as `.ms14` are ignored by git
- exported figures, screenshots, and derived evidence are preferred over raw source project files
- the report is used as a source for extraction, but the repository content is reorganized around circuit units and overall system explanation
