# Introduction
一些有限元仿真模型实例。

前处理一般使用hypermesh；Ls-Dyna部分使用Ls-Prepost。

### 分析类型

- 静力学分析 - 线性、非线性
- 动力学分析
- 模态分析 - 线性
  - 正则模态分析
  - 谐响应分析
  - 模态瞬态分析
- 线性屈曲分析
- 热传导分析
- 热力耦合分析
- 多体动力学分析
  - 刚体动力学分析
  - 刚柔耦合分析
- 显示动力学分析
  - 成型分析
  - 切削分析
- 优化分析

### 一般应用

- ABAQUS Standard - 非线性静力分析
  1. mod_close_static.inp - 车门过开静力非线性分析
- Nastran - 线性分析静力分析
  1. xxx.bdf - 刚度分析
  2. xxx.bdf - 惯性释放法强度分析
  3. xxx.bdf - 模态分析
  4. xxx.bdf - 频率响应分析

- LS-Dyna - 显示动力非线性分析
- Optistruct - 结构优化；模态、频响分析（Nastran）
- Adams - 多体动力分析 - 刚体、柔性体

---

# Contents

-  [<u>ABAQUS</u>](#ABAQUS)
-  [<u>Nastran</u>](#Nastran)
-  [<u>LS-Dyna</u>](#LS-Dyna)
-  [<u>Optistruct</u>](#Optistruct)
-  [<u>Adams</u>](#Adams) 

----

# Application

## Abaqus

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
  - COUP_KIN - WASHER
  - KINCOUP - 连接MASS点
  - MASS
  - SPRING2 - 弹簧

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
- BEAM SECTION - beam属性通过hyperbeam建立
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
- *SURFACE INTERACTION - 属性里

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

- S-Stress components(s) - Mises - Advanced 应力→S, Mises真实应力
- PEEQ-Equivalent plastic strain - Scalar value - Advanced 应变→等效塑性应变PEEQ（>0则表明屈服）
- 以下参考：
  - →LE真实应变
  - →EE弹性应变
  - →NE名义应变
  - 隐式分析无法准确模拟塑性变形过大破坏过程，需要用显示分析

### Tips：

- abaqus不支持 "."，否则报错，命名时要避免
- 接触面必须连续

----

## Nastran

SOL 101 静力学分析

SOL 103 正则模态分析

SOL 105 屈曲分析（惯性释放）

SOL 111 模态频率响应分析

SOL 112 模态瞬态频率响应分析

SOL400/600 非线性分析

```NASTRAN
SOL 101
TIME   6000.0
…
执行控制Executive Control（求解类型、时间容许、系统诊断）
CEND

情况控制Case Control（输出要求、选择模型数据集项目）

BEGIN BULK
数据模型Bulk Data（结构模型定义，求解条件参数）

ENDDATA
```

### 单元类型：

- 壳单元：
  - CQUAD4, Quadrilateral Plate Element Connection
  - CTRIA3, Four-Sided Solid Element Connection

- 体单元：
  - CTETRA, Four-Sided Solid Element Connection
  - CHEXA, Six-Sided Solid Element Connection

- 1D:
  - CBEAM - BEAM
  - CGAP - Gap Element Connection， 间隙单元, 需要属性PGAP定义刚度KA -1000
  - CBUSH - Generalized Spring-and-Damper Connection, 线性非线性弹簧或阻尼, 需要定义属性PBUSH, K1-K3
    - PBUSH - Generalized Spring-and-Damper Property
  - RBE2 - WASHER, 允许RBE2间互相连接
  - CONM2 - Concentrated Mass Element Connection, Rigid Body Form - MASS点

### 材料：

- 弹性材料 - MAT1

### 属性：

- PSHELL
- PSOLID
- PBEAM - Beam Property
- PBEAM - Beam Cross-Section Property

### 连接：

- 焊点：acm单元；diameter=6mm
- 粘胶：ADHESIVE单元

- 螺栓：kincoup刚性单元

### 惯性释放：

惯性释放就是用结构的惯性力来平衡外力。尽管结构没有约束，分析时仍假设其处于一种“静态”的平衡状态。(不只适用于外力平衡情况，还能够计算具有惯性（外力不平衡）的结构的应力分布。)

有载荷无约束结构，要按一般的线性静力求解方法必须人为虚构约束，消除结构刚体位移。虚构约束的最佳位置是实际受力时结构中没有变形的区域。

- Parameters 
  - INREL - "-2" - 软件自动定义

### 加载：

- LOAD - 静荷载组合(叠加)
  - GRAV - Acceleration or Gravity Load
  - FORCE - Static Force
  - FORCE1 - 跟随力，类似火箭尾推，方向随网格变形变动
  - MOMENT - Static Moment

- SUBCASE - Load Steps 工况分隔 - Case Control Commands
  - OUTPUT
  - DISPLACEMENT
  - STRESS
  - ANALYSIS - Analysis type - Linear Static;Normal Modes


### 控制：

- Executive Control
  - SOL 101 - 求解类型
  - TIME   6000.0 - 最大运行时间

- Parameters - 静力分析
  - POST - "-2" - 输出设置

- Parameters - 惯性释放
  - AUTOSPC - Yes
  - INREL - "-2" - 惯性释放分析自动约束，消除刚体位移
  - K6ROT - 100 - 刚度惩罚系数
  - POST - "-2" - 输出设置


### 疲劳分析 - 应变-寿命法(E-N)：

E-N曲线：强度系数[Sf]、强度指数[b]、延性指数[c]、延性系数[Ef]、循环应变硬化Exp [n]、循环强度系数[K’]和反向截止[Nc]

疲劳结果：寿命、损伤、E-N法最大/最小应变、安全系数等

### 疲劳分析 - 应力-寿命法(S-N)：

保守方法：最大应力不超过屈服强度的50%。

精确方法：S-N曲线

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



---

## Adams

### 载荷提取：



### 多刚体：



### 柔性体：




> **注：**
> 
> 1. Nastran主程序必须以管理员权限运行。
> 2. 疲劳分析 ncode - Nastran；fe-safe - Abaqus。

---

# Theory

## 隐式求解 - 求解刚度矩阵



## 显示求解 - 中心差分算法

## 金属材料

### 材料的失效

形式：磨损、腐蚀、断裂三种

类型：韧性断裂（明显宏观塑性变形）、脆性断裂

疲劳：高周疲劳σ＜σs，Nf>10^5；低周疲劳（有塑性变形）σ≥σs，Nf=10^2~10^5

### 材料失效准则

第一强度理论 - 最大主应力理论 - 最大应力

第二强度理论 - 最大伸长线应变理论 - 延伸率

第三强度理论 - 最大切应力理论 - Tresca强度τmax

第四强度理论 - 形状改变比能理论 - von mises强度

## 频率和模态

### 为什么要计算固有频率和模态

1. 评估结构的动力学特性。如安装在结构上的旋转设备，为避免其过大的振动，必须看转动部件的频率是否接近结构的任何一阶固有频率。
2. 评估载荷的可能放大因子。
3. 使用固有频率和正交模态，可以指导后续动态分析（如瞬态分析、响应谱分析、瞬态分析中时间步长Δt的选取等)
4. 使用固有频率和正交模态，在结构瞬态分析时，可以用模态扩张法
5. 指导实验分析，如加速度传感器的布置位置。
6. 评估设计

### 频率响应分析

1. 计算震荡激励的响应
2. 激励在频域中显式定义，在每频率点作用力已知
3. 计算的响应通常包括节点位移、单元力和应力
4. 计算的响应为复数、由大小、相位定义
5. 频率响应分析分为直接法、模态法。

