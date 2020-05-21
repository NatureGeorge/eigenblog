---
title: Note of Transcriptome Informatics (2020-02-25)
author: Zefeng Zhu
date: 2020-02-25 13:30:00 +0800
categories: [Notes, Courses]
tags: [transcriptome-informatics]
---

## Data Preprocessing of Hybrid Genechip

* 数据获取单元: 点
* 红色前景荧光强度: $\text{Rf}$
* 红色背景荧光强度: $\text{Rb}$
* 绿色前景荧光强度: $\text{Gf}$
* 绿色背景荧光强度: $\text{Gb}$
* 背景校正
  * $\text{R=\text{Rf}-\text{Rb}}$
  * $\text{G=\text{Gf}-\text{Gb}}$
* 对数差异比: $M=\log(R/G)$
* 对数强度: $A=1/2(\log(RG))$

## Experimental Design

### 间接比较法

```viz
graph graph1 {
    edge [color="0.700 0.200 1.000"];
    node [style=filled, color="0.650 0.200 1.000"];
    overlap=false;
    R1,R2,R3,R4[label="R"];
    R1--R2--R3--R4;
    A1--R1;
    B1--R2;
    A2--R3;
    B2--R4;
}
```

* 通过一个共同参照物(R)完成
* 基因表达值: ratio, 具有可比性
* ...

### 直接比较法

略

## Task

### Requirement

1. 从`NCBI`的`GEO`或者其它数据库中查找自己感兴趣的`cdna`芯片数据，至少要求给出如下信息：
   * 该套数据所发表的文章的名字：
   * 该套数据的下载网址：
   * 该套数据基本情况介绍（简介以及该套数据包含多少张芯片并且芯片分为多少种类型，以及每种类型有多少张芯片）
2. 对芯片数据进行预处理，至少包括如下操作（如果有必要）：
    * 背景矫正，芯片上重复探针或者基因的合并，弱信号处理，归一化（`lowess`算法或其它算法），去量纲化，补缺失值
    * 可以自己编写程序，也可以用现成的程序，比如`arraytools`
3. 说明文档要求
    * 写清楚数据处理思路和所用的具体方法
    * 要写清楚试验步骤和其它自己要补充的方面，步骤要求按照所写的步骤的指导，可以从新进行数据实验

### Answer

1. Thompson M, Lapointe J, Choi YL, Ong DE et al. Identification of candidate prostate cancer genes through comparative expression-profiling of seminal vesicle. Prostate 2008 Aug 1;68(11):1248-56. PMID: 18500686
2. https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE11376
3. Overall design: cDNA microarrays from the Stanford Functional Genomics Facility were used for expression profiling of __11 normal prostate and 7 seminal vesicle specimens__ (6 of which were matched pairs), against a universal RNA reference. Extracted expression ratios were normalized by array then mean centered by gene, and expression differences between normal prostate and seminal vesicle identified using Significance Analysis of Microarrays (SAM).
4. ...

### 数据处理

* 背景校正
* 弱信号处理
  * 噪声引起
  * 重要信息点
  * 靠重复试验解决
  * 靠设定强度阈值解决
    * 简单信号强度阈值100
    * 信噪比
    * 通过背景、空白点、阴性对照点确定弱信号的阈值
    * 信号轻度的累积分布函数确定阈值
* 对数转换
  * 生物学上易于理解
  * 使数据的分布满足对称性和近似正态分布，满足常用的统计分析方法
  * 使用的方便性
* 重复数据的合并
  * 计算重复值的集中趋势指标
  * 但上面这方法会造成信息损失，减少损失方法如下
    * 去除奇异值：计算原始数据的均值和标准差，去掉给定区间外的数据，可以多次重复此过程
* 异常值和缺失值的处理
  * 异常值的产生
  * 异常值的处理
  * 缺失值的产生: 异常值去除
  * 缺失值的处理
    * 删掉缺失值的整条记录，但是损失大量有价值的信息
    * 数据填充
      * 使用重复数据点
      * 利用基因相关性
        * 预测模型的建立
          * 行均数，中位数、0等简单填充
          * 使用回归模型对每个变量的缺失数据进行迭代性预测
          * 奇异值分解
          * K-近邻
* 数据归一化
  * 原因:系统误差
  * 方法
    * 序列内归一化
      * 全局归一化(整体平移)
      * 强度依存偏倚的归一化(i.e 应用LOWESS法)
      * 点样头归一化
    * 多张芯片见归一化
    * 染色互换配对设计的芯片的归一化


#### 应用K-近邻法来补缺失值

* 采用欧氏距离: $d_{ij}=\sqrt{\sum_{k=1}^{p}(x_{ik}-x_{jk})^{2}}$
  * 计算两个基因的距离
  * 有缺失值的指标跳过
* 找出与所求基因最相似的的k个基因
* 计算这K个基因在该指标上的均数或加权均数
* K一般取10-20 (from Troyanskaya)

### 差异基因的筛选

* 倍数法
* Z值法
* T值排序法
  * 修正的T值
* 多组数据
  * 两类就用T检验
  * 多类用F检验减去方差


