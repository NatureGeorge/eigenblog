---
title: Introduction of pdb-profiling
author: Zefeng Zhu
date: 2020-10-14 10:30:40 +0800
categories: [Notes, Research]
tags: [pdb, python]
---

## 预备知识

* `RefSeq`数据资源与`Ensembl`数据资源
* 什么是`PDB`
* 什么是`UniProt-KB`
* 什么是可变剪切(Alternative Splicing)
* 什么是蛋白质一级结构 (序列)
* 什么是蛋白质三级结构 (晶体结构)

## 从中心法则谈起: 基因位点到蛋白质位点

<table>
    <tr>
        <td>
            <img src="../../assets/img/genome2transcript.drawio.svg">
        </td>
    </tr>
    <tr>
        <td>
            1. from genomic location to transcript site
        </td>
    </tr>
    <tr>
        <td>
            2. from transcript site to protein site
        </td>
    </tr>
    <tr>
        <td>
            3. from protein site to crystal site
        </td>
    </tr>
</table>

在自然生物体内，一生物功能的实现，往往需要具有生物学效应的蛋白质参与实施。而这蛋白从何而来? 经过多肽链空间折叠而来。多肽链从何而来? 由信使RAN(message RNA, mRNA)经过核糖体翻译而来。那么mRNA又从何而来，经过一基因的转录，转录后修饰，进行可变剪切等而来。这样来看，从三核苷酸密码子到多肽链的氨基酸再到蛋白质结构中的氨基酸残基(Residue)，在逻辑上也就存在对应关系。这个对应关系建立在核酸序列的顺序性以及衍生的多肽链序列的顺序性的基础上。

从基因组位置映射到转录本位点以及从转录本位点映射至蛋白质位点的工作，面临着两大问题:

1. 基因组数据资源的庞大，映射位点工作尽管逻辑与计算上简单但是需要大量计算资源
2. 基因组数据标识、转录本数据标识、蛋白质数据标识都会随着国际科学研究的进展而进行更新，在ID标识符与具体的位点上常常有比较大的更新

幸运的是，上述层面的工作被诸如NCBI(美国国家生物技术信息中心)以及EBI(欧洲生物信息学中心)等机构承包。他们本身是数据资源的存储、更新方，同时具有充足的计算资源与经费，能够便捷且良好地完成这些工作。在获取人类基因组突变相关数据时，基本上在得到所关注的基因组层面的突变位置时，数据提供方也会给出映射至转录本或蛋白质可变剪切体层面的位点(标识符常采用Ensembl Gene/Transcript/Protein或RefSeq Nucleotide/Transcript/Protein)。

> 注: 若原始突变数据没有给出具体的蛋白质位点，也有大量工具可以进行转化，本处不再赘述

