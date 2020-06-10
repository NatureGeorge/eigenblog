---
title: Predicting Protein-Protein Interactions from the Molecular to the Proteome Level
author: Zefeng Zhu
date: 2020-06-10 08:30:00 +0800
categories: [Notes, Research]
tags: [protein, interaction, interface, ppi]
---

<script src="../../assets/js/ngl.js"></script>
  <script>
    document.addEventListener("DOMContentLoaded", function () {
      var stage1 = new NGL.Stage("viewport1");
      stage1.loadFile("../../assets/data/2xqt.pdb1", {defaultRepresentation: true}).then(function (o) {
        o.addRepresentation("cartoon", { color: "modelindex" }),
        o.autoView()
        });
      stage1.spinAnimation.axis.set(0, 1, 0);
      stage1.setSpin(true);
    });
</script>

## Basic Information

This blog is a personal note of the following review.

> Keskin O, Tuncbag N, Gursoy A. Predicting Protein-Protein Interactions from the Molecular to the Proteome Level. Chem Rev. 2016;116(8):4884‐4909. doi:10.1021/acs.chemrev.5b00683

![fig](https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/chreay/2016/chreay.2016.116.issue-8/acs.chemrev.5b00683/20160421/images/medium/cr-2015-00683m_0015.gif)

## Outline

> From Abstract

* Role
  * Identification of PPIs is at the center of molecular biology considering the unquestionable role of proteins in cells
  * Combinatorial interactions result in a repertoire of multiple functions
  * Given experimental limitations to find all interactions in a proteome, computational prediction/modeling of protein interactions is a prerequisite to proceed on the way to complete interactions at the proteome level
* The review aims to
  * provide a background on PPIs and their types
  * review and list the state-of-the-art methods, servers, databases, and tools for protein–protein interaction prediction
* Methodology
  * Computational methods for PPI predictions can use a variety of biological data including
    * sequence-,
    * evolution-,
    * expression-,
    * and structure-based data.
  * Physical and statistical modeling are commonly used to integrate above data and infer PPI predictions.

## Introduction

> Background

* Complexity in biological data
  * cannot be explained by the number of genes of the organism
  * alternative splicing
  * post-translational modifications (such as phosphorylation, acetylation)
  * tissue specificity
  * cellular localization
  * communication between proteins
    * Proteins do not act in isolation, and more than 80% of all proteins in the cell interact with other molecules to get functional
    * Protein interactions tell us how proteins come together to construct metabolic and signaling pathways in order to fulfill their functions.
    * Having a complete map of protein interactions is even more difficult (compared with the genome) because of
      * the **temporal** and **spatial** heterogeneity in protein interactions
      * Proteins may need to be chemically modified such as phosphorylated to interact with their partners
      * The localization and transportability of the proteins
      * The expression levels of the proteins vary across different tissues as the proteome maps illustrated -> protein interactomes are not the same in all cell types
      * Proteins are prone to changes in their three-dimensional structure, which directly changes their binding preferences
* Interactome
  * The complete map of protein interactions that can occur in an organism is called the interactome
  * Available PPI data was estimated to represent only 10% of all PPIs in human (2006) -> not be a good representative set of the complete interactome
    * Still the known part of the interactome is valuable for explaining biological processes
  * Protein–protein interactions are dynamic (as the heterogeneity explained above)
    * This heterogeneity leads to difficulty in PPI detection techniques and thus in definition of an interactome
  * It is still an open question whether a complete interactome will ever be found by experimental techniques
  * **Predictive methods** became increasingly popular in the systems biology era to reveal the interaction principles at multiple scales, to detect new interactions, and to construct structural assemblies and networks of proteins
  * However, computational methods will only be better and cover more interactions with the help of reliable experimental data
* Following Content: provides available computational methods to predict
  * interaction of protein pairs
  * binding regions
  * structrual assemblies of proteins

> Given the incomplete knowledge of PPIs and the interactome, in silico approaches emerge to fill out the gap and assist in reconstructing pathway maps.

These methods can be classified into four groups based on the question they are addressing: 

1. Which proteins interact with which others?
2. What are the types of interactions and their importance?
3. At which region of a protein does the binding occur?
4. How strong is the interaction between two proteins which implies the importance of binding energies?

