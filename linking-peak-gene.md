---
title: "Identifying candidate cis-regulatory elements (cCREs)"
parent: "scATAC-seq analysis (multiome)"
nav_order: 1
permalink: /sATAC-seq-analysis/link_peak_gene/
---

# Single Cell ATAC-seq: Identifying candidate cis-regulatory elements (cCREs) 
*R workflow using snATACseq with regular tools like Seurat, Signac, MACS2 and GRanges*

**Notebook:** 08-28-2025

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?logo=linkedin&logoColor=white&style=for-the-badge)](https://www.linkedin.com/in/cynthiacardinault)
[![GitHub](https://img.shields.io/badge/-GitHub-181717?logo=github&logoColor=white&style=for-the-badge)](https://github.com/cyntsc)

---

# Welcome!  

Hi there 👋  

I hope you’re doing well, and that you find these notes useful for your analysis.  

You probably landed here because you’re:  
- looking for a quick overview of **snATAC-seq strategies**,  
- searching for considerations when running downstream analysis, or  
- simply exploring out of curiosity  

Whatever the reason, **welcome** 🙌  

This space is a collection of my working notes on **snATAC-seq downstream analysis** "...not a full reproducible repo — there are already plenty of those out there. With the rise of AI coding assistants like ChatGPT, Gemini, or Claude, anyone can generate a decent pipeline if they know what to ask" (But that’s a topic for another day 😉).  

Instead, my goal is to share the kind of material I often find missing:  
resources that are neither **oversimplified** (skipping important steps) nor **overly specific** to a single research question.  

So these notes focus on six main areas:  

1. **Background** – What these notes gather  
2. **Key Literature** – Papers, papers, and more papers  
3. **Vignettes & Tutorials** – Broad, but still helpful  
4. **Core Functions & References** – What’s most relevant now  
5. **Custom Repo Practices** – Tips from my own workflows  
6. **Behind-the-Scenes Tools & File Formats** – The underrated but essential stuff  

From time to time, I also add quick sketches or diagrams — nothing fancy, just visual aids that help me (and hopefully you) grasp the concepts better.  

"Enjoy exploring, and feel free to share feedback. It helps make this resource better for the whole community"

---

## 1. Background  
<br>
These notes highlight strategies and resources for the **downstream analysis of single-cell chromatin accessibility**.  
The main goal is to compile practical references and methods to support the identification of candidate cis-regulatory elements (cCREs) for advanced analysis — including tasks such as linking peaks to genes or computing differentially accessible regions (DARs).  

---

## 2. Key Literature  

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

## 3. Vignettes & Tutorials  

- [Data structures & object interaction (Signac)](https://stuartlab.org/signac/1.11.0/articles/data_structures)  
- [Linking peaks to genes (10x Multiome)](https://stuartlab.org/signac/articles/pbmc_multiomic)  
- [Calling Peaks with MACS2](https://stuartlab.org/signac/articles/peak_calling)  
- [Motif analysis with Signac](https://stuartlab.org/signac/articles/motif_vignette)  
- [Co-accessible networks with Cicero](https://stuartlab.org/signac/articles/cicero)

---

## 4. Core Functions

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

## 5. Custom Repo Practices  

- **Peak calling workflows:** [scMultiomics_AD repo](https://aanderson54.github.io/scMultiomics_AD/#peak-calling)  
- **Link detection strategies:**  
  - Filter MACS peak calls (e.g., by expression or cell type).  
  - Keep only genes with sufficient UMIs (e.g., ≥200).  
  - Restrict peak–gene links by distance (≤100 kb from TSS).  
  - Example: [link filtering](https://aanderson54.github.io/scMultiomics_AD/#links).  

---

## 6. Behind-the-Scenes Tools & File Formats 
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

*Cynthia SC*

