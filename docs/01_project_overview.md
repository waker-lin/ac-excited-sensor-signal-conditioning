# 01 Project Overview

## Project Positioning

This repository is a circuit-project archive rather than a general study notebook.

Its purpose is to document one complete signal-conditioning system from five engineering angles:

- principle
- calculation process
- simulation
- practical implementation
- final summary

## Core Deliverables

The repository is expected to answer two levels of questions.

### A. Module Level

For each circuit block, the repository should provide:

- schematic
- principle explanation
- parameter calculation and calculation process
- simulation waveform
- practical result

### B. System Level

For the complete chain, the repository should explain:

- how the signal enters the system
- what each stage does to the signal
- how the waveform evolves
- why the final output can represent the measured quantity

## Why This Structure Matters

If the repository contains only schematics, it is difficult to reuse.

If it contains only formulas, it is difficult to verify.

If it contains only waveforms, it is difficult to understand.

So the proper project form is:

`principle -> calculation -> simulation -> practice -> summary`

This sequence is the central organizing rule of the repository.

## Current Circuit Focus

The present archived circuit focuses on an AC-excited signal-conditioning chain that includes:

- sine-wave generation
- AC full bridge sensing stage
- three-op-amp high-CMRR amplification
- square-wave reference generation
- phase-sensitive demodulation
- low-pass filtering
- DC amplification
- final display interface

## Repository Use

This repository should be readable in two ways:

- by module: when checking one circuit block in detail
- by signal chain: when understanding the complete processing path from input to output

That dual readability is the design target of the documentation itself.
