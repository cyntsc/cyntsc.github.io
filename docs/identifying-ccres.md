---
title: "Candidate Regulatory Elements (cCREs)"
parent: "3 Multiome scATAC-seq"
nav_order: 1
permalink: /sATAC-seq-analysis/identifying-ccres/
parent_permalink: /sATAC-seq-analysis/
---

*Cynthia SC* (08-28-2025)

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?logo=linkedin&logoColor=white&style=for-the-badge)](https://www.linkedin.com/in/cynthiacardinault)
<!--[![GitHub](https://img.shields.io/badge/-GitHub-181717?logo=github&logoColor=white&style=for-the-badge)](https://github.com/cyntsc)-->
[![Email](https://img.shields.io/badge/-Email-D14836?logo=gmail&logoColor=white&style=for-the-badge)](mailto:bioinformatic2019@gmail.com)


<!--
<div style="background-color:#fff9db; padding:12px; border-radius:6px; border-left:5px solid #facc15;" markdown="1">
<strong>📌 Note:</strong> For a better understanding of the design of this notebook, please read the
<a href="{{ page.parent_permalink | relative_url }}" style="font-weight:bold; color:#2563eb;">main section page</a> first. Thanks 🙏
</div>
-->

- [1. Background](#1-background)
  - [Calling snATAC-seq peaks using MACS2 and linking peaks to genes](#calling-snatac-seq-peaks-using-macs2-and-linking-peaks-to-genes)
  - [Diagrams and concept maps](#diagrams-and-concept-maps)
- [2. Technical Support Notes](#2-technical-support-notes)
  - [a. Key Literature](#a-key-literature)
  - [b. Vignettes \& Tutorials](#b-vignettes--tutorials)
  - [c. Core Functions](#c-core-functions)
  - [d. Custom Repo Practices](#d-custom-repo-practices)
  - [e. Behind-the-Scenes Tools \& File Formats](#e-behind-the-scenes-tools--file-formats)


---

## 1. Background  
<br>
These notes highlight strategies and resources for the **downstream analysis of single-cell chromatin accessibility**.  
The main goal is to compile practical references and methods to support the identification of candidate cis-regulatory elements (cCREs) for advanced analysis — including tasks such as linking peaks to genes or computing differentially accessible regions (DARs).  

---

### Calling snATAC-seq peaks using MACS2 and linking peaks to genes
<br>
The following video provides a summary of the main topics discussed in this section:  

<iframe width="560" height="315" 
src="https://www.youtube.com/embed/PpgpBkpisO0" 
title="YouTube video player" 
frameborder="0" 
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
allowfullscreen></iframe>

A big thanks to **Leonardo Collado-Torres** from *Lieber Institute for Brain Development* for compiling and editing this recording.  

---

### Diagrams and concept maps

When working with multiome data (RNA + ATAC), Cell Ranger ARC processes the raw data and identifies valid barcodes that contain both RNA and ATAC information. From here, we can build two parallel matrices: one for gene expression counts and another for peak accessibility counts (<span style="background-color:#fff9c4; font-weight:bold;">Image 1</span>).

<div style="background-color:#fff9db; padding:10px; border-radius:8px;">
<details open>
<summary>CellRanger default peak calls</summary>
<p align="center">
<img src="{{ '/images/atac_cisRE/1.jpg' | relative_url }}" alt="Sketch 1" width="300">
</p>
</details>
</div>

To explore regulatory landscapes, we visualize read coverage and chromatin features with CoveragePlot(), which overlays MACS2-called peaks, linked peak–gene relationships, and the gene model (<span style="background-color:#fff9c4; font-weight:bold;">Images 2–3</span>). This provides a clear view of how open chromatin regions may regulate transcription.

<div style="background-color:#fff9db; padding:10px; border-radius:8px;">
<details>
<summary>Peak overview (click to open)</summary>
<p align="center">
  <img src="{{ '/images/atac_cisRE/2.jpg' | relative_url }}" alt="Sketch 2" width="45%">
  <img src="{{ '/images/atac_cisRE/2b.jpg' | relative_url }}" alt="Sketch 2b" width="45%">
</p>
</details>
</div>

Next, we create a chromatin assay: performing GC bias correction, normalization (TF-IDF), and standardization. Then, using Signac::LinkPeaks, we correlate peaks with nearby genes across windows (e.g., ±500 kb around a TSS), evaluating the strength of each regulatory connection (<span style="background-color:#fff9c4; font-weight:bold;">Image 4</span>).

There are two complementary strategies:

*Global peak linking*, where peaks are defined across all cells, merged, normalized, and then linked to genes (broad overview).

*Local peak linking*, where peaks are defined per cluster (via MACS2), quantified, normalized, and linked (more cell-type-specific view) (<span style="background-color:#fff9c4; font-weight:bold;">Image 5</span>).

<div style="background-color:#fff9db; padding:10px; border-radius:8px;">
<details>
<summary>Global vs local peaks (click to open)</summary>
<p align="center">
  <img src="{{ '/images/atac_cisRE/3.jpg' | relative_url }}" alt="Sketch 3" width="45%">
  <img src="{{ '/images/atac_cisRE/4.jpg' | relative_url }}" alt="Sketch 4" width="45%">
</p>
</details>
</div>

The basic “cell-to-cell” approach follows a straightforward workflow: load the Seurat object, load and quantify MACS2 peaks, build the chromatin assay, normalize, and then perform downstream analyses like LinkPeaks or differential accessibility (DARs) (<span style="background-color:#fff9c4; font-weight:bold;">Image 6</span>).

Finally, we can compare cell-to-cell vs pseudobulk strategies. In cell-to-cell, we build a shared peak set (using reduce), quantify fragments, and construct a chromatin assay for fine-grained resolution. In pseudobulk, we aggregate expression at the cluster level for broader, more robust signals. Both converge into analyses of peak–gene links and differential accessibility, but at different resolutions (<span style="background-color:#fff9c4; font-weight:bold;">Image 7</span>).

<div style="background-color:#fff9db; padding:10px; border-radius:8px;">
<details>
<summary>Cell-to-cell vs pseudobulk (click to open)</summary>
<p align="center">
  <img src="{{ '/images/atac_cisRE/5.jpg' | relative_url }}" alt="Sketch 5" width="45%">
  <img src="{{ '/images/atac_cisRE/6.jpg' | relative_url }}" alt="Sketch 6" width="45%">
</p>
</details>
</div>

Together, these steps form a pipeline for uncovering how chromatin accessibility influences gene expression across cell states, balancing global robustness with local specificity.

<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">


## 2. Technical Support Notes

### a. Key Literature  

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

---

### b. Vignettes & Tutorials  

- [Data structures & object interaction (Signac)](https://stuartlab.org/signac/1.11.0/articles/data_structures)  
- [Linking peaks to genes (10x Multiome)](https://stuartlab.org/signac/articles/pbmc_multiomic)  
- [Calling Peaks with MACS2](https://stuartlab.org/signac/articles/peak_calling)  
- [Motif analysis with Signac](https://stuartlab.org/signac/articles/motif_vignette)  
- [Co-accessible networks with Cicero](https://stuartlab.org/signac/articles/cicero)

---

### c. Core Functions

- **Signac**  
  - [CallPeaks](https://stuartlab.org/signac/reference/callpeaks)
  - [FeatureMatrix](https://stuartlab.org/signac/reference/featurematrix)  
  - [RegionStats](https://stuartlab.org/signac/reference/regionstats)  
  - [LinkedPeaks](https://stuartlab.org/signac/reference/linkpeaks)  

- **GenomicRanges**  
  - [reduce](https://bioconductor.org/packages/devel/bioc/vignettes/GenomicRanges/inst/doc/GenomicRangesIntroduction.html)  
  - [IRanges methods](https://rdrr.io/bioc/IRanges/man/inter-range-methods.html)

- **Seurat v5+**  
  - [AggregateExpression](https://satijalab.org/seurat/reference/aggregateexpression)

---

### d. Custom Repo Practices  

- **Peak calling workflows:** [scMultiomics_AD repo](https://aanderson54.github.io/scMultiomics_AD/#peak-calling)  
- **Link detection strategies:**  
  - Filter MACS peak calls (e.g., by expression or cell type).  
  - Keep only genes with sufficient UMIs (e.g., ≥200).  
  - Restrict peak–gene links by distance (≤100 kb from TSS).  
  - Example: [link filtering](https://aanderson54.github.io/scMultiomics_AD/#links).  

---

### e. Behind-the-Scenes Tools & File Formats 
<br>
To run some functions, like `Signac::CallPeaks` (which internally invokes MACS), you might be interested in reading a bit about how MACS work, as you need to install it to let `CallPeaks` operate. Here, I am listing the most elementary resources for making happen, these are:

- **MACS Project:** [GitHub](https://github.com/macs3-project/MACS) and [Changelog](https://macs3-project.github.io/MACS/#changes-for-macs-3-0-3)  
- **Installation:** [MACS2 guide](https://github.com/macs3-project/MACS/wiki/Install-macs2/37ede4928925646fb41d25120a133900038c092e) and [MACS3 guide](https://macs3-project.github.io/MACS/docs/INSTALL.html)  
- **BED = Browser Extensible Data format (standard for genomic intervals):** [UCSC FAQ](https://genome.ucsc.edu/FAQ/FAQformat#format1)  


If you are wondering, Why get into BED format ? 
The short story is that `CallPeaks()` calls `MACS2`, which produces `BED/narrowPeak files`. Then `Signac` reads those into `GRanges`, discarding the need for you to handle files manually. The relation is direct: *CallPeaks() = automated creation + import of MACS2 BED output into R*


This matter if you want to:
1. `Export`: you can always export `Signac’s GRanges` peaks back into a `BED file` (with `rtracklayer::export()`). Useful if you want to load them in `IGV`, `UCSC Genome Browser`, or compare with `ENCODE cCREs`.
2. `Import`: if you already have `BED peaks` (e.g., ENCODE, or your own MACS2 runs outside of Signac), you can load them with `rtracklayer::import()` and use them directly in Signac.
3. Consistency: this BED ↔ GRanges conversion makes it easy to interoperate between R and external genomics tools.

---


Thanks for reading through these notes.
I hope they give you a useful starting point or spark new ideas for your own analysis.  

Feel free to share thoughts, suggestions, or resources. Collaboration makes the whole community stronger.  

Until next time, happy exploring!

---

*Cynthia SC*

