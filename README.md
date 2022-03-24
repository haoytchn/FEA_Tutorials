# Introduction
һЩ����Ԫ����ģ��ʵ����

ǰ����һ��ʹ��hypermesh��Ls-Dyna����ʹ��Ls-Prepost��

### ��������

- ����ѧ���� - ���ԡ�������
- ����ѧ����
- ģ̬���� - ����
  - ����ģ̬����
  - г��Ӧ����
  - ģ̬˲̬����
- ������������
- �ȴ�������
- ������Ϸ���
- ���嶯��ѧ����
  - ���嶯��ѧ����
  - ������Ϸ���
- ��ʾ����ѧ����
  - ���ͷ���
  - ��������
- �Ż�����

### һ��Ӧ��

- ABAQUS Standard - �����Ծ�������
  1. mod_close_static.inp - ���Ź������������Է���
- Nastran - ���Է�����������
  1. xxx.bdf - �նȷ���
  2. xxx.bdf - �����ͷŷ�ǿ�ȷ���
  3. xxx.bdf - ģ̬����
  4. xxx.bdf - Ƶ����Ӧ����

- LS-Dyna - ��ʾ���������Է���
- Optistruct - �ṹ�Ż���ģ̬��Ƶ�������Nastran��
- Adams - ���嶯������ - ���塢������

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

- �ܹ��㷺��������Ժͷ��������⣬������̬��������̬�������Ը��ӵķ�����������������ȡ�

- ��һ��ͨ��ģ�飬������**��ʽ���**��������ÿ������������У���ʽ����ⷽ���顣

### Explicit:

- �������������Զ���ѧ�����׼��̬���⣬�ر�������ģ����ݡ�˲ʱ�Ķ�̬�¼��������ͱ�ը���⡣���⣬���Դ���Ӵ������仯�ĸ߶ȷ���������Ҳ�ǳ���Ч������ģ��������⣩��
- ���Խ���**��ʾ��̬����**����ʱ�������Ժ�С��ʱ����������ǰ�Ƴ��������������ÿ�������������ϵķ���ϵͳ������������նȾ���

### ��Ԫ���ͣ�

- �ǵ�Ԫ��S3, S4(����ն���ȫ���֣����иն���������), S4R(��������) -- S ��СӦ�䵥Ԫ
- �嵥Ԫ��C3D4, C3D6, C3D8
- �������ֵ�Ԫ���ܴ���ɳ©������������ɳ©���ƣ���ȫ���ֵ�Ԫ���ܴ��ڼ���������������������abaqus�ڲ�����������S4��Ԫ�����������������á�һ��ʹ��S4��Ԫ��
- 1D:
  - BEAM: B31
  - COUP_KIN - WASHER
  - KINCOUP - ����MASS��
  - MASS
  - SPRING2 - ����

### ���ϣ�

- ���Բ���
  - [x] Density
    - Density - 1e-09
  - [x] Elastic
    - Type - ISOTROPIC
      - E - 210000
      - NU - 0.3
- �����Բ���
  - [x] Density
    - Density - 1e-09
  - [x] Elastic
    - Type - ISOTROPIC
      - E - 210000
      - NU - 0.3
  - [x] Plastic - Hardening
    - Type - ISOTROPIC
    - PLASTICDATA

### ���ԣ�

- *MASS
- BEAM SECTION - beam����ͨ��hyperbeam����
  - No auto prefix for names
  - SectionType - CIRC
  - Section Axis
- SHELL SECTION
  - No auto prefix for names
  - Thickness
- SOLID SECTION
  - No auto prefix for names

### ���ӣ�

- ���㣺acm��Ԫ��diameter=6mm

- ճ����ADHESIVE��Ԫ

- ��˨��kincoup���Ե�Ԫ

### �Ӵ���

- *SURFACE
- *CONTACT PAIR
- *SURFACE INTERACTION - ������

### ���أ�

- SPC - *BOUNDARY - INITIAL_CONDITION
- LOAD - *CLOAD - HISTORY
- LOAD STEP - *STEP
  - �Ӵ�����ѡĬ��ȫ����Ч
  - Loadcol - ��ѡ��INITIAL����
  - Outputblock - ���
  - Procedure - Static
    - [x] Dataline

