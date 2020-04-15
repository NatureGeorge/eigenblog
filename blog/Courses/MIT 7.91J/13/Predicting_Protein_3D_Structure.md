---
layout: default
date: 2020-04-15
author: Zefeng Zhu
---

# Predicting Protein 3D Structure

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