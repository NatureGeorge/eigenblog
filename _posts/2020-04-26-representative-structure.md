---
title: Representative/Non-redundant Data Set
author: Zefeng Zhu
date: 2020-04-26 08:30:10 +0800
categories: [Notes, Research]
tags: [algorithm, pdb, protein-structure]
---

## Selection of representative protein data set

> Hobohm U., Scharf M., Schneider R., Sander C. Selection of representative protein data sets. Protein Sci. 1992;1:409–417. doi: 10.1002/pro.5560010313. 

### Redundancy in PDB

1. different crystal form
2. a variary of substrate analogues
3. different engineered point mutations

### Necessity for reducing the redundancy

1. blind use in statistical analysis would lead to serious overcounting
2. masking otherwise observable regularities

### Desired properties of non-redundant data

1. Maximum coverage
2. Minimum redundancy
3. No pair of proteins in selected set should have more than a given level of sequence identity
4. Experimental quality of the protein structures should be optimal or meet given criteria
5. The number of proteins in the set should be maximal within the given constraints

### Problems to be solved

* Combinatorial complexity
* Database development
  * any procedure _not sufficiently robust to be routinely applied to new updates of the database_ would soon leave us with an antiquated selection
* Single PDB Entry can contain multiple chains that have to be treated separately, and accessory information such as crystallographic resolution is not unambiguously coded for in the data sets

### Two solutions

* Central to each is the concept of distance (or similarity) in sequence space.
* The decision can be made
  * on the basis of dynamic sequence alignment algorithms followed by application of a length-dependent threshold of similarity
  * Or, for protein structures, on the basis of optimal 3D alignment, followed by application of an appropriate cutoff

> The problem and algorithm could be neatly dsecribed in the language of graph theory(a protein is a vertex; two vertices are connected by an edge if the two proteins are similar; the matrix of all pair relationships is the adjacency matrix; and so on [e.g Sedgewick, 1983])

#### Outline of algorithm 1

* Given a sorted list of candidate proteins, process each protein in turn by selecting or discarding it according to the following criteria:
  * discard proteins that are similar to already selected proteins
  * discard proteins that fail to meet additional user-specified standards

#### Details of algorithm 1

> Select until done

The algorithm proceeds by simultaneous processing of three lists of protein identifiers:

* the test list of all candidate proteins(or protein chains)
  * can be sorted according to user-defined criteria so that certain types of proteins have a higher probability of being selected
* the skip list
  * contains proteins that are similar to a previously processed protein from the test list and may also contain a priori unwanted proteins
* the select list
  * initially empty
  * contains proteins chosen as part of the nonredundant data set

the three lists are processed as follows:

* Stpe 1: read one protein identifier from ***the test list*** and check if this protein is a member of ***the skip list***
  * if so, process the next protein in ***the test list***, i.e., repeat `Step 1`
  * otherwise go to `Step 2`
* Step 2: check if the protein satisfies user-specified requirement such as minimum sequence length, maximun number of unknown residues
  * if the requirements are satisfied, append the protein to ***the select list***
  * otherwise, process the next protein in ***the test list***, i.e., repeat `Step 1`
* Step 3: with the selected protein, start a FASTA search against all remaining sequences in ***the test list***
* Step 4: scan the FASTA output file and append to ***the skip list*** proteins with a higher similarity than the specified threshold

#### Outline of algorithm 2

* Given a list of candidate proteins and a list of neighbors for each of the proteins, remove one protein at a time from the list until those remaining in the list have no more neighbors in the list
* The remaining proteins then represent the selected non-redundant set
* As the number of neighbor relations in the original list is a constant, one can attempt to maximize the number of selected proteins by removing at each step proteins with the largest number of neighbor relations

#### Details of algorithm 2

> Remove until done

* The second algorithm is computationally more expensive, as it requires a complete matrix of pair relations among all proteins in the candidate list.
* The goal of the algorithm is to remove, in the smallest number of steps, one protein at a time, together with its pair relations.
* Solve the problem in practive by a procedure of the "greedy" type:
  * removal of the protein with the largest number of pair relations at a particular step tends to minimize the total number of steps needed to remove all pair relations in the matrix

The algorithm proceeds as follows:

* In each iteration step, the protein with the largest number of relations (neighbors) is removed by setting all its pair relation bits to zero.
  * If the largest number of pair relations is shared by more than one protein, the choice of protein to be removed is made arbitrarily, using a random number generator.
  * The algorithm terminates when all pair relations in the matrix have been set to zero, i.e., all remaining proteins are mutually dissimilar.
  * The proteins remaining are considered as the selected set.
* In a final pass over all removed proteins, a protein is reinstated (added to the selected set) if it has no neighbors in the selected set

In principle, this algorithm can be reformulated so as to guarantee the global optimum of the largest number of unrelated  chains, namely by a complete tree search to arbitary depth.

## Enlarged Representative Set of Protein Structures

> Hobohm U, Sander C. Enlarged representative set of protein structures. Protein Sci. 1994;3(3):522‐524. doi:10.1002/pro.5560030317

### Quality Control of Selected Data Sets

The quality goals can are implemented at 3 levels:

* Initial Filter
* Soft-exclude Flag for marginal data
* Quality Index $Q=r+R/20$
  * $r$: resolution
  * $R$: R-factor (percent)
  * the one with higher Q is considered to be of lower quality

#### Initial Filter

Eliminate all chains with:

* 100% sequence identity on the entire length to another chain, but lower quality
* more than 5% of nonstandard or unknown amino acids(UNK)
* a length of less than 30 residues
* a resolution worse than 3.5 A
* an R-factor of more than 30%
* models not based directly on X-ray or NMR data

> The values for cutoffs were chosen by experience and can be changed to meet special requirements

#### Soft-exclude Flag

Some chains are flagged for preferred exclusion in the subsequent selection procedure. Such chains remain in the list only if no homologous chain exists. These are chains for which:

* the number of residues with sidechain coordinates is less than 90% of the sequence length (e.g backbone-only structure)
* the number of residues with backbone coordinates is less than 90% of the sequence length (e.g C-α-only structures)
* the number of alanine plus glycine residues is higher than 40% of the sequence length (e.g structures woth unknown sequence modeled as polyalanine)
* no data for resolution or R-factor are available

### Selection Procedure

* Algorithm 2
* when more than 1 protein has the same number of neighbors, quality control and downward compatibility are achieved by preferably removing the data set of lower quality and by preferably keepling chains that have been in a previous list
  * remove a chain with a soft-exclude flag.(**?** _what if there are two chains with soft-exclude flag. Let the two chains goto the next step?_) If there is no such chain, then
  * remove the chain with lowest quality. If there is more than 1 chain with the lowest quality, then
  * remove a chain that has no soft-include flag (chains from a previous list are flagged with a soft-include flag). If there is more than 1 chain left, then
  * remove a chain with high(**?** means lower?) PDB identifier (i.e., prefer 6TIM over 2TIM). If there is more than 1 chain left
  * remove a chain with alphabetically higher chain identifier (i.e., prefer 3HLA-A over 3HLA-B)
* For special requirements it may be necessary to have chains of individual choice in the list. To meet such a requirement, a subset of chains can be flagged with a hard-include flag.
  * These chains are not removed during the selection procedure, 
  * and relations between hard-included chains are not considered