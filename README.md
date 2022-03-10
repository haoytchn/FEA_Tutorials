# Introduction
介绍

# Contents

-  <u>LS-Dyna</u>

- <u>ABAQUS</u>
- <u>Optistruct</u>
- <u>Nastran</u>



----

## ABAQUS

### Standard: 

- 能够广泛领域的线性和非线性问题，包括静态分析、动态分析，以复杂的非线性耦合物理场分析等。

- 是一个通用模块，它采用**隐式求解**方法。在每个求解增量步中，隐式地求解方程组。

### Explicit:

- 适用于求解非线性动力学问题和准静态问题，特别是用于模拟短暂、瞬时的动态事件，如冲击和爆炸问题。此外，它对处理接触条件变化的高度非线性问题也非常有效（例如模拟成形问题）。

- 可以进行**显示动态分析**。在时间域中以很小的时间增量步向前推出结果，而无需在每个增量步求解耦合的方程系统或者生成总体刚度矩阵。

## LS-Dyna

### Explicit:

LS-Dyna以Lagrange算法为主，兼有ALE和Euler算法；以显式求解为主，兼有隐式求解功能；以结构分析为主，兼有热分析、流体结构耦合功能；以非线性动力分析为主，兼有静力分析功能；是通用的**结构分析非线性**有限元程序。

### Implicit:

通过如下关键字激活

`*CONTROL_IMPLICIT_GENERAL`显、隐式分析切换

`*CONTROL_IMPLICIT_AUTO`自动调整隐式分析时间步

`*CONTROL_IMPLICIT_SOLUTION`隐式求解控制卡片

### MPP：

Linux：Open MPI, Platform MPI(PBS)

Windows：Microsoft MPI

## Optistruct

### Structure:



### Topology:



## Nastran

SOL 101 静力学分析

SOL 103 正则模态分析

SOL 105 屈曲分析（惯性释放）

SOL 111 模态频率响应分析

SOL 112 模态瞬态频率响应分析

SOL400/600 非线性分析

### 疲劳分析 - 应变-寿命法(e-N)：

E-N曲线：强度系数[Sf]、强度指数[b]、延性指数[c]、延性系数[Ef]、循环应变硬化Exp [n]、循环强度系数[K’]和反向截止[Nc]

疲劳结果：寿命、损伤、E-N法最大/最小应变、安全系数等

