---
title: Variant Annotation
author: Zefeng Zhu
date: 2020-04-13 08:01:00 +0800
categories: [Notes, Courses]
tags: [clinical genomics]
---

## Tool

### [TransVar](https://bioinformatics.mdanderson.org/public-software/transvar/)

> Zhou W., Chen T., Chong Z., Rohrdanz M.A., Melott J.M., Wakefield C., Zeng J., Weinstein J.M., Meric-Bernstam F., Mills G.B., et al. TransVar: A multilevel variant annotator for precision genomics. Nat. Methods. 2015;12:1002. doi: 10.1038/nmeth.3622. [GitHub Link](https://github.com/zwdzwd/transvar)

![fig](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4772859/bin/nihms761865f1.jpg)

* A multi-way annotator for genetic elements and genetic variations
* Operates on
  * genomic coordinates 
    * `chr3:g.178936091G>A`
  * and transcript-dependent cDNA as well as protein coordinates 
    * `PIK3CA:p.E545K` or `PIK3CA:c.1633G>A`
    * or `NM_006218.2:p.E545K`, or `NP_006266.2:p.G240Afs*50`
* and was designed to resolve ambiguous mutation annotations arising from differential transcript usage.
* Supports
  * [HGVS nomenclature](https://varnomen.hgvs.org/)
  * both left-alignment and right-alignment convention in reporting indels
  * annotation of a region based on a transcript dependent characterization
  * single nucleotide variation (SNV), insertions and deletions (indels) and block substitutions
  * mutations at both coding region and intronic/UTR regions
  * transcript annotation from commonly-used databases such as Ensembl, NCBI RefSeq and GENCODE etc
  * UniProt protein id as transcript id
  * GRCh36, 37, 38
  * forward annotation

### [OpenCRAVAT](https://opencravat.org/index.html)

> Pagel KA et al. Integrated Informatics Analysis of Cancer-Related Variants. JCO Clinical Cancer Informatics 2020 4, 310-317. doi: 10.1200/CCI.19.00132 [GitHub Link](https://github.com/KarchinLab/open-cravat)


<table>
    <tr>
        <td>
        <img src='https://github.com/KarchinLab/open-cravat/wiki/figures/OpenCRAVAT_Overview3.png
'></td>
        <td>
        <img src='https://github.com/KarchinLab/open-cravat/wiki/figures/OC_schematic.png'>
        </td>
    </tr>
</table>

* A python package that performs genomic variant interpretation including variant impact, annotation, and scoring
* Has a modular architecture with a wide variety of analysis modules that can be selected and installed/run based on the needs of a given study
* It takes a file of genomic variants as input. 
  * The most common input format is a VCF file but other formats are supported including dbSNP identifiers, 23&Me and Ancestry.com file formats.
* The analysis performed by OpenCRAVAT depends upon user-selected annotation and visualization options, available for download from the free OpenCRAVAT Store. 
* In addition to the interactive user interface, OpenCRAVAT provides several output formats including text reports, Excel spreadsheets, and a SQLite database of results used by cravat_view.
* When the pipeline program is run, it will execute a series of modules required for variant analysis. 
  * First, the appropriate converter will be run to parse the input variant file. 
  * Next, a mapper module will determine the transcripts and associated genes affected by each variant including protein impact. 
  * Then OpenCRAVAT runs all of the requested/installed annotation modules and after all annotation is complete, an aggregator program collects and collates the results into a SQLite database. 
  * Finally, reporter modules are run to produce the requested format of results.


### [Annovar](https://doc-openbio.readthedocs.io/projects/annovar/en/latest/)

> Wang K, Li M, Hakonarson H. ANNOVAR: Functional annotation of genetic variants from next-generation sequencing data. Nucleic Acids Research, 38:e164, 2010

* An efficient software tool to utilize update-to-date information to functionally annotate genetic variants detected from diverse genomes
  * including human genome hg18, hg19, hg38
  * as well as mouse, worm, fly, yeast and many others
* Given a list of variants with chromosome, start position, end position, reference nucleotide and observed nucleotides, ANNOVAR can perform:
  * Gene-based annotation
  * Region-based annotation
  * Filter-based annotation
  * Other functionalities