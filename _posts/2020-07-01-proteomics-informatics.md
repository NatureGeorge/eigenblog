---
title: Summary of Proteomics Informatics (Course ver.2020)
author: Zefeng Zhu
date: 2020-07-01 13:30:00 +0800
categories: [Notes, Courses]
tags: [proteomics-informatics]
---

## Basic Information

> Note: This blog is a personal note of the Proteomics Informatics Course and most of the content is non-original.

## 8 蛋白质结构与功能分析

* 蛋白质结构相关软件
* 蛋白质结构比对
* 蛋白质功能
* 蛋白质结构与功能分析方法

### 蛋白质结构可视化

* `Chimera`
* `VMD`
* `PyMOL`

### 蛋白质结构比对

* 探索进化关系
* 确认重复的基序
* 探索结构和功能之间的关系
* 预测功能
* 评价预测的结构
* 结构分类——多种用途

#### 相似结构搜索算法

* Dynamic Programming
  * 一维结构，还原问题
  * 拓扑指数不能改变
  * e.g. `SSAP`:二级结构序列比对程序
* 3D比对/聚类
  * 二级结构或碎片的确认
  * 在不同结构中寻找相似性序列
  * 运行拓扑指数改变，大的插入
  * e.g. `VAST`
* Distance Matrix
  * 确认基团相邻的接触模式
  * 比较不同结构的距离
  * 快速，对插入不敏感
  * e.g. `DALI`
* 单位向量
  * 将结构映射至球向量
  * 快速，对球外区域不敏感
  * e.g. `MAHMMOTH`

#### RMSD

> Root Mean Square Deviation

$$\text{RMSD}=\sqrt{\cfrac{\sum_{i}d_{i}^{2}}{n}}$$

* $n$: number of atoms
* $d_i$: distance between 2 corresponding atoms $i$ in 2 structures

Unit of RMSD:

* identical structure: RMSD=0Å
* similar structure: RMSD between 1Å and 3Å
* distant structure: RMSD > 3Å

### 蛋白质功能

* 催化和调节
* 转运功能
* 收缩或运动功能
* 防御功能
* 营养和存储功能
* something relate to health

#### 蛋白质功能研究策略

* from 新蛋白
* 信息学手段
  * 序列同源比较
  * 相互作用区域
  * 功能区
  * 从而功能partner的预测
  * 为抗体、siRNA、shRNA等涉及提供预测
* 试验分析
  * 试验工具：抗体、克隆、siRNA、shRNA
  * 生理学背景：内源表达和组织及细胞定位
  * 从而筛选相关蛋白
* 信息学+试验分析->验证相关蛋白
  * 细胞内共定位
  * 去除相互作用区域的影响
  * 新蛋白与相关蛋白在功能上的一致性
* 从找到的新相关蛋白开始，进行新一轮的筛选，建立功能网络

#### 蛋白质功能预测方法

* 基于同源序列的蛋白质功能预测
  * e.g. 蛋白质A具有转录功能，蛋白质B与A在氨基酸序列上同源（直系同源），因而蛋白 质B也具有转录功能
* 基于结构域/motif的蛋白质功能预测
  * 蛋白质不同区段的进化速率不同：蛋白质的一些部分必须保持一定的残基模式以保持蛋白质的功能，通过确定这些保守区域，有可能为蛋白质功能提供线索
  * 蛋白质模体或结构域在氨基酸序列水平比其他区域保守，通过对序列比对可以发现这些在进化上较为保守的区域
  * 蛋白质模体或结构域通常与该蛋白质的功能直接相关
  * 根据模体或结构域信息可以对同源水平较低的蛋白质的进行功能预测
  * e.g. `PROSITE, HMMER`
* 基于空间结构的蛋白质功能预测
  * e.g. 蛋白质A具有某一空间结构 ，而蛋白质B也具有与A类似的空间结构特征 ，因而蛋白质B具有与A相似的功能
  * 蛋白质结构决定蛋白质性质和功能，相似结构具有类似功能
  * 结构比序列更保守，空间结构比较可以发现序列相似性很低但结构相似的远源同源蛋白，根据这些远源同源蛋白的结构和相关信息推测蛋白可能的功能
    * 序列-结构比较
    * 结构-结构比较
* 基于相互作用的蛋白质功能预测
  * e.g. 蛋白质之间相互作用以及通过相互作用而形成的 蛋白复合物是细胞各种基本功能的主要完成者。蛋白质A具有转录功能，蛋白质B可以与蛋白质A相互作用，因而蛋白质B可能与基因转录相关

### 蛋白质结构与功能分析方法

#### Protein structure networks