<table>
    <tr>
        <td>
            <img src="https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/chreay/2016/chreay.2016.116.issue-8/acs.chemrev.5b00683/20160421/images/medium/cr-2015-00683m_0002.gif">
        </td>
    </tr>
    <tr>
        <td>
             Conceptual view on PPI prediction methods. PPI prediction approaches can be divided into three groups at the top level
        </td>
    </tr>
    <tr>
        <td>
             (i) Pairwise PPI prediction methods to learn which proteins interact. Learning-based approaches, literature mining and scoring, interolog search, gene or domain fusion, gene coexpression, and co-occurrence belong to this class of PPI prediction.
        </td>
    </tr>
    <tr>
        <td>
             (ii) Binding site prediction methods to learn which region on a protein surface is used for binding. Binding patch and motif search belongs to this class.
        </td>
    </tr>
    <tr>
        <td>
             (iii) Protein assembly prediction to find out how proteins interact and form a complex. Docking and template-based prediction methods belong to this class.
        </td>
    </tr>
    <tr>
        <td>
             Eventually, each of these methods contributes to constructing a more complete interactome.
        </td>
    </tr>
</table>


## Experimental Detection of Protein Interactions

* Pairwise protein interactions can be detected with high-throughput or low-throughput experimental techniques.
  * The most popular experimental methods for protein interactions are
    * the yeast-two-hybrid (Y2H) system
      * false positives challenge: the failure of considering the dynamic aspects of protein interactions
      * false negatives challenge: detection of interactions occurring after post-translational modifications is not possible
      * another drawback: only identify binary interactions
    * affinity purification followed by mass spectrometry (AP-MS)
      * drawback: Compared to the Y2H system, transient interactions are less represented in the AP-MS approach
      * drawback: indirect (nonphysical) interactions can be included with AP-MS
      * However, with recently developed cross-linking and quantitative proteomic technologies, direct and dynamic interactions can also be elucidated by AP-MS.
      * drawback again: biased toward highly abundant proteins; not successful in discriminating the specific interactions
    * and literature-derived low-throughput experiments
    * luminescence-based mammalian interactome mapping (LUMIER)
    * Other techniques to identify PPIs include co-immunoprecipitation, protein microarrays, fluorescence spectroscopy, resonance-energy transfer systems, mammalian-two-hybrid, mammalian protein–protein interaction trap (MAPPIT), phage display, surface plasmon resonance, protein-fragment complementation assay, and isothermal titration calorimetry (ITC)...

## PPI Types and Characteristics

* transient
* permanent
* non-obligate
* obligate

