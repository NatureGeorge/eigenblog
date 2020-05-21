---
title: Proteomics Informatics (Test 1)
author: Zefeng Zhu
date: 2020-03-27 13:30:00 +0800
categories: [Notes, Courses]
tags: [proteomics-informatics]
---

## Requirement 

* 从蛋白质组学实验中鉴定了与疾病相关的蛋白，如何利用现有的数据库和生物信息学工具对其进行功能注释
* 至少列出2个数据库和1个工具，并简单列举注释的步骤

## Assumptions

* 假定已知蛋白质学实验得到的蛋白序列已知
* 且现有数据库中存在对该蛋白的注释

## Annotation

### Define Protein

当鉴定蛋白未知其确切蛋白名称种类等信息时，利用序列比对来搜索数据库，找到与其完全(或近乎)匹配的条目。

* 序列搜索工具: BLAST
* 序列搜索数据库: UniProt

![view](../../assets/img/BLAST.png)

![view](../../assets/img/BLAST_Example.png)

### Annotate via UniProt

鉴定蛋白当匹配到最佳的UniProt条目后，可在UniProt数据库中找到序列等层面的信息/注释，其中包括:

* 蛋白功能
* 名称与分类
* 亚细胞定位
* 病理信息/突变信息
* 后转录残基修饰位点
* 蛋白相互作用
* GO注释
* 晶体结构索引
* 二级结构预测
* 结构域，家族，Motif
* 相似蛋白
* ...

<table><tr>
<td><img src="../../assets/img/UniProt.png" width=900em></td>
</tr></table>

程序性提取可通过:

1. 解析XML或txt格式的条目页面
2. 调用Retrieve/ID Mapping 的API获取相关列

![view](../../assets/img/UniProt_Column.png)

### Annotate via PDB (RCSB/PDBe)

若鉴定蛋白有已解析出的晶体结构(可知UniProt中查看到,或是直接进入数据库搜索到)，可以在蛋白结构层面进行注释，于RCSB中可得到下列注释:

* 结构解析时间等基础信息
* 结构质量 (分辨率+质量评估信息)
* 结合配体 (配体分子，配体结合位点)
* 蛋白一至四级结构信息
* 结构域注释
* 二级结构注释
* ...

![view](../../assets/img/RCSB_Protein_Feature_View.png)

<table><tr>
<td><img src="../../assets/img/RCSB_CUSTOM_TABULAR_REPORT_1.png" width=900em></td>
<td><img src="../../assets/img/RCSB_CUSTOM_TABULAR_REPORT_2.png"></td>
</tr></table>

可在RCSB的custom tabular report页面自定义且批量提取指定PDB条目的相关功能层面的注释

#### Annotation Example

![view](../../assets/img/RCSB_Annotation_Example1.png)

### Annotate by others

除了通过注释好的数据库来获取功能注释，也可通过其他工具来自己产出注释，经典工具包括ExPASy的:

* ProtParam
* TMPred
* ...