### �����

- *OUTPUT
  - HISTORY
  - PRESELECT

### ����

- S-Stress components(s) - Mises - Advanced Ӧ����S, Mises��ʵӦ��
- PEEQ-Equivalent plastic strain - Scalar value - Advanced Ӧ�����Ч����Ӧ��PEEQ��>0�����������
- ���²ο���
  - ��LE��ʵӦ��
  - ��EE����Ӧ��
  - ��NE����Ӧ��
  - ��ʽ�����޷�׼ȷģ�����Ա��ι����ƻ����̣���Ҫ����ʾ����

### Tips��

- abaqus��֧�� "."�����򱨴�����ʱҪ����
- �Ӵ����������

----

## Nastran

SOL 101 ����ѧ����

SOL 103 ����ģ̬����

SOL 105 ���������������ͷţ�

SOL 111 ģ̬Ƶ����Ӧ����

SOL 112 ģ̬˲̬Ƶ����Ӧ����

SOL400/600 �����Է���

```NASTRAN
SOL 101
TIME   6000.0
��
ִ�п���Executive Control��������͡�ʱ������ϵͳ��ϣ�
CEND

�������Case Control�����Ҫ��ѡ��ģ�����ݼ���Ŀ��

BEGIN BULK
����ģ��Bulk Data���ṹģ�Ͷ��壬�������������

ENDDATA
```

### ��Ԫ���ͣ�

- �ǵ�Ԫ��
  - CQUAD4, Quadrilateral Plate Element Connection
  - CTRIA3, Four-Sided Solid Element Connection

- �嵥Ԫ��
  - CTETRA, Four-Sided Solid Element Connection
  - CHEXA, Six-Sided Solid Element Connection

- 1D:
  - CBEAM - BEAM
  - CGAP - Gap Element Connection�� ��϶��Ԫ, ��Ҫ����PGAP����ն�KA -1000
  - CBUSH - Generalized Spring-and-Damper Connection, ���Է����Ե��ɻ�����, ��Ҫ��������PBUSH, K1-K3
    - PBUSH - Generalized Spring-and-Damper Property
  - RBE2 - WASHER, ����RBE2�以������
  - CONM2 - Concentrated Mass Element Connection, Rigid Body Form - MASS��

### ���ϣ�

- ���Բ��� - MAT1

### ���ԣ�

- PSHELL
- PSOLID
- PBEAM - Beam Property
- PBEAM - Beam Cross-Section Property

### ���ӣ�

- ���㣺acm��Ԫ��diameter=6mm
- ճ����ADHESIVE��Ԫ

- ��˨��kincoup���Ե�Ԫ

### �����ͷţ�

�����ͷž����ýṹ�Ĺ�������ƽ�����������ܽṹû��Լ��������ʱ�Լ����䴦��һ�֡���̬����ƽ��״̬��(��ֻ����������ƽ����������ܹ�������й��ԣ�������ƽ�⣩�Ľṹ��Ӧ���ֲ���)

���غ���Լ���ṹ��Ҫ��һ������Ծ�����ⷽ��������Ϊ�鹹Լ���������ṹ����λ�ơ��鹹Լ�������λ����ʵ������ʱ�ṹ��û�б��ε�����

- Parameters 
  - INREL - "-2" - ����Զ�����

### ���أ�

- LOAD - ���������(����)
  - GRAV - Acceleration or Gravity Load
  - FORCE - Static Force
  - FORCE1 - �����������ƻ��β�ƣ�������������α䶯
  - MOMENT - Static Moment

- SUBCASE - Load Steps �����ָ� - Case Control Commands
  - OUTPUT
  - DISPLACEMENT
  - STRESS
  - ANALYSIS - Analysis type - Linear Static;Normal Modes


### ���ƣ�

- Executive Control
  - SOL 101 - �������
  - TIME   6000.0 - �������ʱ��

- Parameters - ��������
  - POST - "-2" - �������

- Parameters - �����ͷ�
  - AUTOSPC - Yes
  - INREL - "-2" - �����ͷŷ����Զ�Լ������������λ��
  - K6ROT - 100 - �նȳͷ�ϵ��
  - POST - "-2" - �������


### ƣ�ͷ��� - Ӧ��-������(E-N)��

E-N���ߣ�ǿ��ϵ��[Sf]��ǿ��ָ��[b]������ָ��[c]������ϵ��[Ef]��ѭ��Ӧ��Ӳ��Exp [n]��ѭ��ǿ��ϵ��[K��]�ͷ����ֹ[Nc]

ƣ�ͽ�������������ˡ�E-N�����/��СӦ�䡢��ȫϵ����

### ƣ�ͷ��� - Ӧ��-������(S-N)��

���ط��������Ӧ������������ǿ�ȵ�50%��

��ȷ������S-N����

----

## LS-Dyna

### Explicit:

LS-Dyna��Lagrange�㷨Ϊ��������ALE��Euler�㷨������ʽ���Ϊ����������ʽ��⹦�ܣ��Խṹ����Ϊ���������ȷ���������ṹ��Ϲ��ܣ��Է����Զ�������Ϊ�������о����������ܣ���ͨ�õ�**�ṹ����������**����Ԫ����

### Implicit:

ͨ�����¹ؼ��ּ���

`*CONTROL_IMPLICIT_GENERAL` �ԡ���ʽ�����л�

`*CONTROL_IMPLICIT_AUTO` �Զ�������ʽ����ʱ�䲽

`*CONTROL_IMPLICIT_SOLUTION` ��ʽ�����ƿ�Ƭ

### MPP:

Linux��Open MPI, Platform MPI(PBS)

Windows��Microsoft MPI

----

## Optistruct

### Structure:



### Topology:



---

## Adams

### �غ���ȡ��



### ����壺



### �����壺




> **ע��**
> 
> 1. Nastran����������Թ���ԱȨ�����С�
> 2. ƣ�ͷ��� ncode - Nastran��fe-safe - Abaqus��

---

# Theory

## ��ʽ��� - ���նȾ���



## ��ʾ��� - ���Ĳ���㷨

## ��������

### ���ϵ�ʧЧ

��ʽ��ĥ�𡢸�ʴ����������

���ͣ����Զ��ѣ����Ժ�����Ա��Σ������Զ���

ƣ�ͣ�����ƣ�ͦң���s��Nf>10^5������ƣ�ͣ������Ա��Σ��ҡݦ�s��Nf=10^2~10^5

### ����ʧЧ׼��

��һǿ������ - �����Ӧ������ - ���Ӧ��

�ڶ�ǿ������ - ����쳤��Ӧ������ - ������

����ǿ������ - �����Ӧ������ - Trescaǿ�Ȧ�max

����ǿ������ - ��״�ı�������� - von misesǿ��

## Ƶ�ʺ�ģ̬

### ΪʲôҪ�������Ƶ�ʺ�ģ̬

1. �����ṹ�Ķ���ѧ���ԡ��簲װ�ڽṹ�ϵ���ת�豸��Ϊ�����������񶯣����뿴ת��������Ƶ���Ƿ�ӽ��ṹ���κ�һ�׹���Ƶ�ʡ�
2. �����غɵĿ��ܷŴ����ӡ�
3. ʹ�ù���Ƶ�ʺ�����ģ̬������ָ��������̬��������˲̬��������Ӧ�׷�����˲̬������ʱ�䲽����t��ѡȡ��)
4. ʹ�ù���Ƶ�ʺ�����ģ̬���ڽṹ˲̬����ʱ��������ģ̬���ŷ�
5. ָ��ʵ�����������ٶȴ������Ĳ���λ�á�
6. �������

### Ƶ����Ӧ����

1. �����𵴼�������Ӧ
2. ������Ƶ������ʽ���壬��ÿƵ�ʵ���������֪
3. �������Ӧͨ�������ڵ�λ�ơ���Ԫ����Ӧ��
4. �������ӦΪ�������ɴ�С����λ����
5. Ƶ����Ӧ������Ϊֱ�ӷ���ģ̬����

