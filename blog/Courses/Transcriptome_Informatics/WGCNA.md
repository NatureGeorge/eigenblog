---
layout: default
date: 2020-04-28
author: Zefeng Zhu
---

# Weighted gene co-expression network analysis

## Standard differential expression analyses seek to identify individual genes

* Each gene is treated as an individual entity
* Often misses the forest for the trees:
  * Fails to recognize that thousands of genes can be organized into relatively few modules

## Philosophy of WGCNA

* Unstand the "system" instead of reporting a list of individual parts
  * Describe the functioning of the engine instead of enumerating individual nuts and bolts
* Focus on modules/clusters as opposed to individual genes
  * the greatly alleviates multiple testing problem
* Network terminology is intutive to biologists

## Steps

* Construct a network
  * Rationale: make use of interaction patterns between genes
* Identify modules
  * Rationale: module (pathway) based analysis
* Relate modules to external information
  * Array Information: Clinical data, SNPs, proteomics
  * Gene Information: Gene ontology, EASE, IPA
  * Rationale: find biologically interesting modules
* Study Module Preservation across different data
  * Rationale：
    * Same data: to check robustness of module definition
    * Different data: to find interesting modules
* Find the key drivers in interesting modules
  * Tools: intramodular connectivity, causality testing
  * Rationale: experimental validation, therapeutics, biomarkers

### Biologically Meaningful

* reduction of high dimensional data
  * expression: microarray, RNA-seq
  * gene methylation data, fMRI data, etc.
* integration of multiscale data
  * expression data from multiple tissues
  *  SNPs (module QTL analysis)
  *  Complex phenotype

### How to construct

* Network = Adjacency Matrix
* $A=[a_{ij}]$
* A symmetric matrix with entries in [0,1]
* For unweighted netowrk, entries are 1 or 0 depending on whether or not 2 nodes are adjacent (connected)
* For weighted networks, the adjacency matrix reports the connection strength between gene pairs

### Steps for constructing a co-expression network

* Gene expression data (array or RNA-seq)
* Measure co-expression with a correlation coefficient
* The correlation matrix is either
  * dichotomized to arrive at an adjacency matrix -> unweighted network
  * OR transformed continuously with the power adjacnecy function -> weighted network

#### Two types of weighted correlation networks

* Unsigned network, absolute value
  * $a_{ij}=|\text{cor}(x_i,x_j)|^\beta$
* Signed network preserves sign info
  * $a_{ij}=|\cfrac{1+\text{cor}(x_i,x_j)}{2}|^\beta$ (min-max normalization)
  * Default values: $\beta=6$ for unsigned and $\beta=12$ for signed networks
  * Prefer unsigned network when genes with negative correlation should also be group in the same module
  * Prefer signed network when you focus on those genes with positive correlation

