---
layout: default
date: 2020-05-03
author: Zefeng Zhu
---

# Sequence Identity & Similarity & Homology

> _identity_ and _similarity_ are very often used interchangeably (for nucleotide sequence or in the context of graph theory)

## Sequence Identity

> Sequence identity is the amount of characters which match exactly between two different sequences

Having got the alignment by some method, there are many different ways of calculating percentage identity (PID). For example divide the number of identities by:

* length of shortest sequence.
* length of alignment.
* arithmetic mean length of sequence.
* number of non-gap positions.
* number of equivalenced positions excluding overhangs.

$$\text{identity}(\text{Seq}_{A}, \text{Seq}_{B})=100\%\cdot\cfrac{\text{identical characters}}{\min(\text{length}(\text{Seq}_{A}), \text{length}(\text{Seq}_{B}))}$$

$$\text{identity}(\text{Seq}_{A}, \text{Seq}_{B})=100\%\cdot\cfrac{\text{identical characters}}{\text{length}(\text{alignment})}$$

$$\dots$$

* $\text{length}(\text{alignment})$: aligned columns including columns containing a gap in either sequence

> not transitive


## Sequence Similarity

> When one amino acid is mutated to a similar residue such that the physiochemical properties are preserved, a conservative substitution is said to have occurred. ... Thus, percent similarity of two sequences is the sum of both identical and similar matches (residues that have undergone conservative substitution). Similarity measurements are dependent on the criteria of how two amino acid residues are to each other.

> On a BLAST search, **similarity** is also known as **positives**.

$$\text{similarity}(\text{Seq}_{A}, \text{Seq}_{B})=100\%\cdot\cfrac{\text{identical characters}+\text{similar characters}}{\min(\text{length}(\text{Seq}_{A}), \text{length}(\text{Seq}_{B}))}$$

$$\text{similarity}(\text{Seq}_{A}, \text{Seq}_{B})=100\%\cdot\cfrac{\text{identical characters}+\text{similar characters}}{\text{length}(\text{alignment})}$$

$$\ldots$$

$$\text{similarity}(\text{Seq}_{A}, \text{Seq}_{B})=100\%\cdot(1-\cfrac{\text{edit distance}}{\min(\text{unaligned length})})$$

### Similarity Score

$$\sum_{i=1}^{l}\sigma(S'[i],T'[i])$$

* $l=|S'|=|T'|$

## Z-Score

> Boberg J, Salakoski T, Vihinen M. Selection of a representative set of structures from Brookhaven Protein Data Bank. Proteins. 1992;14(2):265‐276. doi:10.1002/prot.340140212

> Webber C, Barton GJ. Estimation of P-values for global alignments of protein sequences. Bioinformatics. 2001;17(12):1158‐1167. doi:10.1093/bioinformatics/17.12.1158

...

## Homology

>  Objects are homologous when they share an evolutionary ancestor. It is a binary concept.

Richard Owen, an English biologist who lived from 1804 to 1892, introduced the term homology, stating that it is "the same organ in different animals under every variety of form and function."

Today, we use the term homology is used to characterize biological species that share a common evolutionary ancestory.

__Strictly a qualitative measure!__

Note that homology is a binary qualitative measure - either two sequences are homologous or not. Homology cannot be quantified. Thus, it is incorrect to claim that "two sequences are 55% homologous" - we use percent identity or similarity to quote numbers, which we will talk about in the next lesson.

> Identity and similarity values are often used to assess whether or not two sequences share a common ancestor or function.

## Reference

1. <https://www.researchgate.net/post/Homology_similarity_and_identity-can_anyone_help_with_these_terms>
2. <https://snipcademy.com/pairwise-alignment>
3. <https://courses.cs.washington.edu/courses/cse527/00wi/lectures/lect03.pdf>
4. <https://github.com/soedinglab/MMseqs2/wiki>
5. <https://openwetware.org/wiki/Wikiomics:Percentage_identity>
6. Gusfield, D. (1997). Algorithms on Strings, Trees, and Sequences: Computer Science and Computational Biology. Cambridge: Cambridge University Press. doi:10.1017/CBO9780511574931
7. Boberg J, Salakoski T, Vihinen M. Selection of a representative set of structures from Brookhaven Protein Data Bank. Proteins. 1992;14(2):265‐276. doi:10.1002/prot.340140212
8. Webber C, Barton GJ. Estimation of P-values for global alignments of protein sequences. Bioinformatics. 2001;17(12):1158‐1167. doi:10.1093/bioinformatics/17.12.1158
