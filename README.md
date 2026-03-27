# AC-Excited Sensor Signal Conditioning

交流激励式传感器信号调理电路分析与实现。

文档围绕课程设计中的模拟调理主链路展开，按总体设计、单元电路和材料清单三部分组织，重点放在电路原理、参数计算、仿真结果和实测结果的对应关系上。

## 总体设计

系统采用交流激励和同步检波路线：

- 用正弦信号激励交流全桥
- 将应变引起的电阻变化转换为微弱交流差动信号
- 用高共模抑制比放大电路完成前级差模放大
- 用方波参考和移相器完成同步控制
- 经相敏检波、低通滤波和直流放大得到最终输出

整体流程图：

![Overall design flow](./images/system_design_flow.png)

总体电路图：

![Overall circuit simulation](./images/overall_circuit_simulation.png)

## 单元电路

1. 正弦驱动电路
2. 交流全桥及调零电路
3. 三运放高共模抑制比放大电路
4. 方波转换电路
5. 开关式全波相敏检波电路
6. 移相器
7. 低通滤波器
8. 直流放大电路

各单元统一按下列顺序展开：

`电路设计 -> 参数计算 -> 器件选型 -> 仿真结果 -> 调试与实测结果`

## 关键关系

相敏检波部分的核心关系为：

`v_s(t) = A(x)cos(ωt + φ)`

`v_r(t) = cos(ωt)`

低通后保留与参考同相的平均分量，因此最终输出满足：

`V_out ∝ A(x)cos(φ)`

## 文档入口

总体部分：

- [docs/01_project_overview.md](./docs/01_project_overview.md)
- [docs/02_system_architecture.md](./docs/02_system_architecture.md)
- [reports/final_report.md](./reports/final_report.md)

单元电路部分：

- [docs/modules/01_sine_wave_generator.md](./docs/modules/01_sine_wave_generator.md)
- [docs/modules/02_ac_full_bridge.md](./docs/modules/02_ac_full_bridge.md)
- [docs/modules/03_three_op_amp_amplifier.md](./docs/modules/03_three_op_amp_amplifier.md)
- [docs/modules/04_square_wave_converter.md](./docs/modules/04_square_wave_converter.md)
- [docs/modules/05_phase_sensitive_demodulator.md](./docs/modules/05_phase_sensitive_demodulator.md)
- [docs/modules/06_phase_shifter.md](./docs/modules/06_phase_shifter.md)
- [docs/modules/07_low_pass_filter.md](./docs/modules/07_low_pass_filter.md)
- [docs/modules/08_dc_amplifier.md](./docs/modules/08_dc_amplifier.md)

材料清单：

- [reports/03_bill_of_materials.md](./reports/03_bill_of_materials.md)
