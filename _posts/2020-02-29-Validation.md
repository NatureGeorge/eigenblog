---
title: Validation of PDB
author: Zefeng Zhu
date: 2020-02-29 08:30:10 +0800
categories: [Notes, Research]
tags: [protein, pdb, protein structure]
---

## wwPDB X-ray validation reports

According to the recommendations of the wwPDB X-ray Validation Task Force (VTF)

> A New Generation of Crystallographic Validation Tools for the Protein Data Bank. Structure. 2011 Oct 12; 19(10): 1395–1412 doi: 10.1016/j.str.2011.08.006

The report
* summarises the quality of the structure 
* and highlights specific concerns 
* by considering 
  * the atomic model, 
  * the diffraction data 
  * and the fit between the atomic model and the diffraction data ([Gore et al., 2012](https://doi.org/10.1107/S0907444911050359); [Gore et al., 2017](https://doi.org/10.1016/j.str.2017.10.009)).

> Implementing an X-ray validation pipeline for the Protein Data Bank. Acta Crystallogr D Biol Crystallogr. 2012 Apr;68(Pt 4):478-83. doi: 10.1107/S0907444911050359. Epub 2012 Mar 16.

> Validation of Structures in the Protein Data Bank. Methods Enzymol. 2003;374:370-85. doi: 10.1016/S0076-6879(03)74017-8.

### Overall quality at a glance

This section provides a succinct "executive" summary of key quality indicators. If there should be serious issues with a structure, this would usually be evident from this summary.

The metrics shown in the "slider" graphic (see examples below) compare several important global quality indicators for this structure with __those of previously deposited PDB entries__. 

* The comparison is carried out by calculation of the percentile rank
  * i.e. the percentage of entries that are equal or poorer than this structure in terms of a quality indicator. 
* The global percentile ranks (black vertical boxes) are calculated with respect to all X-ray structures available in the PDB archive up to 27 December 2017. 
* The resolution-specific percentile ranks (white vertical boxes) are calculated with respect to a subset of X-ray entries in the same subset of the PDB archive, but only considering entries with comparable resolution to this entry. 

In general, one would of course like all sliders to lie far to the right in the blue areas (especially for recently determined structures, and especially the resolution-specific sliders).