![fig](https://d3i71xaburhd42.cloudfront.net/00ae5e12df354c4c70b7d51863c6f9f15e44a46a/8-Figure2-1.png)

Question: 

> should network construction account for the sign of the expression relationship?

Answer:

> Overall, recent applications have convinced that signed networks are preferable

#### Why construct a co-expression network based on the correlation coefficient

1. Intuitive
2. Measuring linear relationship avoids the pitfall of overfitting
3. Because many studies have limited numbers of arrays -> hard to estimate non-linear relationships
4. Work well in practice
5. Computationally fast
6. Leads to reproducible research 

#### Why soft thresholding as opposed to hard thresholding

> Hard thresholding may lead to an information loss

1. Preserves the continuous information of the co-expression information
2. Results tend to be more robust with regard to different thresholds

#### Advantages of soft thresholding with the power function

1. Robustness: Network results are highly robust with respect to the choice of the power $\beta$
2. Calibration of different networks becomes straightforward, which facilitates consensus module analysis
3. Module preservation statistics are particularly sensitive for measuring connectivity preservation in weighted networks
4. Math resaon: Geometric Interpretation of Gene Co-Expression Network Analysis

### Choose Parameters of Adjacency Function

Question:

> How should we choose the power $\beta$ or a hard threshold? Or more generally the parameters of an adjacency function?

IDEA: 

Use properties of the connectivity distribution

Answer:

* Generalized Connectivity
  * row sum of the adjacency matrix
    * For unweighted networks: number of direct neighbors
    * For weighted networks: sum of connection strengths to other nodes
  * $k_{i}=\sum_{j}a_{ij}$
  * $p(k)$ vs $k$ in scale free networks
    * $p(k)$: proportion of nodes that have connectivity k
* Generalized Scale Free Topology
  * Motivation: using weak general assumptions, we have proven that gene co-expression networks satisfy these distributions approximately
  * Barabasi(1999): ScaleFree Topology means $\log(p(k))=c_0+c_1\log(k)$

<table>
    <tr>
        <td>
            <img src="https://www.spandidos-publications.com/article_images/ol/18/4/ol-18-04-3673-g01.jpg" width="800">
        </td>
        <td>
            <img src="https://d3i71xaburhd42.cloudfront.net/00ae5e12df354c4c70b7d51863c6f9f15e44a46a/15-Figure4-1.png" width="1000">
        </td>
    </tr>
    <tr>
        <td>
            Scale Free Topology refers to the frequency distribution of the connectivity k
        </td>
        <td>
            The idea of check Scale Free Topology is to log transform p(k) and k and look at scatter plots.
        </td>
    </tr>
    <tr>
    <td>
        With different beta, the linear model fitting R^2 index can show different goodness of fit and thus help us to select an appropriate beta value. In practive we use the lowest value where the curve starts to "saturate". And as beta increases, the mean connectivity decreases.
    </td>
    <td>
        ...
    </td>
    </tr>
</table>

#### The scale free topology criterion for choosing the parameter values of an adjacency function

> Consider only those parameter values in the adjacency function that result in approximate scale free topology, i.e, high scale free topology fitting index $R^2$

Rationale:

* Empirical finding: Many co-expression networks based on expression data from a single tissue exhibit scale free topology
* Many other network e.g. protein-protein interaction networks have been found to exhibit scale free topology

> Caveat: When the data contains few very large modules, then the criterion may not apply. In this case, use the default choices.

### Measure interconnectedness in a network

* adjacency matrix
* topological overlap matrix

#### Topological overlap matrix and corresponding dissimilarity

* $\text{TOM}_{ij}=\cfrac{\sum_{u}a_{iu}a_{uj}+a_{ij}}{\min(k_{i},k_{j})+1-a_{ij}}$
* $\text{DistTOM}_{ij}=1-\text{TOM}_{ij}$
* Generalization to weighted networks is straightforward since the formula is mathematically meaningful even if the adjacencies are real numbers in [0,1]
* Generalized topological overlap (Yip et al (2007) BMC Bioinformatics)
  * $\text{TOM}(i,j)=\cfrac{|N_1(i)\cup N_1(j)|+a_{ij}}{\min(|N_1(i)|, |N_1(j)|)+1-a_{ij}}$ for unweighted network

Noted that:

* $\sum_{u}a_{iu}a_{uj}\le\min(k_i,k_j)-a_{ij}$
* $|N_1(i)\cup N_1(j)|\le\min(|N_1(i)|, |N_1(j)|)-a_{ij}$
* $N_!(i)$ denotes the set of 1-step neighbors of node i
* $||$ measures the cardinality
* Adding 1 to the denominator prevents it from becoming 0
* Therefore, $0\le a_{ij}\le1$ implies $0\le\text{TOM}_{ij}\le1$

![fig](https://www.researchgate.net/profile/Mohamed_Nadhir_Djekidel/post/What_do_adjacency_matrix_and_Topology_Overlap_Matrix_from_WGCNA_package_tell_about_the_data/attachment/59d6382b79197b8077995681/AS%3A396304128724992%401471497675133/download/Screen+Shot+2016-08-18+at+1.19.32+PM.png)

### Detect Network Modules(Clusters)

#### Module Definition

* We often use average linkage hierarchical clustering coupled with the topological overlap dissimilarity measure
* Based on the resulting cluster tree, we define modules as branches
* Modules are either labeled by integers (1,2,3...) or equivalently by colors

#### Dynamic cut tree
