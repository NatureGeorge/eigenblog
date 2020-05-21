---
title: Experiment of Clinical Genomics Course
author: Zefeng Zhu
date: 2020-05-04 08:00:00 +0800
categories: [Notes, Courses]
tags: [clinical-genomics]
---

## Basic Information

> This is a experiment guidence.

## Databases

* ClinVar
  * annotate with existing data
  * big (a large amount of data)
* ClinGen
  * expert reviewed annotation
  * more precise classification
  * relatively small (compared with ClinVar)
* ExAC(exon) & gnomAD(whole genome)
  * intergred
  * ExAC contain data more than gnomAD
  * population data, got allele frequence (not variant data)
    * input a group of variants, then they can help defining common variants, rare variants
  * Focus on interest gene
* MARRVEL
  * Interface that integrate various databases
* HGMD
  * Disease mutation, demaging mutation ...
  * Mutation Classification
* GDC
  * Data Portal
  * dataset of TCGA
  * Cancer
  * 权威
* ICGC Data Portal
  * Data Portal
  * Cancer
  * 权威
* COSMIC
  * DataBase
  * from ICGC, GCD, Text mining, other research lab
  * data processing
  * cancer census gene, cancer cell line
  * Statistics and Classification
* CGI
  * integreate multiple databases
  * discover cancer driver mutation
  * 肿瘤异质性 (在这个sample是driver mutation, 另一个sample就不一定是)
  * most are rare mutation (appear only once)
* CIVIC
  * Clinical, Precision Medicine
  * According to Drug Treatment

## Annotation Tools

* ANNOVAR
  * powerful
* TransVar
  * ID Mapping
* OpenCRAVAT
  * integrate existing db/tool
* GDC Data Transfer Tool
* GTEx Portal
  * 表达
* PMKB
  * annotation
  * integrate multi db 可以学习其是如何整合多方数据
* VarSome
  * need user's knowledge to inteperate results
* REVEL
* VEST4
* DEOGEN2
* PrimateAI
* dbNSFP
  * V4.0
  * used by annovar
  * keep update
  * all human genome possible mutation
* CHASMplus
* ParsSNP
* MutaGene Bscore (non-training)
* MutPred2
* MutFunc
* Interactome INSIDER
* G2S
* PhyreRisk

## Q1

> Assuming that you are given a data set of human genome exon-sequenceing data, which tools that you have learned during the courses can you use to perform analysis?

1. Collect mutation data via some mutation discovery tools
2. Crude classification of the mutations and choose corresponding tools
3. Annotate these mutation via gnomAD, Annovar etc.