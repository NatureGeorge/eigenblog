---
layout: default
date: 2020-04-15
author: Zefeng Zhu
---

# Predicting Protein 3D Structure

## Homology

### Rosetta

* Align query to sequences in PDB
* Use several alignment methods
* Three categories of queries
  * High sequence similarity template(s) (>50% sequence similarity)
  * Medium sequence similarity template(s) (20-50% sequence similarity)
  * Low sequence similarity template(s) (<20% sequence similarity)
* Refine models

#### General Refinement Procedure

* Random changes to backbone torsion angles
* Rotamer optimization of side chains
* Energy minimization of torsion angles (bond lengths and angles kept fixed)

##### High sequence similarity template(s)

* Minimal refinement, focus on regions where alignment is poor
* Refine structures
* Choose best model by final energy

##### Medium sequence similarity template(s)

* Refinement focuses on regions near gaps and insertions, loops in the starting model, and sequence segments with low conservation
* Replaces torsion angles with those from peptides of known structures
* Minimize local structure
* Refine global structure

##### Low sequence similarity template(s)

* Use many more starting models
* More aggressive refinement strategy
  * Rebuild secondary structure elements in addition to regions refined in medium homology
    * gaps and insertions
    * loops in the starting model
    * regions with low conservation

## de novo

### Rosetta

When there is no suitable homology model:

* Monte Carlo search for backbond angles
  * Choose a short region (3-9 amino acids, `Empirical`) of backbone
  * Set torsion angles to those of a similar peptide in PDB
  * Accept with [Metropolis Criteria](./Refine_Structures.html)
* 36,000 MC steps
* Repeat entire process to get 2,000 final structures
* Cluster structures
* Refine clusters