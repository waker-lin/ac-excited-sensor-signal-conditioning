# Final Report Outline

## Structure Rule

This project report should follow this confirmed logic:

`overall design concept -> unit-circuit design, simulation, and debugging -> bill of materials`

The intention is to keep the whole report compact and avoid splitting simulation and practical verification into a separate large chapter.

## Clean Chapter Outline

### Chapter 1 Overall Design Concept

#### 1.1 Design Objectives
- measurement target
- signal characteristics
- output target

#### 1.2 Overall Design Concept
- why AC excitation is used
- why synchronous demodulation is used
- signal-chain design logic

#### 1.3 System Signal Flow
- excitation path
- sensing path
- demodulation path
- output path

#### 1.4 Overall Circuit Schematic
- complete circuit figure
- module partitioning in the full schematic
- correspondence between blocks and actual circuit units

### Chapter 2 Unit-Circuit Design, Simulation, And Debugging

#### 2.1 Sine-Wave Drive Circuit
- 2.1.1 Circuit Design
- 2.1.2 Parameter Calculation
- 2.1.3 Component Selection
- 2.1.4 Simulation Results
- 2.1.5 Debugging And Measured Results

#### 2.2 AC Full Bridge And Zero Adjustment
- 2.2.1 Circuit Design
- 2.2.2 Zeroing Method
- 2.2.3 Output Characteristic Analysis
- 2.2.4 Simulation Results
- 2.2.5 Debugging And Measured Results

#### 2.3 Three-Op-Amp High-CMRR Amplifier
- 2.3.1 Circuit Design
- 2.3.2 Parameter Calculation
- 2.3.3 Component Selection
- 2.3.4 Simulation Results
- 2.3.5 Debugging And Measured Results

#### 2.4 Square-Wave Converter
- 2.4.1 Circuit Design
- 2.4.2 Component Selection
- 2.4.3 Simulation Results
- 2.4.4 Debugging And Measured Results

#### 2.5 Switch-Type Full-Wave Phase-Sensitive Demodulator
- 2.5.1 Circuit Design
- 2.5.2 Component Selection
- 2.5.3 Simulation Results
- 2.5.4 Debugging And Measured Results

#### 2.6 Phase Shifter
- 2.6.1 Circuit Design
- 2.6.2 Parameter Calculation
- 2.6.3 Simulation Results
- 2.6.4 Debugging And Measured Results

#### 2.7 Low-Pass Filter
- 2.7.1 Circuit Design
- 2.7.2 Component Selection
- 2.7.3 Parameter Calculation
- 2.7.4 Simulation Results
- 2.7.5 Debugging And Measured Results

#### 2.8 DC Amplifier
- 2.8.1 Circuit Design
- 2.8.2 Parameter Calculation
- 2.8.3 Simulation Results
- 2.8.4 Debugging And Measured Results

### Chapter 3 Bill Of Materials

#### 3.1 Active Devices
#### 3.2 Passive Devices
#### 3.3 Instruments And Auxiliary Materials

## Why This Structure Works Better

This structure avoids two common problems:

- simulation and practical verification are no longer separated from the circuit they belong to
- the report does not repeat the same module twice in different chapters

Each unit circuit now forms one closed engineering loop:

`principle -> calculation -> simulation -> debugging -> measured result`
