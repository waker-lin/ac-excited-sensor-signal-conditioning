# 01 Project Overview

## Project Positioning

This repository is a circuit-project archive for one complete AC-excited sensor signal-conditioning system.

The repository should be organized in the same order as the final engineering report instead of starting immediately from isolated circuit units.

## Recommended Top-Level Logic

The preferred project logic is:

`overall design concept -> overall circuit schematic -> unit-circuit design and simulation -> hardware implementation and debugging -> bill of materials`

That means the repository should begin with the system-level explanation, then move into unit circuits, and only after that enter hardware debugging and materials.

## Overall-First Principle

Before writing the individual circuit units, the project should first explain two things clearly:

1. the overall design concept
2. the overall circuit schematic

Without these two parts, later module descriptions become fragmented because the reader does not yet know:

- why the system uses AC excitation
- why the signal must pass through bridge, amplification, square-wave conversion, synchronous demodulation, filtering, and DC amplification
- how the individual circuits are connected in the full schematic

So the repository should always start from the whole system and then move into the units.

## Suggested Chapter Mapping

### Chapter 3: Overall Design Concept And System Schematic

This part should include:

- design objectives
- technical route selection
- overall signal flow
- full circuit schematic and block partitioning

### Chapter 4: Unit Circuit Design And Simulation

This part should cover:

- sine-wave drive circuit
- AC full bridge and zero adjustment
- three-op-amp high-CMRR amplifier
- square-wave converter
- switch-type full-wave phase-sensitive demodulator
- phase shifter
- low-pass filter
- DC amplifier

Each unit should use one consistent internal order:

`circuit design -> parameter calculation -> component selection -> simulation result`

For bridge and debugging-oriented sections, waveform and zero-adjustment results can be placed directly after the circuit-design subsection if that matches the available material better.

### Chapter 5: Hardware Implementation And Debugging

This part should include:

- physical construction
- system integration
- step-by-step debugging by circuit unit

Each debugging section should answer four questions:

- what was the intended function
- what practical problem appeared
- how was it adjusted
- what result was finally obtained

### Appendix A: Bill Of Materials

The material list should be placed in an appendix instead of being mixed into the main design chapters.

## Why This Structure Is Better

The previous numbering mixed:

- whole-system explanation
- circuit design
- simulation verification
- practical debugging
- material listing

That makes the report hard to expand and hard to maintain.

The cleaned structure now follows the actual engineering logic:

- first explain the overall idea and total circuit
- then explain each unit circuit
- then describe hardware implementation and debugging
- finally attach the material list
