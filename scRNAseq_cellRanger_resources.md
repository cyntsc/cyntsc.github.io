---
title: "Getting started with single-cell"
parent: "3 scRNA-seq analysis"
nav_order: 1
permalink: /single_cell_RNA-seq/starting/
parent_permalink: /single_cell_RNA-seq/
---

# Single Cell RNA-seq Analysis: "Cell Ranger Pipeline"
**Notebook:** 09-25-2025

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?logo=linkedin&logoColor=white&style=for-the-badge)](https://www.linkedin.com/in/cynthiacardinault)
<!--[![GitHub](https://img.shields.io/badge/-GitHub-181717?logo=github&logoColor=white&style=for-the-badge)](https://github.com/cyntsc)-->
[![Email](https://img.shields.io/badge/-Email-D14836?logo=gmail&logoColor=white&style=for-the-badge)](mailto:bioinformatic2019@gmail.com)

<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

<div style="background-color:#fff9db; padding:12px; border-radius:6px; border-left:5px solid #facc15;" markdown="1">
📌 Notebook design, go to <a href="{{ page.parent_permalink | relative_url }}" style="font-weight:bold; color:#2563eb;">main section page</a> 
</div>


- [Single Cell RNA-seq Analysis: "Cell Ranger Pipeline"](#single-cell-rna-seq-analysis-cell-ranger-pipeline)
  - [1. Getting Started with 10x Genomics Cell Ranger](#1-getting-started-with-10x-genomics-cell-ranger)
  - [2. Technical Support Notes](#2-technical-support-notes)


<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

## 1. Getting Started with 10x Genomics Cell Ranger
<br>

If you’ve just generated your first single-cell dataset on the 10x Genomics Chromium platform, chances are you’ll meet **Cell Ranger**—the software suite that transforms raw sequencing output into clean, interpretable data.

Think of Cell Ranger as your pipeline companion: it takes messy raw files straight from the sequencer and turns them into gene-by-cell count matrices, quality metrics, and interactive reports. From there, you can dive deeper into clustering, dimensionality reduction, and downstream biological insights.

---

**What Does Cell Ranger Do?**

Here’s a bird’s-eye view of the core steps:

<p align="center">
<img src="{{ '/images/scRNAseq/cellranger_workflow.png' | relative_url }}" alt="Sketch 1" width="300">
</p>


| Step                        | What Happens                                                                                 | What You Get                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **1. `cellranger mkfastq`** | Converts raw sequencing BCL files into FASTQ files and demultiplexes samples.                | FASTQ files                                                   |
| **2. `cellranger count`**   | Aligns reads, identifies barcodes and UMIs, filters noise, and builds the expression matrix. | Gene-barcode matrix, BAM file, summary report, `.cloupe` file |
| **3. `cellranger aggr`**    | Combines results from multiple runs into one dataset.                                        | Aggregated gene-barcode matrix                                |
| **4. Secondary Analysis**   | Calls real cells, reduces dimensions (PCA, UMAP/t-SNE), and clusters cells.                  | Filtered matrix + interactive Loupe Browser exploration       |



## 2. Technical Support Notes
<!--
- **Chawla et al., 2025 (Nat Genet)**  
  *Single-nucleus chromatin accessibility profiling identifies cell types and functional variants contributing to major depression.*  
  [DOI link](https://doi.org/10.1038/s41588-025-02249-4)  
  Includes code for DAR analysis (cell-type/cluster-specific cCREs and TFs): [GitHub repo](https://github.com/MGSSdouglas/snATAC_MDD/blob/0db3e853badaca009407e887e7c6064cd3bbf2fe/3_differential_analysis/README.md)

- **Zeng et al., 2024 (Science)**  
  *Genetic regulation of cell type–specific chromatin accessibility shapes brain disease etiology.*  
  [DOI link](https://doi.org/10.1126/science.adh4265)

- **Anderson et al., 2023 (Cell Genomics)**  
  *Single nucleus multiomics identifies ZEB1 and MAFB as candidate regulators of Alzheimer’s disease-specific cis-regulatory elements.*  
  [DOI link](https://doi.org/10.1016/j.xgen.2023.100263)  
  Includes code for peak calling: [Workflow](https://aanderson54.github.io/scMultiomics_AD/#peak-calling)
-->

- Official 10X web page [getting started guide](https://www.10xgenomics.com/support/software/cell-ranger/latest/getting-started).

---

Hope you have a useful starting point. Feel free to share thoughts, suggestions, or resources here or in my social networks . Collaboration makes the whole community stronger.  

[![View my LinkedIn post](https://img.shields.io/badge/See%20on-LinkedIn-blue?logo=linkedin)](https://www.linkedin.com/posts/...)

[![View post on X](https://img.shields.io/badge/See%20on-X-black?logo=twitter)](https://twitter.com/handle/status/1234567890123456789)



---

*Cynthia SC*

