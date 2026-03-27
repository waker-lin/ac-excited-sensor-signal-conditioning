# AC-Excited Sensor Signal Conditioning

Project archive for a complete AC-excited sensor signal-conditioning circuit.

Chinese title: 交流激励式传感器信号调理电路分析与实现

This repository is organized as an engineering project archive. The writing order is not module-first, but system-first.

## Recommended Reading Order

The recommended reading order is:

1. overall design concept
2. overall circuit schematic
3. unit-circuit design and simulation
4. hardware implementation and debugging
5. bill of materials

This order is used because the circuit units only make full sense after the reader has seen the total signal path and the complete circuit structure.

## Overall Design Concept

The system uses AC excitation and synchronous demodulation to extract a weak sensor signal while preserving phase information.

The core idea is:

- use a sinusoidal source to excite the sensing bridge
- convert sensor variation into a weak AC differential signal
- amplify the useful differential component while suppressing common-mode interference
- generate a synchronous square-wave reference
- perform phase-sensitive demodulation
- use low-pass filtering and DC amplification to obtain a usable output quantity

## Overall Circuit Schematic

Overall design flow extracted from the project report:

![Overall design flow](./images/system_design_flow.png)

Overall circuit simulation figure extracted from the project report:

![Overall circuit simulation](./images/overall_circuit_simulation.png)

## Unit-Circuit Scope

After the overall concept and the total circuit are clear, the repository enters the unit-circuit part.

Main unit circuits:

1. Sine-wave drive circuit
2. AC full bridge and zero adjustment
3. Three-op-amp high-CMRR amplifier
4. Square-wave converter
5. Switch-type full-wave phase-sensitive demodulator
6. Phase shifter
7. Low-pass filter
8. DC amplifier

## Core Working Relation

For the synchronous extraction stage, the key relation is:

`v_s(t) = A(x)cos(omega t + phi)`

`v_r(t) = cos(omega t)`

After synchronous demodulation and low-pass filtering, the retained component follows the phase law:

`V_out ∝ A(x)cos(phi)`

This is the theoretical reason the system can distinguish in-phase, anti-phase, and quadrature conditions.

## Document Map

### Overall System Part

- [docs/01_project_overview.md](./docs/01_project_overview.md)
- [docs/02_system_architecture.md](./docs/02_system_architecture.md)
- [docs/04_waveform_evolution.md](./docs/04_waveform_evolution.md)
- [docs/05_mathematical_analysis.md](./docs/05_mathematical_analysis.md)
- [reports/final_report.md](./reports/final_report.md)

### Unit-Circuit Part

- [docs/modules/01_sine_wave_generator.md](./docs/modules/01_sine_wave_generator.md)
- [docs/modules/02_ac_full_bridge.md](./docs/modules/02_ac_full_bridge.md)
- [docs/modules/03_three_op_amp_amplifier.md](./docs/modules/03_three_op_amp_amplifier.md)
- [docs/modules/04_square_wave_converter.md](./docs/modules/04_square_wave_converter.md)
- [docs/modules/05_phase_sensitive_demodulator.md](./docs/modules/05_phase_sensitive_demodulator.md)
- [docs/modules/06_low_pass_filter.md](./docs/modules/06_low_pass_filter.md)
- [docs/modules/07_dc_amplifier.md](./docs/modules/07_dc_amplifier.md)

## Repository Status

The repository already contains extracted schematics, simulation figures, measured waveforms, and draft module pages, but the formal writing order is now defined as:

`overall first -> unit circuits second -> hardware/debugging third`
