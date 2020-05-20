---
title: Artificial Intelligence
author: Zefeng Zhu
date: 2020-05-08 18:30:00 +0800
categories: [Notes, Lectures]
tags: [artificial intelligence, machine learning, lie group, lie group machine learning, dynamic fuzzy logic, dynamic fuzzy machine learning, deep learning]
---

## Machine Learning

> Tom M. Mitchell

### Method

> 以认知科学为基础、数学方法为手段、可计算理论为标准、分析数据规律为目标、计算机技术为实现途径，沿着这样的路径，构建机器学习的理论、技术、方法、应用体系 (李凡长)

#### 机器学习5原理

* 机器学习可解释原理
* 机器学习可学习原理
* 机器学习可泛化原理
* 机器学习自学习原理
* 机器学习小样本原理

### 李群机器学习 (Lie Group Machine Learning, LML)

* 数据->特征提取->特征表示->矩阵群->李群表示->李群代数计算->李群机器学习算法->实例

一般使用G表示输入空间，M表示输出域。通常$G\subseteq R^{n}$, 借用李群的定义将G对M的*左作用*可用如下映射表示:

$$
\begin{aligned}
    \varphi:& G\times M\rightarrow M\\
    & g,x\rightarrow\varphi(g,x)
\end{aligned}
$$

要求$\varphi(g_1,\varphi(g_2,x))=\varphi(g_1,g_2,x)$, $\varphi(e,x)=x$

---

满足$\varphi(\varphi(g_1,x),g_2)=\varphi(x,g_1,g_2),\varphi(e,x)=x$

易看出G对M*右作用*对应的微分同胚变换:

$$
\begin{aligned}
    \varphi_{g}:& M\rightarrow M\\
    &x\rightarrow \varphi(x, g)\\
\end{aligned}
$$

由上述定义表明，李群是一个具有群结构的解析流形，而且群运算是解析的。故我们可以分析它的维数、紧致性、连通性、幂零性、子群、陪集、商群等。


readings:

* Lie Group Machine Learning
* 机器学习理论及应用
* 李群机器学习

NOTE:

A special issue of Entropy (ISSN 1099-4300) (2020 January)

![fig](https://www.mdpi.com/img/journals/entropy-logo.png?78b1902e596e9c35)

#### 历史

* 2004李群机器学习概念提出
* 2005李群机器学习四个公理假设
* 2006李群机器学习两个模型
* 2007李群机器学习相关算法

#### 李群定义

设G是一个非空集合，满足：

* G是一个群
* G也是一个微分流形
* 群的运算是可微的，即：由$G\times G$到G的映射$(g_1,g_2)|\rightarrow g_{1}g_{2}^{-1}$是可微的映射

则称G是一个李群

#### 为什么

Q:为什么要用李群这一数学工具来研究机器学习？

A:李群是代数结构和几何结构的自然结合体并含有深刻的内在理论

* 李群->群
* 李群->微分流形

#### 李群机器学习公理假设

* 一致性假设: 假设世界W与样本集G由相同的性质
* 划分独立性假设: 将样本集G放到n维空间，寻找超平面(等价关系),使得问题决定的不同对象划分在不相变的区域内或将n维非线性问题映射成线性问题，形成可解的线性结构
* 泛化能力假设: 从有限样本集合中建立世界观测模型，泛化能力就是这个模型对世界为真程度的指标
* 对偶空间假设: 要保证学习的内容与学习的目标结构尽可能一致

#### 李群机器学习的分类器构造 (例)

* 对于一个二分类问题，假定样本集G为$(x_1,y_1),(x_2,y_2)\ldots,(x_l,y_l)\in R^{N}\times Y$, 其中$Y=\{-1,+1\}$，我们就是要寻找分类器$f(x)$,使得$y\cdot f(x)\gt0$。那么从李群机器学习的角度，我们可以把构造其分类器的过程分为如下步骤:

1. 将样本集映射到G这个非空集合上
2. 根据G构造相应的李群结构
3. 将所得的李群作用与所建立的李群机器学习模型中
4. 形成相应的分类器
5. 实例测试
6. 应用

> 实例：李群学习晶体分类演示系统

### 动态模糊机器学习 (Dynamic Fuzzy Machine Learning, DFML)

> DFML=Dynamic Fuzzy Data + Dynamic Fuzzy Mathematics + Dynamic Fuzzy Learning Algorithm + Cognition Principle

readings:

* 动态模糊数据分析理论与方法 (2013)

#### 研究方法

动态模糊数据分析5原理:

* 动态模糊数据可分解原理
* 动态模糊数据可扩展原理
* 动态模糊数据可表示原理
* 动态模糊数据可度量原理
* 动态模糊数据可理解原理

#### 动态模糊逻辑 (Dynamic Fuzzy Logic)

History:

* 1994提出动态模糊逻辑
* 1997出版*动态模糊集及其应用*和*动态模糊逻辑及其应用*
* 2005出版*动态模糊逻辑引论*
* 2008出版*Dynamic Fuzzy Logic and Application*
* 2017出版*Dynamic Fuzzy Machine Learning*

following papers:

* Dynamic fuzzy logic and reinforcement learning for adaptive energy efficient routing in mobile ad-hoc networks. Saloua Chettibi,Salim Chikhi <https://doi.org/10.1016/j.asoc.2015.09.003>
* ...

NOTE:

Special issue on Application of Fuzzy Systems in Data Science and Big Data (2020)

![fig](https://cis.ieee.org/images/files/template/cis-logo.png)

Dyncmic Fuzzy Machine Learning (including Fuzzy Deep Learning) used on big data

> 实例: 无限掌上彩超系统，利用李群机器学习和动态模糊机器学习算法，实现高质量彩色超声图像转化

## Deep Learning

### Shortcoming

* 理论创新性问题，基础理论问题
* 可解释性问题
* 泛化能力问题
* ...

## 概率与因果推理

> Judea 

## PAC 学习理论

> Leslie Gabriel Valiant

## Future

AI is:

* Science
* Technology
* Productivity

* AI有智能但无情感
* AI会计算但无算计
* AI会发展但无自发
* AI可感知但无认知

Some other note:

* 第四个AI主义: 类脑协同主义
* 第五科研范式: 智能