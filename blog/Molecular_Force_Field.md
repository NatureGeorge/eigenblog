---
layout: default
date: 2020-04-03
author: Zefeng Zhu
---

# Molecular Force Field

## Bond Stretching

### 谐振子函数

$E_s=\cfrac{1}{2}k_s(l-l_0)^2$

* $l_0$: 平衡键长
* $l$: 键长
* $k_s$: 键伸缩力常数


### Morse Function

$E_s=D_e[\exp(-A(l-l_0))-1]^2$

## Angle Bending

* in plane
* out of plane
  * improper torsion

### 谐振子模型

$E_B=\cfrac{1}{2}k_b(\theta-\theta_0)^2$

* $\theta_0$: 平衡键角
* $\theta$: 键角
* $k_b$: 键角弯折力常数

> 谐振子模型在偏离平衡位置不大的情况下($10\degree$以内)可以取得很好的结果, 若偏离过度，则该模型预测值与实验值差异较大

## Torsion Rotation

> 二面角扭转能

$E_T=\sum^{N}_{n=0}\cfrac{V_n}{2}[1+\cos(n\omega-\gamma)]$

## Crossing Terms

> 交叉相互作用项

* 键伸缩-键伸缩相互作用
* 键伸缩-键角弯折相互作用
* 键伸缩-二面角旋转相互作用
* 键角弯折-键角弯折相互作用
* 键角弯折-二面角旋转相互作用

## 范德华相互作用能

### Lennard-Jones势函数

$E_{LJ}=\varepsilon[(\cfrac{r_0}{r})^{m}-\cfrac{m}{n}(\cfrac{r_0}{r})^n]$

## 静电相互作用

### 点电荷法

$V_{\text{chg}}=K\cfrac{q_iq_j}{\varepsilon r_{ij}}$

### 偶极矩

$V_{\text{dipole}}=K\cfrac{\mu_{i}\mu_{j}}{\varepsilon r^{3}_{ij}}(\cos\chi-3\cos\alpha_i\cos\alpha_j)$

## Hydrogen Bond

..

## Conjugate Systems

..

---

## 传统力场

### AMBER力场

> Kollman group, 1984

$$E=\sum_{i\in\text{bonds}} E_{si}+\sum_{i\in\text{angles}} E_{Bi}+\sum_{i\in\text{torsions}} E_{Ti}+\sum_{j=1}^{N-1}\sum_{i=j+1}^{N} f_{ij}\left\{ E_{12,6}+V_{\text{chg}}\right\}$$

$$E_{si}=\cfrac{1}{2}k_{si}(l_{i}-l_{i}^{0})^2$$

$$E_{Bi}=\cfrac{1}{2}k_{bi}(\theta_{i}-\theta_{i}^{0})^2$$

$$E_{Ti}=\sum_{n}\cfrac{V_{i}^{n}}{2}[1+\cos(n\omega_{i}-\gamma_{i})]$$

$$E_{12,6}=\varepsilon_{ij}[(\cfrac{r^{0}_{ij}}{r_{ij}})^{12}-2(\cfrac{r^{0}_{ij}}{r_{ij}})^6]$$

$$V_{\text{chg}}=K\cfrac{q_iq_j}{\varepsilon_{0} r_{ij}}, \,K=\cfrac{1}{4\pi}$$

### CHARMm力场

> Karplus group, 1983

$E=E_s+E_B+\sum_{\phi}$

### CVFF力场

...

### MMX力场

...

### CFF力场

...

### COMPASS力场

...

### MMFF94力场

> Hagler, 1996

### 力场的选择

* 蛋白质分子的模拟
  * AMBER
  * CHARMm
  * CFF
  * CVFF
  * MMFF94
* 核酸分子
  * AMBER
  * CHARMm
  * MMFF94
* 小分子-蛋白复合物
* 高分子

### 力场的参数化

* 分子力学力场的性能主要取决于势能函数和结构参数

## 分子力学应用

* 分子动力学
* 构象能力搜寻
* 分子对接