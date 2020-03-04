---
layout: default
date: 2020-02-29
author: Zefeng Zhu
---


# Note of some special PDB Entry

## Content

### 5VGA

> PubMed: 29113027

__Re-refinement Note__

This entry reflects an alternative modeling of the original data in: 

* 2A6I - determined by Sethi, D.K., Agarwal, A., Manivel, V., Rao, K.V., Salunke, D.M.


### 4CSC

With UNK residue

### 6BCU

Identity values with Q8N122 and Q8N122-2 are wrong.

#### Feedback to EMBL-EBI

```mail
Hi, I am implementing the REST calls based on SIFTS mappings to get Range-mapping between PDB-chain and UniProt-Isoform. However, I found some error-prone information in the results. Here is an example:

GET: https://www.ebi.ac.uk/pdbe/api/mappings/all_isoforms/6bcu

And I Got Q8N122's [2,1335] mapped with 6bcu W chain's [10,1343] with identity 0.19. But the alignment of these two sequences shows that they are identical in the mapped range and there are only 9 residues of 6bcu_W and 1 residue of Q8N122 outside the range.

I am confused about the result. Could you please help me figure out this issue?

(2020-02-29)
```