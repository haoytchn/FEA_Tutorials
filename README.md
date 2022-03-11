# Introduction
һЩ����Ԫ����ģ��ʵ����

ǰ����ʹ��hypermesh��

# Contents

-  [<u>ABAQUS</u>](#ABAQUS)
-  [<u>LS-Dyna</u>](#LS-Dyna)
-  [<u>Optistruct</u>](#Optistruct)
- [<u>Nastran</u>](#Nastran)



----

## ABAQUS

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
- BEAM SECTION
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

  ?	S-Stress components(s) - Mises - Advanced Ӧ��

  ?	PEEQ-Equivalent plastic strain - Scalar value - Advanced Ӧ��

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



----

## Nastran

SOL 101 ����ѧ����

SOL 103 ����ģ̬����

SOL 105 ���������������ͷţ�

SOL 111 ģ̬Ƶ����Ӧ����

SOL 112 ģ̬˲̬Ƶ����Ӧ����

SOL400/600 �����Է���

### ƣ�ͷ��� - Ӧ��-������(E-N)��

E-N���ߣ�ǿ��ϵ��[Sf]��ǿ��ָ��[b]������ָ��[c]������ϵ��[Ef]��ѭ��Ӧ��Ӳ��Exp [n]��ѭ��ǿ��ϵ��[K��]�ͷ����ֹ[Nc]

ƣ�ͽ�������������ˡ�E-N�����/��СӦ�䡢��ȫϵ����

### ƣ�ͷ��� - Ӧ��-������(S-N)��

���ط��������Ӧ������������ǿ�ȵ�50%��

��ȷ������S-N����

