# 嵌入式开发 · 项目索引

集美大学 · 电子信息工程 · 单片机与嵌入式系统

做的都是「让一块 MCU 把物理世界的事情干对」的活：电机控制、姿态解算、
传感器采集、实时调度。下面每个仓库都带完整 README——硬件配置、实测参数、
控制架构、已知限制，不只是代码。

---

## 智能汽车竞赛

### [TC264_BALANCE_MOTO](https://github.com/hhttiuui/TC264_BALANCE_MOTO)

平衡摩托整车控制，Infineon TC264D 双核。Mahony 四元数姿态解算、
**2/4/8 ms 三层多速率 PID**、GNSS 导航、惯导 Pure Pursuit 路径跟踪、
12 种赛事模式。1 ms 定时器分五槽调度，yaw 外环带俯仰门控
（|pitch| > 30° 时不追航向）。

`TC264D` `TASKING` `Mahony AHRS` `多速率 PID` `GNSS`

---

## 电子设计竞赛

### [TC264_BALL_BALANCE](https://github.com/hhttiuui/TC264_BALL_BALANCE)

板球平衡：摄像头识别小球位置 → UART3 → 舵机带动连杆调平台倾角。
复用平衡摩托的底盘代码，把航向外环换成**小球位置外环**。
连杆有五种模式：位置闭环 / 段表开环 / 遥控直驱 / 录制 / 回放。

`TC264D` `视觉反馈` `舵机闭环` `Flash 参数存储`

### [MSPM0C1104_LINE_SENSOR](https://github.com/hhttiuui/MSPM0C1104_LINE_SENSOR)

8 路红外巡线传感器采集与标定。ADC 只有 4 个 MEM 槽，所以把 8 路拆成
**奇偶两组交替采集**；4 次采样取均值后做**上下限双阈值滞后判决**，
抑制黑白边界抖动。按键现场校准，阈值存 Flash。

`MSPM0C1104` `12-bit ADC` `序列模式` `双阈值迟滞`

### [MSPM0G3507_TRACK_CAR](https://github.com/hhttiuui/MSPM0G3507_TRACK_CAR)

四轮循迹小车。速度环 V2（含目标斜坡与前馈、抗积分饱和回算）、
IMU660RB + Fusion AHRS 航向保持、五种运行模式（航向保持直行 / 正方形循迹 /
循迹定距双段停车等）、Si24R1 2.4G 无线、OLED 四级菜单。

`MSPM0G3507` `速度环` `航向保持` `Si24R1`

---

## 骑行 / 车载

### [MSPM0G3507_FLUID](https://github.com/hhttiuui/MSPM0G3507_FLUID)

单车尾灯。HT16D33A 驱动的 16×16 灰度点阵上跑流体模拟，另有心跳动画、
文字滚动、图片/动图槽位。跟重力自动转 90°/180°，带电池电量检测与省电模式。
配了一个零依赖的单文件网页上位机（图片/文字/手绘 → 帧数据，Web Serial 直推）。

`MSPM0G3507` `HT16D33A` `流体模拟` `Web Serial`

### [STM32F407_BIKE_MONITOR](https://github.com/hhttiuui/STM32F407_BIKE_MONITOR)

骑行状态监控。核心是**摔车检测**——MPU6050 加速度超阈值即点亮三面 LED
并鸣喇叭警示。另有超声波测距、轮速测量、骑行计时、OLED 显示、
蓝牙把速度和时长上报手机。

`STM32F407` `MPU6050` `超声波测距` `蓝牙`

---

## 电机控制（代码未公开）

### MSPM0G3507 FOC 伺服驱动 · 实验室项目

基于 MSPM0G3507 + DRV8316 + MT6701 的无刷电机 FOC 驱动器。
**16 kHz 事件驱动**控制环，热路径**纯定点、零浮点**（M0+ 无 FPU）；
中断只做采样、FOC 计算放主循环；调试器与串口收敛到同一条命令通道，
`dbg` 镜像恒等于实际生效值。电流环已定稿，串级外环在推进。

代码按实验室约定不公开，架构与设计取舍整理在 **[架构说明](foc-notes.md)**。

`MSPM0G3507` `DRV8316` `MT6701` `FOC` `定点运算` `IQmath`

---

## 技术栈

| 分类 | 内容 |
|---|---|
| **MCU** | TI MSPM0G3507 / MSPM0C1104、Infineon TC264D、STM32F407 |
| **电机控制** | FOC、串级 PID、多速率调度、编码器测速、换相标定 |
| **姿态解算** | Mahony 四元数、Fusion AHRS、IMU 标定与滤波 |
| **工具链** | CCS（Theia）、AURIX Development Studio / TASKING、Keil MDK |
| **通信** | UART / SPI / I2C / CAN、2.4G（Si24R1）、蓝牙、GNSS |
| **实时性** | 中断驱动分槽调度、事件触发、DMA、定点化（IQmath） |

---

## 说明

电机 FOC 驱动的代码按实验室约定不公开，架构与设计取舍见上文「架构说明」。

仓库里的代码以竞赛与课程作品为主，每个 README 都写明了已知限制和改进方向——
如果对某处实现有疑问或者发现错误，欢迎开 issue。