* Protein Contact Maps
* Protein Contact Networks (PCNs)
  * [Protein Contact Networks: An Emerging Paradigm in Chemistry](https://pubs.acs.org/doi/10.1021/cr3002356)
* Amino acid network (AAN)
  * Node: Amino Acid
  * Edge: Interaction between aa
  * `NACEN`: [Node-Weighted Amino Acid Network Strategy for Characterization and Identification of Protein Functional Residues](https://pubs.acs.org/doi/full/10.1021/acs.jcim.8b00146)
  * `RING`
* Protein Structure Network (PSN)
* Residue Interaction Network (RIN)
  * [Insights on protein thermal stability: a graph representation of molecular interactions](https://doi.org/10.1093/bioinformatics/bty1011)

<table>
    <tr>
        <td>
            <img src="https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/chreay/2013/chreay.2013.113.issue-3/cr3002356/production/images/medium/cr-2012-002356_0007.gif">
        </td>
        <td>
            <img src="https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/chreay/2013/chreay.2013.113.issue-3/cr3002356/production/images/medium/cr-2012-002356_0010.gif">
        </td>
    </tr>
    <tr>
        <td>
            Recoverin 3D structure (left) and correspondent adjacency matrix
        </td>
        <td>
            Graph protein formulas
        </td>
    </tr>
    <tr>
        <td>
            <img src="https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/jcisd8/2018/jcisd8.2018.58.issue-9/acs.jcim.8b00146/20180918/images/medium/ci-2018-001467_0006.gif">
        </td>
        <td>
            <img src="https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/jcisd8/2018/jcisd8.2018.58.issue-9/acs.jcim.8b00146/20180918/images/medium/ci-2018-001467_0002.gif">
        </td>
    </tr>
    <tr>
        <td>
            NACEN
        </td>
        <td>
            Schematic diagram of NACENs
        </td>
    </tr>
    <tr>
        <td>
            <img src="https://www.ncbi.nlm.nih.gov/pmc/articles/instance/6662296/bin/bty1011f1.jpg">
        </td>
        <td>
            <img src="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6662296/bin/bty1011f2.jpg">
        </td>
    </tr>
    <tr>
        <td>
            Insights on protein thermal stability fig.1
        </td>
        <td>
            Insights on protein thermal stability fig.2
        </td>
    </tr>
</table>

application:

* [Identifying functionally important sites](https://doi.org/10.1016/j.jmb.2004.10.055)
* [Identification of protein-protein interface residues](https://doi.org/10.1093/protein/15.4.265)
* [Communication paths](https://doi.org/10.1073/pnas.0704459104)

Tools:

* `NAPS`
* `RING`
* `Cytoscape:RINspector`
* ...

Others:

* [Protein Contacts Atlas](https://doi.org/10.1038/s41594-017-0019-z)
  * an interactive resource of noncovalent contacts from over 100,000 PDB crystal Structures
  * for visualization and analysis of non-covalent contacts at different scales of organization: atoms, residues, secondary structure, subunits, and entire complexes
    * allostery
    * disease mutations
    * polymorphisms

![Protein Contacts Atlas](https://www.mrc-lmb.cam.ac.uk/rajini/img/illustrations/3EML_A_mixed_trans.png)

#### Elastic Network Models (ENMs)

* 结构-序列-动力学-功能
* Protein Dynamics
* ENM: representation of protein structure as a network
  * Gaussian network model (GNM)
  * Anisotropic Network Model (ANM)
* Each node represents a residue
* Elastic springs connect residues located within a cutoff
* e.g
  * `ProDy`: GNM vs ANM
  * `iGNM`: Gaussian Network Model Database
  * `DynOmics`
* from biophysics to bioinformatics
* from structures to interactions

<table>
    <tr>
        <td>
            <img src="https://www.ncbi.nlm.nih.gov/pmc/articles/instance/3102222/bin/btr168f1.jpg">
        </td>
        <td>
            <img src="https://www.ncbi.nlm.nih.gov/pmc/articles/instance/4702874/bin/gkv1236fig1.jpg">
        </td>
    </tr>
    <tr>
        <td>
            ProDy
        </td>
        <td>
            iGNM 2.0
        </td>
    </tr>
</table>


#### Sequence similarity networks

* 在每一个序列相似性网络中，每个蛋白由节点表示，如果它们的序列相似性超过用户定义阈值，就以连接到其它蛋白来显示。以此将不同家族蛋白的关系以简单、快速的方法可视化
* e.g. `EFI`: Enzyme Similarity Tool

## 9 蛋白质芯片

> protein chips, protein array

> 蛋白质芯片, 又称蛋白质阵列或蛋白质微阵列，是指以蛋白质分子作为配基,将其有序地固定在固相载体的表面形成微阵列；用标记了荧光的蛋白质或其他它 分子与之作用，洗去未结合的成分，经荧光扫描等检测方式测定芯片上各点的荧光强度，来分析蛋白之间或蛋白与其它分子之间的相互作用关系


## Reference

1. Di Paola L, De Ruvo M, Paci P, Santoni D, Giuliani A. Protein contact networks: an emerging paradigm in chemistry. Chem Rev. 2013;113(3):1598-1613. doi:10.1021/cr3002356
2. Yan W, Hu G, Liang Z, et al. Node-Weighted Amino Acid Network Strategy for Characterization and Identification of Protein Functional Residues. J Chem Inf Model. 2018;58(9):2024-2032. doi:10.1021/acs.jcim.8b00146
3. Amitai G, Shemesh A, Sitbon E, et al. Network analysis of protein structures identifies functional residues. J Mol Biol. 2004;344(4):1135-1146. doi:10.1016/j.jmb.2004.10.055
4. Brinda KV, Kannan N, Vishveshwara S. Analysis of homodimeric protein interfaces by graph-spectral methods. Protein Eng. 2002;15(4):265-277. doi:10.1093/protein/15.4.265
5. Ghosh A, Vishveshwara S. A study of communication pathways in methionyl- tRNA synthetase by molecular dynamics simulations and structure network analysis. Proc Natl Acad Sci U S A. 2007;104(40):15711-15716. doi:10.1073/pnas.0704459104
6. Miotto M, Olimpieri PP, Di Rienzo L, et al. Insights on protein thermal stability: a graph representation of molecular interactions. Bioinformatics. 2019;35(15):2569-2577. doi:10.1093/bioinformatics/bty1011
7. Kayikci M, Venkatakrishnan AJ, Scott-Brown J, Ravarani CNJ, Flock T, Babu MM. Visualization and analysis of non-covalent contacts using the Protein Contacts Atlas. Nat Struct Mol Biol. 2018;25(2):185-194. doi:10.1038/s41594-017-0019-z
8. Bakan A, Meireles LM, Bahar I. ProDy: protein dynamics inferred from theory and experiments. Bioinformatics. 2011;27(11):1575-1577. doi:10.1093/bioinformatics/btr168