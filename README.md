# Introduction
一些有限元仿真模型实例。

前处理使用hypermesh。

# Contents

-  [<u>ABAQUS</u>](#ABAQUS)
-  [<u>LS-Dyna</u>](#LS-Dyna)
-  [<u>Optistruct</u>](#Optistruct)
- [<u>Nastran</u>](#Nastran)



----

## ABAQUS

### Standard: 

- 能够广泛领域的线性和非线性问题，包括静态分析、动态分析，以复杂的非线性耦合物理场分析等。

- 是一个通用模块，它采用**隐式求解**方法。在每个求解增量步中，隐式地求解方程组。

### Explicit:

- 适用于求解非线性动力学问题和准静态问题，特别是用于模拟短暂、瞬时的动态事件，如冲击和爆炸问题。此外，它对处理接触条件变化的高度非线性问题也非常有效（例如模拟成形问题）。
- 可以进行**显示动态分析**。在时间域中以很小的时间增量步向前推出结果，而无需在每个增量步求解耦合的方程系统或者生成总体刚度矩阵。

### 单元类型：

- 壳单元：S3, S4(拉伸刚度完全积分；剪切刚度缩减积分), S4R(缩减积分) -- S ：小应变单元
- 体单元：C3D4, C3D6, C3D8
- 缩减积分单元可能存在沙漏，最好设置添加沙漏控制；完全积分单元可能存在剪切自锁（收敛变慢），abaqus内部修正消除了S4单元剪切自锁，无需设置。一般使用S4单元。
- 1D:
  - BEAM: B31

### 材料：

- 弹性材料
  - [x] Density
    - Density - 1e-09
  - [x] Elastic
    - Type - ISOTROPIC
      - E - 210000
      - NU - 0.3
- 弹塑性材料
  - [x] Density
    - Density - 1e-09
  - [x] Elastic
    - Type - ISOTROPIC
      - E - 210000
      - NU - 0.3
  - [x] Plastic - Hardening
    - Type - ISOTROPIC
    - PLASTICDATA

### 属性：

- *MASS
- BEAM SECTION
  - No auto prefix for names
  - SectionType - CIRC
  - Section Axis
- SHELL SECTION
  - No auto prefix for names
  - Thickness
- SOLID SECTION
  - No auto prefix for names

### 连接：

- 焊点：acm单元；diameter=6mm

- 粘胶：ADHESIVE单元

- 螺栓：kincoup刚性单元

### 接触：

- *SURFACE
- *CONTACT PAIR

### 加载：

- SPC - *BOUNDARY - INITIAL_CONDITION
- LOAD - *CLOAD - HISTORY
- LOAD STEP - *STEP
  - 接触不勾选默认全部生效
  - Loadcol - 勾选非INITIAL加载
  - Outputblock - 输出
  - Procedure - Static
    - [x] Dataline

### 输出：

- *OUTPUT
  - HISTORY
  - PRESELECT

### 后处理：

  ?	S-Stress components(s) - Mises - Advanced 应力

  ?	PEEQ-Equivalent plastic strain - Scalar value - Advanced 应变

----

## LS-Dyna

### Explicit:

LS-Dyna以Lagrange算法为主，兼有ALE和Euler算法；以显式求解为主，兼有隐式求解功能；以结构分析为主，兼有热分析、流体结构耦合功能；以非线性动力分析为主，兼有静力分析功能；是通用的**结构分析非线性**有限元程序。

### Implicit:

通过如下关键字激活

`*CONTROL_IMPLICIT_GENERAL` 显、隐式分析切换

`*CONTROL_IMPLICIT_AUTO` 自动调整隐式分析时间步

`*CONTROL_IMPLICIT_SOLUTION` 隐式求解控制卡片

### MPP:

Linux：Open MPI, Platform MPI(PBS)

Windows：Microsoft MPI

----

## Optistruct

### Structure:



### Topology:



----

## Nastran

SOL 101 静力学分析

SOL 103 正则模态分析

SOL 105 屈曲分析（惯性释放）

SOL 111 模态频率响应分析

SOL 112 模态瞬态频率响应分析

SOL400/600 非线性分析

### 疲劳分析 - 应变-寿命法(E-N)：

E-N曲线：强度系数[Sf]、强度指数[b]、延性指数[c]、延性系数[Ef]、循环应变硬化Exp [n]、循环强度系数[K’]和反向截止[Nc]

疲劳结果：寿命、损伤、E-N法最大/最小应变、安全系数等

### 疲劳分析 - 应力-寿命法(S-N)：

保守方法：最大应力不超过屈服强度的50%。

精确方法：S-N曲线

