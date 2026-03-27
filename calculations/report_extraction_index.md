# Report Extraction Index

Source file:

- `D:\大学课程\大三上\传感器课设\传感器课设-林鸿伟\传感器课设说明书-林鸿伟.docx`

## Extracted Figures

The following figures were extracted directly from the Word document and copied into the repository.

- `图2-1 整体设计流程图`
  - raw extracted file: `images/report_extracted/fig_2_1_overall_design_flow.png`
  - convenient project copy: `images/system_design_flow.png`

- `图2-6 整体电路仿真图`
  - raw extracted file: `images/report_extracted/fig_2_6_overall_circuit_simulation.png`
  - convenient project copy: `images/overall_circuit_simulation.png`

- `图4-1 正弦驱动电路`
  - raw extracted file: `images/report_extracted/fig_4_1_sine_drive_circuit.png`
  - module copy: `images/module_figures/sine_wave_generator_schematic.png`

## Extracted Formula Objects

The calculation formulas in the report are not stored as plain Word equations. They are embedded `Equation.DSMT4` objects, so they were extracted as image objects.

Relevant extracted files:

- `calculations/extracted_formulas/eq_4_1_raw.png`
  - equation `(4-1)` near `4.1.2 电路参数计算`
  - context text: `该正弦波振荡电路产生的正弦波谐振频率为`
  - likely meaning: resonance / oscillation frequency formula of the RC bridge oscillator

- `calculations/extracted_formulas/eq_4_1_condition_c_raw.png`
  - context text: `这里让 ...`
  - likely meaning: capacitor equality assumption for the symmetric RC bridge

- `calculations/extracted_formulas/eq_4_1_condition_r_raw.png`
  - context text: `这里让 ...`
  - likely meaning: resistor equality assumption for the symmetric RC bridge

- `calculations/extracted_formulas/eq_4_1_substitution_raw.png`
  - context text: `即 ...`
  - likely meaning: substitution result for the 5 kHz design target

- `calculations/extracted_formulas/eq_4_2_gain_raw.png`
  - equation `(4-2)`
  - context text: `稳幅振荡时放大电路的电压放大倍数为`

- `calculations/extracted_formulas/eq_4_3_feedback_raw.png`
  - equation `(4-3)`
  - context text: `RC串并联正反馈选频网络的反馈系数为`

- `calculations/extracted_formulas/eq_4_12_dc_gain_raw.png`
  - equation `(4.12)` in the DC amplifier section
  - context text: `需要的放大倍数为13.9，即`

## Notes

- The figure extraction is direct and reliable.
- The formula extraction is also direct, but the original report stores these equations as embedded rendered objects, not normal text equations.
- If needed, the next step can be to manually transcribe these extracted formula images into editable Markdown equations and insert them into the corresponding module files.
