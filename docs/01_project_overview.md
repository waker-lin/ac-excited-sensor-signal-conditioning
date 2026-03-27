# 01 Project Overview

## Project Positioning

This repository is a circuit-project archive focused on one complete signal-conditioning system.

The preferred reading structure is now divided into two parts.

## Part I: Unit Circuits

The main body of the repository is organized by circuit unit. Each major unit should be understandable on its own.

The six core circuit units are:

1. Full bridge
2. Three-op-amp high-CMRR amplifier
3. Square-wave converter
4. Phase-sensitive demodulator
5. Low-pass filter
6. DC amplifier

For each unit, the repository should provide:

- schematic
- principle explanation
- parameter calculation and calculation process
- simulation waveform
- practical result
- short conclusion

This is the primary organizational rule of the repository.

## Part II: Overall System Explanation

After the unit circuits are documented individually, the repository explains how they work together as one full processing chain.

This second part should answer:

- how the signal enters the system
- what each stage does to the signal
- how the waveform evolves from stage to stage
- why the final output can represent the measured quantity

## Why This Structure Matters

If the repository is organized only by the whole chain, then the details of each circuit become hard to track.

If it is organized only by isolated circuits, then the overall logic becomes fragmented.

So the repository uses a two-layer structure:

`Part I: unit circuits`

`Part II: overall chain explanation`

## Supporting Circuit Blocks

The sine-wave generator and the display path are still part of the complete system, but they serve as supporting context around the six core unit circuits listed above.

## Documentation Rule

The working rule of the repository remains:

`principle -> calculation -> simulation -> practice -> summary`

That rule applies first to each individual circuit, and then to the overall system explanation.