而在研究蛋白质时，基本上离不开`UniProt-KB`这一数据资源，其对大量物种的每个基因得到的蛋白质都赋予特定标识符(i.e. UniProt Accession/Entry: P21359)，同时有专门人员对大量条目进行专家鉴定(被鉴定过的UniProt条目会被标记上`reviewed`，反之`unviewed`)；没被review过的条目一般是计算方法更具同源性等理论预测出的条目，一般不采用。值得注意的是，`UniProt-KB`考虑到了转录本层面的可变剪切现象 ([click here to learn more](https://www.uniprot.org/help/canonical_and_isoforms))，因而在具有可变剪切现象的UniProt条目下，其也给出了不同的可变剪切条目，称为isoform (e.g `P21359-1, P21359-2, P21359-3, P21359-4, P21359-5`)。这些可变剪切体在序列上有所差异，因而对应的完整蛋白质结构可能有所不同。(若某部分可变剪切体的序列与其他部分可变剪切体的序列差异达到一定程度，`UniProt-KB`将创建新的条目来给那一部分的isoform)。在这些isoform中，`UniProt-KB`会选出一isofrom作为canonical sequence来代表该`UniProt`条目(一般是选最长的，但也不一定)。选定的canonical isoform在`UniProt-KB`中一般不会显示其具体的isoform编号(例如`P21359`的canonical isoform为`P21359-1`，那在`UniProt-KB`中说`P21359`一般就是指`P21359-1`)。

> 注: UniProt-KB会用诸如VAR_032459等来表示各个可变剪切体(isoform)的sequence与canonical sequence的差异

作为`UniProt-KB`以及`Ensembl`的持有方，EBI将ensembl与UniProt条目进行了匹配，且对于reviewed UniProt条目精确到了具体的对应isoform；同时与NCBI合作，将`RefSeq`资源也进行了如上对应。样例:

Ensembl:

```txt
ENST00000356175; ENSP00000348498; ENSG00000196712 [P21359-2]
ENST00000358273; ENSP00000351015; ENSG00000196712 [P21359-1]
ENST00000431387; ENSP00000412921; ENSG00000196712 [P21359-5]
ENST00000487476; ENSP00000491589; ENSG00000196712 [P21359-3]
```

RefSeq:

```txt
NP_000258.1, NM_000267.3 [P21359-2]
NP_001035957.1, NM_001042492.2 [P21359-1]
NP_001121619.1, NM_001128147.2 [P21359-5]
```

> 注: 不是所有的UniProt条目对应的原始转录本都有发生可变剪切现象

目前为止是从基因组位点到蛋白质位点(UniProt Isoform层面)的工作。

### Code to Achieve Your Goal

```python
from pdb_profiling import default_config
from pdb_profiling.processors import Identifier

default_config(your_output_folder)

demo = Identifier('ENST00000431387')
entry, isoform = demo.map2unp().result()
print(f"UniProt Entry: {entry}, UniProt Isoform: {isoform}")
# UniProt Entry: P21359, UniProt Isoform: P21359-5
```

## 从UniProt到PDB结构: 为什么有些事情不是那么顺理成章

从基因序列到转录本序列再到对应的可变剪切体氨基酸序列，都是站在所研究的基因的视角上，要求着各个层面上序列对应关系的一致性(尤其是转录本发生可变剪切后翻译出的蛋白质序列要与对应的UniProt isoform完全一致，i.e ENSP00000351015的序列和P21359-1的序列完全一致才匹配上)，这样才能实现位点的精确对应，不会出现错配的情况。而UniProt与PDB之间可以通过氨基酸序列匹配、物种匹配等便捷建立匹配关系。但对于PDB晶体结构，有如下情况需要澄清:

1. 通常，现有的序列匹配结果资源是将UniProt Isofrom的序列与PDB链的完整序列(不论有无确切解析出的空间坐标、无论是否是人工添加的序列片段等)进行匹配；为什么要说这个? 请看下面的内容
2. 解析出该结构的晶体学科学家不一定就是按照其研究的基因或蛋白的原始序列进行结晶，其可能是编辑过一级序列进行C端添加人工序列、对某段序列进行了重复、减去了某段序列、插入某段序列等增删改操作；
3. 晶体学家解析结构的方法有X-ray(X射线)、NMR(核磁共振)、EM(电镜)等方法，不同方法得到的同一目标蛋白结构在精度上有所不同，且存在部分预期要结晶的氨基酸序列却没结晶出来(所以结构中间可能会中断一部分, missing residues)或是只结晶出了氨基酸残基的主链原子没得到侧链原子信息
   * 某一氨基酸残基侧链原子完全丢失或丢失到无法确定氨基酸种类且未知确切的一级结构就无法确定该氨基酸为何种氨基酸，氨基酸残基用`UNK`表示，单字符用`X`表示
   * 若某一氨基酸残基的侧链原子部分缺失或完全丢失，但知道一级结构就仍能确定该氨基酸种类
4. 同时，结晶出的结构的某些残基可能会发生糖基化、酰基化、磷酸化等修饰，使得氨基酸残基侧链改变，相当于氨基酸种类变化，这就与原一级结构对应的氨基酸有了差异
5. 一级结构决定三级结构，一级结构的些许差异对于三级结构的空间构象可能有着不小的影响
   * 有的晶体结构在涵盖了匹配上的蛋白序列(UniProt Isoform sequence)之外，还有其他序列片段被结晶了出来，这些"额外"的片段在空间结构上可能对匹配上的片段部分产生构象等层面的影响
6. 一条匹配上的PDB链不一定就完全覆盖了目标蛋白的全部序列，可能只是其中的某一小部分；匹配上的不同PDB链对该UniProt Isoform的覆盖范围可能存在差异

上述这些要求着我们对UniProt Isoform与PDB链的序列匹配的identity进行把控，同时考虑到missing residues等的影响，选择出与目标UniProt Isoform最匹配的PDB链；同时，考虑到不同PDB链的不同覆盖范围，尽量选择覆盖范围最大的链，并可选择多条链使得这些链尽可能覆盖完整蛋白质，以此来尽多地囊括我们所要研究的位点。

## 聊聊三维结构数据: 数据文件的组织形式

### Logical Structure

* Entry
  * Assembly
    * Model
      * Entity
        * Chain
          * Residue
            * Residue Conformer
              * Atom

### Asymmetric Unit & Biological Assembly/Unit



## 路在何方: 站在蛋白质位点的视角，有何应用

...