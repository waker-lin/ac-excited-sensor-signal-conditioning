# Final Report Outline

## Structure Rule

This project report should follow one clear logic:

`overall design concept -> overall circuit schematic -> unit-circuit design and simulation -> hardware implementation and debugging -> material list`

The numbering below fixes the current inconsistencies and establishes one stable writing structure for the repository and the final report.

## Clean Chapter Outline

### Chapter 3 Overall Design Concept And System Schematic

#### 3.1 Design Objectives
- measurement target
- signal characteristics
- output target

#### 3.2 Overall Design Concept
- why AC excitation is used
- why synchronous demodulation is used
- signal-chain design logic

#### 3.3 Overall Signal Flow
- excitation path
- sensing path
- demodulation path
- output path

#### 3.4 Overall Circuit Schematic
- complete circuit figure
- module partitioning in the full schematic
- correspondence between blocks and actual circuit units

### Chapter 4 Unit Circuit Design And Simulation

#### 4.1 Sine-Wave Drive Circuit
- 4.1.1 Circuit Design
- 4.1.2 Parameter Calculation
- 4.1.3 Component Selection
- 4.1.4 Simulation Results

#### 4.2 AC Full Bridge And Zero Adjustment
- 4.2.1 Circuit Design And Zeroing Method
- 4.2.2 Output Waveform And Balance Result

#### 4.3 Three-Op-Amp High-CMRR Amplifier
- 4.3.1 Circuit Design
- 4.3.2 Parameter Calculation
- 4.3.3 Component Selection
- 4.3.4 Simulation Results

#### 4.4 Square-Wave Converter
- 4.4.1 Circuit Design
- 4.4.2 Component Selection
- 4.4.3 Simulation Results

#### 4.5 Switch-Type Full-Wave Phase-Sensitive Demodulator
- 4.5.1 Circuit Design
- 4.5.2 Component Selection
- 4.5.3 Simulation Results

#### 4.6 Phase Shifter
- 4.6.1 Circuit Design
- 4.6.2 Parameter Calculation
- 4.6.3 Simulation Results

#### 4.7 Low-Pass Filter
- 4.7.1 Circuit Design
- 4.7.2 Component Selection
- 4.7.3 Parameter Calculation
- 4.7.4 Simulation Results

#### 4.8 DC Amplifier
- 4.8.1 Circuit Design
- 4.8.2 Parameter Calculation
- 4.8.3 Simulation Results

### Chapter 5 Hardware Implementation And Debugging

#### 5.1 Physical Construction
- board implementation
- device installation
- wiring and power arrangement

#### 5.2 System Integration
- module interconnection
- signal-chain bring-up
- initial functional verification

#### 5.3 Circuit Debugging
- 5.3.1 Sine-Wave Drive Circuit Debugging
- 5.3.2 Bridge Circuit Debugging
- 5.3.3 Three-Op-Amp High-CMRR Amplifier Debugging
- 5.3.4 Square-Wave Converter Debugging
- 5.3.5 Phase-Sensitive Demodulator Debugging
- 5.3.6 Low-Pass Filter Debugging
- 5.3.7 DC Amplifier Debugging

### Appendix A Bill Of Materials
- A.1 Active devices
- A.2 Passive devices
- A.3 Connectors, board, and wiring materials
- A.4 Instruments used in testing

## Numbering Fixes Applied

The following numbering issues are corrected in this outline:

- `4.2 全桥电桥及其调零` under-item `4.3.1` is corrected to `4.2.1`
- `4.6 移相器` gains the missing `4.6.1 Circuit Design`
- `4.8 直流放大电路` is normalized to `4.8.1`, `4.8.2`, `4.8.3`
- `6.3.7 直流放大电路调试` is corrected to `5.3.7`
- the material list is moved out of the main chapters and placed in `Appendix A`

## Writing Recommendation

For Chapter 3, keep the internal writing order consistent:

`design target -> technical route -> signal flow -> overall circuit schematic`

For each unit circuit in Chapter 4, keep the internal writing order consistent:

`design purpose -> circuit principle -> parameter calculation -> component selection -> simulation result`

For each debugging item in Chapter 5, keep the internal writing order consistent:

`target -> observed problem -> adjustment method -> final result`
