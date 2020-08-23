---
title: Cluster Interface / Interface Similarity
author: Zefeng Zhu
date: 2020-07-30 08:30:00 +0800
categories: [Notes, Research]
tags: [protein, interaction, interface, ppi]
---
# Cluster Interface / Interface Similarity

## Data Resource

* (2007) [PISA](https://www.ebi.ac.uk/pdbe/pisa/)
* (2011, 2020) [ProtCID](http://dunbrack2.fccc.edu/ProtCiD/Default.aspx)
* (2012) [InterEvol](http://biodev.cea.fr/interevol/interevol.aspx)
* (2014) [PIFACE](http://prism.ccbb.ku.edu.tr/piface/)
* (2014) [EPPIC](http://www.eppic-web.org/ewui/#)
* (2017) ❌[QSbio](http://www.qsbio.org/)

## Method

* (2004) [MultiProt](https://onlinelibrary.wiley.com/doi/abs/10.1002/prot.10628)
* (2005) [TM-Align](https://doi.org/10.1093/nar/gki524)
* (2009) [MM-align](https://doi.org/10.1093/nar/gkp318)
* (2010) [iAlign](http://doi.org/10.1093/bioinformatics/btq404)
* (2015) [PCalign](https://doi.org/10.1186/s12859-015-0471-x)
* (2015) [PROSTA-inter](https://doi.org/10.1093/bioinformatics/btv242)
* (2018) [InterComp](https://doi.org/10.1093/bioinformatics/bty587)
* (2018) [PatchBag](https://doi.org/10.1038/s41598-018-26497-z)

## Some Other Insights

* (2018) [Integrating co‐evolutionary signals and other properties of residue pairs to distinguish biological interfaces from crystal contacts](https://onlinelibrary.wiley.com/doi/full/10.1002/pro.3448)
* (2018) [Distinguishing crystallographic from biological interfaces in protein complexes: role of intermolecular contacts and energetics for classification](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2414-9)
* (2019) [Accurate Classification of Biological and non-Biological Interfaces in Protein Crystal Structures using Subtle Covariation Signals](http://dx.doi.org/10.1038/s41598-019-48913-8)

### PatchBag

#### Introduction

> Previous Works

Several algorithms were developed to identify surface similarities, independent of the overall protein folds. These algorithms represent the surface shapes in various ways, such as

* Alpha Shapes and Delaunay Triangulations
    * [Three-dimensional alpha shapes](https://doi.org/10.1145/174462.156635)
    * 16
    * 17
* Three-Dimensional Zernike Descriptors (3DZD)
    * 18
    * 19
    * 20
    * 21
* or an unordered collection of the three-dimensional (3D) coordinates of the surface atoms
    * 22
    * 23
* Some of these algorithms were implemented for a fast 3D comparison of protein surfaces
    * 24
    * 25

In addition, algorithms were developed dedicatedly to compare interfaces - the functional part of the surface. The available interface-comparison and interface clustering algorithms are usually limited to specific interface types, such as

* protein-binding interfaces
    * [Characterizing the morphology of protein binding patches](https://doi.org/10.1002/prot.24144)
    * [Fast screening of protein surfaces using geometric invariant fingerprints](https://doi.org/10.1073/pnas.0906146106)
    * `PROSTA-inter`: [Finding optimal interaction interface alignments between biological complexes](https://doi.org/10.1093/bioinformatics/btv242)
    * `iAlign`: [a method for the structural comparison of protein-protein interfaces](http://doi.org/10.1093/bioinformatics/btq404)
    * [Global and local structural similarity in protein–protein complexes: Implications for template-based docking](https://doi.org/10.1002/prot.24392)
    * [Protein Docking by the Interface Structure Similarity: How Much Structure Is Needed?](https://doi.org/10.1371/journal.pone.0031349)
    * `SiteEngines`: [recognition and comparison of binding sites and protein-protein interfaces](https://doi.org/10.1093/nar/gki482)
    * ❌`PBSword`: [a web server for searching similar protein–protein binding sites](https://doi.org/10.1093/nar/gks527)
* ligand-binding interfaces
    * 13
    * 34
    * 35
    * 36
    * 37
    * 38
    * 39
    * 40
* or nucleic-acid-binding interfaces
    * [Structural Alignment of Protein–DNA Interfaces: Insights into the Determinants of Binding Specificity](https://doi.org/10.1016%2Fj.jmb.2004.11.010)

Other methods based on local structural surface comparison have been employed for
* pocket comparison
    * 42 
* and interface prediction, for instance, 
    * predicting protein-protein interacting residues
        * 43
    * or finding ligand binding sites
        * 21
        * 44

> What PatchBag Has Fulfilled

Here, we present a novel geometry-based approach, named `PatchBag`, for characterizing and comparing protein surfaces or sub-surfaces (interface) in an accurate and highly efficient manner. 

* In contrast to other interface-comparison tools, `Patchbag` is applicable for comparing interfaces of any type, as it does not require any information from the specific partner of the protein of interest.
* PatchBag represents protein surfaces or interfaces as vectors that count the number of appearances of various geometrical types of small surface units, defined as surface patches.
    * This is known as the bag-of-words approach (BOW), where the surface is described as an unordered collection of local features.
    * The advantage of using BOWs is that it is magnitudes faster to compare vectors than to compare the original objects. 
    * Indeed, the BOW approach is extensively used in large search systems such as web search engines.
    * BOW algorithms were also used in the context of protein study, as for example in `SVM-Fold` to detect protein sequence homology, in `FragBag` to rapidly retrieve protein structures from the Protein Data Bank (PDB), and to discriminate native structures from structure prediction decoys.
* In PatchBag, we define local surface patches by the coordinates of the C-alpha atom of an exposed residue and their nearest C-alpha atoms that are not consecutive along the protein chain.


#### Calculating protein surfaces or interface similarities using PatchBag

> Given a library `L < patch_size, library_size >` , we characterize a protein surface or subsurface `P` by its PatchBag vector of `library_size` entries; the vector represents the number of times each library-patch best approximates a surface patch in `P`. We calculate the similarity between two protein surfaces by the cosine distance of their corresponding PatchBag vectors – see equation (2). We refer to 1 - the cosine angle between two PatchBag vectors as "PatchBag distance".

$$
\begin{aligned}
\text{PatchBag\_Distance}(P_{1},P_{2})=1-\frac{{P}_{1}{P}_{2}^{T}}{\sqrt{{P}_{1}{P}_{1}^{T}}\sqrt{{P}_{2}{P}_{2}^{T}}},\, (2)
\end{aligned}
$$

### InterComp

> `iAlign` was developed (Gao and Skolnick, 2010a). It is a protein–protein interface comparison method based on an extension of the Kabsch algorithm (Kabsch, 1976), also used in `TM-align`, that will optionally allow for sequence-order independent comparisons for interfaces. However, it utilizes a definition of protein interface, where the interfacial residues are collected across both the protein chains involved in the interaction. This means that the mutual position of the patches of interfacial residues must be known before any comparison can be performed against a template. Thus, `iAlign` can only be used if the complete interface is known, e.g. for comparing known interfaces, and not for searching for interfacial residues on a monomer structure.

## Reference

1. Xu, Q., Dunbrack, R.L. ProtCID: a data resource for structural information on protein interactions. Nat Commun 11, 711 (2020). https://doi.org/10.1038/s41467-020-14301-4
2. Gao M, Skolnick J. iAlign: a method for the structural comparison of protein-protein interfaces. Bioinformatics (Oxford, England). 2010 Sep;26(18):2259-2265. DOI: 10.1093/bioinformatics/btq404.
3. Cukuroglu E, Gursoy A, Nussinov R, Keskin O. Non-redundant unique interface structures as templates for modeling protein interactions. PLoS One. 2014;9(1):e86738. Published 2014 Jan 27. doi:10.1371/journal.pone.0086738
4. Baskaran, K., Duarte, J.M., Biyani, N. et al. A PDB-wide, evolution-based assessment of protein-protein interfaces. BMC Struct Biol 14, 22 (2014). https://doi.org/10.1186/s12900-014-0022-0
5. Krissinel E, Henrick K. Inference of macromolecular assemblies from crystalline state. J Mol Biol. 2007;372(3):774-797. doi:10.1016/j.jmb.2007.05.022
6. Yang Zhang, Jeffrey Skolnick, TM-align: a protein structure alignment algorithm based on the TM-score, Nucleic Acids Research, Volume 33, Issue 7, 1 April 2005, Pages 2302–2309, https://doi.org/10.1093/nar/gki524
7. Xuefeng Cui, Hammad Naveed, Xin Gao, Finding optimal interaction interface alignments between biological complexes, Bioinformatics, Volume 31, Issue 12, 15 June 2015, Pages i133–i141, https://doi.org/10.1093/bioinformatics/btv242
8. Claudio Mirabello, Björn Wallner, Topology independent structural matching discovers novel templates for protein interfaces, Bioinformatics, Volume 34, Issue 17, 01 September 2018, Pages i787–i794, https://doi.org/10.1093/bioinformatics/bty587
9. Budowski-Tal I, Kolodny R, Mandel-Gutfreund Y. A Novel Geometry-Based Approach to Infer Protein Interface Similarity. Sci Rep. 2018;8(1):8192. Published 2018 May 29. doi:10.1038/s41598-018-26497-z
10. Faure G, Andreani J, Guerois R. InterEvol database: exploring the structure and evolution of protein complex interfaces. Nucleic Acids Res. 2012;40(Database issue):D847-D856. doi:10.1093/nar/gkr845
11. Elez K, Bonvin AMJJ, Vangone A. Distinguishing crystallographic from biological interfaces in protein complexes: role of intermolecular contacts and energetics for classification. BMC Bioinformatics. 2018;19(Suppl 15):438. Published 2018 Nov 30. doi:10.1186/s12859-018-2414-9
12. Fukasawa Y, Tomii K. Accurate Classification of Biological and non-Biological Interfaces in Protein Crystal Structures using Subtle Covariation Signals. Sci Rep. 2019;9(1):12603. Published 2019 Aug 30. doi:10.1038/s41598-019-48913-8
13. Dey S, Ritchie DW, Levy ED. PDB-wide identification of biological assemblies from conserved quaternary structure geometry. Nat Methods. 2018;15(1):67-72. doi:10.1038/nmeth.4510
14. Hu J, Liu HF, Sun J, Wang J, Liu R. Integrating co-evolutionary signals and other properties of residue pairs to distinguish biological interfaces from crystal contacts. Protein Sci. 2018;27(9):1723-1735. doi:10.1002/pro.3448
15. Shatsky M, Nussinov R, Wolfson HJ (2004) A method for simultaneous alignment of multiple protein structures. Proteins 56: 143–156.
16. Cheng, S., Zhang, Y. & Brooks, C.L. PCalign: a method to quantify physicochemical similarity of protein-protein interfaces. BMC Bioinformatics 16, 33 (2015). https://doi.org/10.1186/s12859-015-0471-x
17. Mukherjee S, Zhang Y. MM-align: a quick algorithm for aligning multiple-chain protein complex structures using iterative dynamic programming. Nucleic Acids Res. 2009;37(11):e83. doi:10.1093/nar/gkp318


