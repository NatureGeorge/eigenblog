---
layout: default
date: 2020-04-26
author: Zefeng Zhu
---

# Representative/Non-redundant Data Set

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