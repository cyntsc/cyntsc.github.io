---
title: "Integration of DNA metilation + scATACseq + scRNAseq * TF"
parent: "3 Multiome scATAC-seq"
nav_order: 2
permalink: /sATAC-seq-analysis/integration_dna_metilation/
parent_permalink: /sATAC-seq-analysis/
---


[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?logo=linkedin&logoColor=white&style=for-the-badge)](https://www.linkedin.com/in/cynthiacardinault)
[![Email](https://img.shields.io/badge/-Email-D14836?logo=gmail&logoColor=white&style=for-the-badge)](mailto:bioinformatic2019@gmail.com)


# Integration of DNA Methylation, scATAC-seq, and scRNA-seq to Infer Transcription Factor Activity

Gene expression regulation depends on the interaction between transcription factors (TFs) and regulatory regions of the genome such as promoters and enhancers. However, the ability of TFs to bind DNA is strongly influenced by the local epigenetic state, including chromatin accessibility and DNA methylation.

In this context, three types of data provide complementary information: **scRNA-seq**,  **scATAC-seq**, and **DNA methylation**. Integrating these layers makes it possible to infer which TFs are active and which genes they regulate, providing a more complete view of the regulatory networks that define cellular states.

In this *notebook*, I present a summary analysis pipeline to integrate these three sources of information using single-cell data. Each omic layer answers a different biological question.

| Data type       | What it tells us                                                      |
| --------------- | --------------------------------------------------------------------- |
| scRNA-seq       | Which genes and TFs are expressed                                     |
| scATAC-seq      | Which genomic regions are accessible                                  |
| DNA methylation | Whether regulatory regions are epigenetically permissive or repressed |

**Overall:**

*TF* activity is inferred, not directly measured. To infer TF activity we combine multiple signals:
```plaintext
TF expressed
+ motif present
+ chromatin accessible
+ low methylation
→ strong evidence that the TF is active
```

Integration reveals *regulatory programs*. When these signals are combined across thousands of cells we can:
```plaintext
identify active transcription factors
map enhancers to target genes
reconstruct cell-type–specific regulatory networks
```

These approaches allow researchers to move from descriptive transcriptomics to mechanistic models of gene regulation.

>Single-cell multi-omics integrates expression, chromatin accessibility, and DNA methylation to identify which transcription factors are active and how they control gene regulatory networks that define cellular identity.


## Summary of the Pipeline

1. Process **scRNA-seq** (gene expression)
2. Identify **TF** expression levels
3. Process **scATAC-seq** (peak detection)
4. Process **DNA methylation** (epigenetic state of regulatory regions)
5. Perform **TF motif** analysis
6. Integrate **multi-omic matrices**
7. Infer **regulatory activity**

---

### Step 1. Processing scATAC-seq and Peak Identification

First, chromatin accessibility data are processed to define open regions of the genome.

Typical steps include read alignment, quality filtering, peak calling and construction of a **peak × cell matrix**.

#### Conceptual Example

| Peak   | Cell1 | Cell2 | Cell3 |
|-------|------|------|------|
| peak_1 | 0 | 4 | 2 |
| peak_2 | 1 | 0 | 0 |
| peak_3 | 5 | 3 | 1 |

These peaks represent potential regulatory regions where transcription factors may bind. 


---
### Step 2. Integrating Methylation with Accessible Regions

Once peaks are defined, they are intersected with methylation data:

`scATAC peaks ∩ methylated CpGs`

This allows the calculation of methylation levels within accessible regions.

#### Conceptual Example

| Peak   | Methylation |
|-------|-------------|
| peak_1 | 0.12 |
| peak_2 | 0.78 |
| peak_3 | 0.25 |

#### Interpretation

- Low methylation + high accessibility → active regulatory region
- High methylation + low accessibility → repressed region


---
### Step 3. Identification of TF Motifs

Next, TF motif scanning is performed within the identified peaks. This allows the detection of potential TF binding sites.

#### Example

**Peak region**
`chr1:10200–10400`


**Motifs detected**

- SOX2  
- GATA1  
- RUNX1  

#### Resulting Matrix (Peak × TF Motif)

| Peak   | SOX2 | GATA1 | RUNX1 |
|-------|------|------|------|
| peak_1 | 1 | 0 | 1 |
| peak_2 | 0 | 1 | 0 |
| peak_3 | 1 | 1 | 0 |


---
### Step 4. Estimating TF Activity Using Chromatin Accessibility

Peaks containing motifs for a given TF can be aggregated to estimate regulatory activity based on accessibility. This strategy is commonly used in methods such as **chromVAR**.

#### Conceptual Workflow

```textplain
TF motif peaks
↓
average accessibility
↓
TF accessibility score
```


---
### Step 5. Integrating Methylation at Motif Sites

DNA methylation is then used to refine TF activity inference. For each `motif`:

- identify nearby **CpGs**
- calculate **average methylation**
- evaluate **patterns of hypomethylation**

#### Conceptual Example

| TF | Accessibility | Methylation |
|----|---------------|-------------|
| SOX2 | high | low |
| GATA1 | low | high |

#### Interpretation

- High accessibility + low methylation → TF likely active
- Low accessibility + high methylation → TF likely inactive


---
### Step 6. Integration with Gene Expression (scRNA-seq)

Finally, transcriptomic information is incorporated. For each `TF`, the following evidence is evaluated:

| Evidence | Interpretation |
|---------|---------------|
| TF highly expressed | potential active regulator |
| motifs in accessible regions | possible binding |
| low methylation at those regions | permissive epigenetic state |

#### Example

**TF: RUNX1**

- high expression  
- motifs in accessible peaks  
- low methylation  

**→ strong evidence of regulatory activity**


---
### Step 7. Inference of Regulatory Networks

Once active `TFs` are identified:

- peaks are linked to nearby genes
- *TF → gene relationships* are inferred
- *cellular regulatory networks* are reconstructed

#### Conceptual Model
```textplain
TF
↓
enhancer / promoter
↓
target gene
```


These networks help identify **cell-type-specific regulatory programs**.

---
## Conclusion

Integrating **chromatin accessibility**, **DNA methylation**, and **gene expression** enables more accurate inference of transcription factor activity and reconstruction of regulatory networks in complex biological systems.

In the context of **single-cell epigenomics**, this type of multi-omic integration is increasingly used to characterize cellular states and differentiation trajectories. Computational frameworks such as **SCENIC**, **ArchR**, and **MOFA** allow these analyses to be implemented systematically, and their use is rapidly becoming standard practice in single-cell epigenomics studies.


---
## Key References

The following publications and resources were consulted as key methodological and conceptual references for the approaches described in this document.

**chromVAR: Inferring transcription-factor–associated accessibility from single-cell epigenomic data**\
Introduces a statistical framework to infer transcription factor (TF) activity from chromatin accessibility data by analyzing deviations in accessibility at TF motif sites in single-cell ATAC-seq datasets.

**SCENIC: Single-cell regulatory network inference and clustering**\
Describes a workflow for reconstructing gene regulatory networks and identifying active transcription factors using single-cell RNA-seq data combined with motif enrichment analysis.

**MOFA+: A statistical framework for comprehensive integration of multi-modal single-cell data**\
Presents a factor analysis framework designed to integrate multiple omics layers (such as transcriptomics, epigenomics, and methylation) to identify shared and modality-specific sources of biological variation.

**ArchR: scalable software for integrative single-cell chromatin accessibility analysis**\
Provides a scalable computational platform for large-scale analysis of single-cell ATAC-seq data, including integration with other modalities, TF motif enrichment, peak-to-gene linkage, and regulatory network inference.

**ENCODE (Encyclopedia of DNA Elements) Project**\
A major international consortium that provides comprehensive reference annotations of regulatory elements, transcription factor binding sites, and epigenomic datasets widely used in regulatory genomics studies.

---

![Visitors](https://visitor-badge.laobi.icu/badge?page_id=el-arkhe.scrnaseq-workshop)

© El Arkhe · Talleres Multiomics

CSC. March 13, 2026