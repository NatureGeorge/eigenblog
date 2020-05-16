---
layout: default
date: 2020-05-12
author: Zefeng Zhu
---

# DESeq2

## Goal

Want their normalization to handle:

1. Differences in library sizes
2. Differences in library composition

The goal is calculate a scaling factor for each factor. The scaling factor has to take **read depth** and **library composition** into account.

## Step

* Take the log of all the values
  * (default) log base e: $\log_{e}$
* Average each row
  * average of log values (after raised by e, this is geometric average)
  * $\cfrac{1+\text{Infity}}{2}=\text{Infity}$
* Filter Out Genes with Infinity
  * filter out genes with zero read counts in one or more samples
  * help focus the scaling factors on the house keeping genes - genes that transcribed at similar levels regardless of tissue type
* Subtract the average log value from the log(counts)
  * $\log(\text{reads for gene X})-\log(\text{average for gene X})=\log(\cfrac{\text{reads for gene X}}{\text{average for gene X}})$
* Calculate the median of the ratios for each sample
  * avoid extreme genes from swaying the value too much in one direction
  * genes with huge differences in expression have no more influence on the median than genes with minor differences
* Convert the medians to "normal numbers" to get the final scaling factors for each sample
  * raise e to the median value for each sample
* Divide the original read counts by the scaling factors

> Personal Note: 本质上就是$\cfrac{x\cdot\text{Geometric Average}}{\text{Median}}$

## Reference

1. Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biol. 2014;15(12):550. doi:10.1186/s13059-014-0550-8
2. [StatQuest: DESeq2, part 1, Library Normalization](https://www.youtube.com/watch?v=UFB993xufUU)