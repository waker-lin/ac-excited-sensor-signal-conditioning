# AC-Excited Sensor Signal Conditioning

Project archive for a complete AC-excited sensor signal-conditioning circuit.

Chinese title: 交流激励式传感器信号调理电路分析与实现

This repository follows a compact three-part structure.

## Repository Logic

1. Overall design concept
2. Unit-circuit design, simulation, and debugging
3. Bill of materials

The intent is straightforward:

- explain the overall idea first
- then document each circuit as a complete closed loop
- finally attach the material list

## Part 1 Overall Design Concept

This part should answer four questions before any unit circuit is discussed:

- what the measurement target is
- why AC excitation is used
- how the signal is processed from input to output
- how the complete circuit is organized in one schematic

Overall design flow extracted from the report:

![Overall design flow](./images/system_design_flow.png)

Overall circuit simulation figure extracted from the report:

![Overall circuit simulation](./images/overall_circuit_simulation.png)

## Part 2 Unit Circuits

Each circuit should be written as one self-contained engineering record.

Recommended internal order for each unit:

`circuit design -> parameter calculation -> component selection -> simulation result -> debugging and measured result`

Current unit scope:

1. Sine-wave drive circuit
2. AC full bridge and zero adjustment
3. Three-op-amp high-CMRR amplifier
4. Square-wave converter
5. Switch-type full-wave phase-sensitive demodulator
6. Phase shifter
7. Low-pass filter
8. DC amplifier

## Part 3 Bill Of Materials

The material list is kept separate from the design text so that the main body stays focused on principle, calculation, simulation, and practical verification.

## Core Working Relation

For synchronous extraction, the key relation is:

`v_s(t) = A(x)cos(omega t + phi)`

`v_r(t) = cos(omega t)`

After synchronous demodulation and low-pass filtering:

`V_out ∝ A(x)cos(phi)`

## Document Map

### Overall Design Part

- [docs/01_project_overview.md](./docs/01_project_overview.md)
- [docs/02_system_architecture.md](./docs/02_system_architecture.md)
- [reports/final_report.md](./reports/final_report.md)

### Unit-Circuit Part

- [docs/modules/01_sine_wave_generator.md](./docs/modules/01_sine_wave_generator.md)
- [docs/modules/02_ac_full_bridge.md](./docs/modules/02_ac_full_bridge.md)
- [docs/modules/03_three_op_amp_amplifier.md](./docs/modules/03_three_op_amp_amplifier.md)
- [docs/modules/04_square_wave_converter.md](./docs/modules/04_square_wave_converter.md)
- [docs/modules/05_phase_sensitive_demodulator.md](./docs/modules/05_phase_sensitive_demodulator.md)
- [docs/modules/06_low_pass_filter.md](./docs/modules/06_low_pass_filter.md)
- [docs/modules/07_dc_amplifier.md](./docs/modules/07_dc_amplifier.md)

### Supporting Analysis

- [docs/04_waveform_evolution.md](./docs/04_waveform_evolution.md)
- [docs/05_mathematical_analysis.md](./docs/05_mathematical_analysis.md)
- [docs/06_key_parameters.md](./docs/06_key_parameters.md)
- [docs/08_experiment_and_results.md](./docs/08_experiment_and_results.md)
