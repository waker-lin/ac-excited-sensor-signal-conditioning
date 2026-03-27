# 01 Project Overview

## Project Positioning

This repository is a circuit-project archive for one complete AC-excited sensor signal-conditioning system.

The project structure should remain compact and directly support the final report writing.

## Confirmed Structure

The repository now follows this three-part logic:

1. overall design concept
2. unit-circuit design, simulation, and debugging
3. bill of materials

This structure is better suited to the available material because the practical verification is concentrated around each unit circuit rather than requiring a large independent debugging chapter.

## Part 1 Overall Design Concept

Before entering the unit circuits, the project must first explain:

- the design target
- the overall technical route
- the signal-processing chain
- the complete circuit schematic

This part provides the system-level context for all later module descriptions.

## Part 2 Unit-Circuit Design, Simulation, And Debugging

Each unit circuit should form one complete engineering record.

Recommended internal order:

`circuit design -> parameter calculation -> component selection -> simulation result -> debugging and measured result`

The current unit list is:

- sine-wave drive circuit
- AC full bridge and zero adjustment
- three-op-amp high-CMRR amplifier
- square-wave converter
- switch-type full-wave phase-sensitive demodulator
- phase shifter
- low-pass filter
- DC amplifier

This arrangement avoids repetition. The reader can finish one circuit and immediately see its principle, computation, simulation, and practical verification in the same place.

## Part 3 Bill Of Materials

The material list should remain independent from the circuit-analysis text.

It should summarize:

- active devices
- passive devices
- instruments and auxiliary materials

## Writing Rule

The working rule of the repository is now:

`overall first -> circuit unit closed loop second -> materials last`
