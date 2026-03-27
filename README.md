# AC-Excited Sensor Signal Conditioning

Learning-oriented project archive for an AC-excited sensor signal-conditioning system.

Chinese title: 交流激励式传感器信号调理电路分析与实现

This repository is intended to grow into a complete engineering study record rather than a schematic-only dump. The target is to document the full path from system concept to module-level reasoning, mathematical derivation, simulation evidence, and experiment notes.

## Project Scope

The current system chain is:

`Sine-Wave Generator -> Full Bridge / Differential Transformer -> Three-Op-Amp High-CMRR Amplifier -> Square-Wave Conversion -> Phase-Sensitive Demodulator -> Low-Pass Filter -> DC Amplifier -> 3.5-Digit Display`

The practical engineering objective is to convert a weak AC sensor output into a stable DC quantity that still preserves both magnitude information and directional information around the balance point.

This repository will therefore focus on:

- why AC excitation is used instead of direct DC measurement
- how the sensor stage converts physical variation into an amplitude- and phase-related AC signal
- how differential amplification suppresses common-mode interference while retaining the small useful component
- how synchronous demodulation extracts the signed low-frequency or DC term from the carrier
- how filtering, gain allocation, and output scaling affect readability and stability

## Current Implementation Snapshot

Based on the available schematic and the circuit-related sections of the project report, the present implementation can already be described more concretely:

- sensing frontend: AC-excited full bridge using four 350 ohm strain gauges with bridge balancing and capacitive compensation
- excitation source: RC bridge sine-wave oscillator, nominally 5.5 V and 5 kHz
- front-end amplification: three-op-amp high-CMRR differential amplifier for long-wire and weak-signal conditions
- reference generation: LM393 zero-crossing square-wave converter
- synchronous extraction: switch-mode phase-sensitive demodulator
- post-processing: second-order MFB low-pass filter with 100 Hz cutoff, followed by DC gain stage
- practical output target: filtered signal around -144 mV further amplified toward about 2 V DC for the display stage

This means the repository is no longer just documenting a generic topology. It is being aligned to one concrete implementation while still keeping the analysis reusable for similar AC-excited sensor systems.

## Core Working Principle

For an AC-excited sensor, the measured signal can be modeled as:

`v_s(t) = A(x)cos(omega t + phi)`

where `A(x)` is related to the measured quantity and `phi` represents the phase relation between the sensor output and the excitation reference.

The synchronous reference is:

`v_r(t) = cos(omega t)`

After multiplication in the phase-sensitive detector:

`v_m(t) = v_s(t) * v_r(t)`

and after low-pass filtering, the retained component is proportional to:

`V_out ∝ A(x)cos(phi)`

This is the key reason the chain can distinguish in-phase, anti-phase, and quadrature cases.


## Visual Overview

Overall design flow extracted from the project report:

![Overall design flow](./images/system_design_flow.png)

Overall circuit simulation figure extracted from the project report:

![Overall circuit simulation](./images/overall_circuit_simulation.png)
## Repository Goals

This project is being structured as a sustainable technical archive with four parallel lines of work:

1. System documentation: end-to-end signal chain, module responsibilities, and design constraints.
2. Mathematical analysis: gain budgeting, phase-sensitive demodulation, filter behavior, and error sources.
3. Simulation verification: exported waveforms, parameter comparison, and evidence traceability without forcing raw source files into the public repository.
4. Hardware and experiment records: schematic, notes, test data, troubleshooting, and final conclusions.

## Document Map

- [docs/01_project_overview.md](./docs/01_project_overview.md): project positioning, motivation, and archive usage
- [docs/02_system_architecture.md](./docs/02_system_architecture.md): end-to-end block logic and path decomposition
- [docs/03_module_principles.md](./docs/03_module_principles.md): module-by-module engineering notes
- [docs/04_waveform_evolution.md](./docs/04_waveform_evolution.md): waveform transition along the signal chain
- [docs/05_mathematical_analysis.md](./docs/05_mathematical_analysis.md): key formulas and phase-sensitive demodulation derivation
- [docs/06_key_parameters.md](./docs/06_key_parameters.md): frequencies, gains, filter corner, dynamic range, and other tunable values
- [docs/07_debugging_and_errors.md](./docs/07_debugging_and_errors.md): failure modes, debugging logic, and error sources
- [docs/08_experiment_and_results.md](./docs/08_experiment_and_results.md): experiment procedures and observed results
- [docs/09_future_improvements.md](./docs/09_future_improvements.md): future iterations and optimization directions

## Repository Structure

```text
.
|-- docs/
|-- images/
|-- calculations/
|-- simulations/
|   |-- multisim/
|   |-- ltspice/
|   |-- proteus/
|   `-- exported_waveforms/
|-- hardware/
|   |-- schematic/
|   |-- bom/
|   `-- pcb/
|-- data/
|   |-- raw/
|   `-- processed/
|-- reports/
`-- refs/
```

## Recommended Content Placement

- `simulations/multisim/`: keep local or private working files if needed, but do not assume they should be versioned publicly
- `simulations/exported_waveforms/`: exported waveform screenshots or CSV data suitable for publication
- `images/`: schematic screenshots, block diagrams, module diagrams, and oscilloscope captures
- `hardware/schematic/`: full circuit source files, PDFs, or exported images
- `reports/`: progressive write-ups, stage summaries, and final report drafts
- `refs/`: textbooks, papers, datasheets, and external references used in analysis

## What Is Intentionally Excluded

This repository is currently structured to emphasize reproducible documentation rather than raw design source dumping.

- Multisim source files such as `.ms14` are intentionally ignored by git
- only exported figures, screenshots, or derived evidence should be uploaded unless there is a specific reason to publish source files
- the course report is used as a reference source, but only the circuit-related content is driving the repository narrative

## Current Status

Stage 1 establishes the repository skeleton and the first-pass core documentation.

Stage 2 begins binding the repository to the actual course-design implementation by incorporating:

- the overall circuit screenshot
- circuit-section information extracted from the report
- concrete parameter values and device choices for each module

## How To Extend This Repository

When new materials are added, expand the archive in this order:

1. Place source assets in the correct directory.
2. Add node names, screenshots, and measured values to the relevant document.
3. Link each waveform or formula back to the exact module and system function it supports.
4. Record assumptions, component values, and unresolved issues explicitly so the repository remains reusable.