> However, usually a continuum exists and it is not straightforward to separate PPIs into one of the classes [link](https://pubs.acs.org/servlet/linkout?suffix=ref6/cit6&dbid=8&doi=10.1021%2Facs.chemrev.5b00683&key=12853464).

### Homo-Oligomeric and Hetero-Oligomeric Complexes

* Homo-oligomers
  * The proteins in a complex are identical (interactions occurring between identical protein chains)
  * Mostly symmetric and provide a good scaffold for stable macromolecules
  * Most of the homodimers are only observed in the oligomeric form
  * Often impossible to separate them into independently stable folded monomers
* Hetero-oligomers
  * Interactions occurring between nonidentical chains
  * The stability of hetero-oligomers varies

<table>
    <tr>
        <td>
            <div id="viewport1" style="width:40em; height:15em;"></div>
        </td>
    </tr>
    <tr>
        <td>
            The cyclic structure of ATP sythase C-ring is an example of the homooligomeric, highly symmetric, and stable protein complexes 
        </td>
    </tr>
</table>

### Obligate and Nonobligate Complexes

> In order to classify interactions as obligate/nonobligate, one needs to know the affinity and stability of the proteins in the complex and monomeric states, see section 6.

* If the proteins (monomers) of a complex are unstable on their own in vivo then this is an obligate interaction, whereas the components of nonobligate interactions can exist independently.
* Obligate interactions are named as two-state folders
  * Protein components fold and bind at the same time to form stable complexes
  * The individual proteins cannot exist as stable, folded structures
* The components of the nonobligate interactions are three-state folders
  * they first fold and then come together to form the complex

### Transient and Permanent Complexes

Protein interactions can be classified based on the lifetime of the complex. This classification is relevant only to nonobligatory interactions.

* Permanent interactions are usually very stable
  * once two proteins interact they permanently stay as a complex
* Transient interactions associate and dissociate temporarily in vivo
  * Binding between hormone–receptors, signal transduction, inhibition of proteases, and chaperone-assisted protein folding are examples of transient interactions
  * These types of interactions dominate signaling and regulatory pathways as they provide a mechanism for the cell to quickly respond to extracellular stimuli and relay the signals when needed.

> It is hard to differentiate between transient/permanent and obligate/nonobligate complexes. Usually these classifications are intermixing such that obligate interactions are permanent whereas nonobligate interactions can be either transient or permanent. 

Usually binding free energy is used to determine the stability and affinity of complexes. The interacting proteins of permanent complexes are more likely to be coexpressed and colocalized than proteins in transient complexes.

<table>
    <tr>
        <td>
            <img src="https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/chreay/2016/chreay.2016.116.issue-8/acs.chemrev.5b00683/20160421/images/medium/cr-2015-00683m_0003.gif">
        </td>
    </tr>
    <tr>
        <td>
             Classification of PPI types
        </td>
    </tr>
    <tr>
        <td>
             On the basis of the stability of complex, interaction can be obligate or nonobligate.
        </td>
    </tr>
    <tr>
        <td>
             On the basis of the lifetime of the complex, interaction can be permanent or transient. Affinity of the interaction implies if the interaction is weak or strong.
        </td>
    </tr>
    <tr>
        <td>
             On the basis of composition, a complex can be a homo- (upper panel dimers) or heterodimer (lower panel cases).
        </td>
    </tr>
</table>

### Disordered-to-Ordered Complexes

A not so well-defined type of PPIs is the one formed by disordered proteins.

> Intrinsically disordered proteins have regions that are unstructured whose amino acid compositions cannot provide a stable folded structure.[link](https://pubs.acs.org/servlet/linkout?suffix=ref37/cit37&dbid=8&doi=10.1021%2Facs.chemrev.5b00683&key=11381529)

* Disordered proteins are especially abundant in eukaryotic proteins including the tails of histone proteins and proteins that control the cell division cycle and signaling.
* These proteins can bind to several different proteins by adapting a conformation compatible with partner proteins.
* Post-translational modifications on these regions also mediate binding to different partners.

> A large portion or a small region of the protein might be disordered (intrinsically disordered).

> Larger disordered segments can fold simultaneously when they bind to their biological targets (coupled folding and binding), whereas shorter flexible disordered linkers might have a role in the assembly of macromolecular complexes.

### Biological and Crystal Complexes

* Structures found by X-ray crystallography in the Protein Databank (PDB) can contain *nonbiological contacts* which can be considered as *experimental artifacts*
  * Although these interactions might sometimes be similar to biological ones, for accurate analysis of protein binding preferences and properties, crystal contacts need to be identified.
  * Toward this aim, there are several efforts to distinguish biological complexes from crystal ones
    * The most discriminative feature between crystal and biological interfaces is the interface area size
      * Although interface area size is a good determinant of crystal interfaces, there are also counter examples having a very large interface area but the interface is completely composed of crystal contacts
    * Another approach to distinguish biological interfaces from nonbiological ones is checking the conservation rate
    * Combination of interface size and conservation distinguishes biological interfaces with an accuracy of 98.3%. [link](https://pubs.acs.org/servlet/linkout?suffix=ref47/cit47&dbid=8&doi=10.1021%2Facs.chemrev.5b00683&key=11800565)
    * Amino acid compositions of interfaces are another feature to distinguish biological interfaces
      * If the amino acid composition of the interface is similar to the rest of the protein surface then these interfaces are labeled as nonbiological
    * Some other interface properties to distinguish biological and crystal interactions are hydrogen bonds and salt bridges across the interface, free energy, and hydrophobicity

### Physicochemical Properties of PPI Binding Sites

> Structural aspects, physicochemical properties, affinity, and specificity of binding are diverse across different protein–protein interfaces

Proteins interact through their interfaces. 

* Structural aspects,
* physicochemical properties, 
* affinity, 
* and specificity of binding are diverse across different protein–protein interfaces.


> In this section, we review characteristics of protein interfaces and available databases and tools about protein interface properties. For the analysis of binding preferences of proteins, interface regions need to be extracted.

* There are several approaches to find interface regions from 3-dimensional coordinates of a protein complex, such as 
  * calculating the accessible surface area (ASA) of the residues
    * If the difference between the ASA of a residue in monomeric state and complex state is greater than a threshold (usually 1 Å2) that residue is labeled to be an interface residue.
  * calculating the atomic distances
    * If the distance between any atoms of two residues each from one chain of a protein complex is less than a threshold (usually 4.5 Å) those residues are labeled as contacting.

A list of some available protein interface databases and tools to find interfaces is provided in ...


* Physicochemical properties of protein–protein interfaces include structural and chemical properties. These should be examined to understand the nature of the intermolecular interactions. 
  * For example, the surface area that is buried by the interacting molecules and the nonpolar fraction, 
  * the hydrogen bonds and the salt bridges across the interface, buried water molecules, the charge distribution and the composition of the interface, 
  * residue conservation, 
  * the strength of the interaction, 
  * flexibility of the interface residues 
  * and residues that contribute significantly to the free energy of binding (hot spots), 
  * the shape of the binding interface, 
  * complementarity of two binding sites, 
  * and the types of secondary structures are some of the properties of binding sites.

* One of the most **striking** features in protein binding is the energy distribution in the interface region. 
  * Hot spots in protein interfaces are energetically critical and contribute more to the binding.
    * These residues can be found experimentally by alanine scanning mutagenesis.
      * If there is a change in binding affinity, usually a variation in binding energy greater than 2 kcal/mol, when a residue is mutated to alanine then this residue is labeled a hot spot.
        * As an example to show an interface and hot spot localizations, Ras/Raf1 complex is illustrated in Figure 3, highlighting predicted hot spots(53) in the interface region. 
      * Although the alanine scanning experiment is invaluable in hot spot identification, the available data deposited in several databases is limited. 
    * Given these limitations, several predictive methods have been developed which successfully distinguish hot spots from nonhot spots in protein interfaces. 
      * Computational alanine scanning is one of them.
      * Other methods include learning-based and molecular dynamics-based approaches
    * The most discriminative feature in hot spot prediction is the **solvent accessibility**
      * Usually hot spots are buried and excluded from solvent and found in close proximity to each other.
    * Some computational methods have been listed in Table 2. 
  * Hot spots are potential drug targets, and drug molecules have a tendency to bind hot regions in protein interfaces


* As to the chemical properties of protein interfaces
  * aromatic side chains have preference to be in the binding site.
  * Also, the stability and specificity of protein interactions are highly dependent on the presence of hydrogen bonds, electrostatic interactions, salt bridges, and hydrophobic attractions. 
  * Although the frequency of seeing disulfide bonds is very low, they contribute to the rigidity and stability of relatively small protein complexes. 
* Protein interfaces can be divided into core and rim regions where the rim region is more exposed to the solvent. 
  * Core regions are shown to be more similar to the interior part of the proteins, 
  * and rim regions are more similar to the protein surface in terms of residue frequency.
  * Besides, protein binding regions are less flexible than the remaining surface region

* There are differences between interfaces of different types of interactions.
  * For example, permanent complexes are more hydrophobic compared to transient interfaces.
  * While interfaces of the obligate ones are more conserved in sequence than the transient ones,
    * the shape complementarity is less important in transient interactions. 
  * Hydrophobic interactions are more preferred in obligate complexes, 
    * while salt bridges and hydrogen bonds are more preferred in transient complexes. 
  * In globular complexes and receptor–ligand complexes, interfaces are larger than transient and oncogenic interactions.


  * Stefin B/papain has a relatively smaller interface area compared to the interface in methylmalonyl-CoA mutase complex. The gap volume index (GV index) between interacting pairs gives some insight about the interface complementarity which is the gap volume between two protein interfaces normalized with the interface area size. A small GV index corresponds to better complementarity. For example, the interface complementarity of methylmalonyl-CoA mutase complex (GV index = 1.65 Å) is higher than stefin B/papain complex (GV index = 2.12 Å). The nonobligate stefin B/papain interface has 7 hydrogen bonds and 105 nonbonded atomic interactions. In the obligate methylmalonyl-CoA mutase interface, 30 hydrogen bonds, 10 salt bridges, and 649 nonbonded atomic interactions are formed.