---
title: "Linking peaks to genes"
parent: "scATAC-seq analysis (multiome)"
nav_order: 1
permalink: /sATAC-seq-analysis/link_peak_genes/
---

# Chromatin Accessibility Downstream Analysis for High-Confidence Peaks

**Notebook:** Cynthia SC (08-28-2025)

---

## Background  

These notes focus on strategies and resources for downstream analysis of high-confidence chromatin accessibility peaks.  
The goal is to gather resources to explore/deepen strategies to identify candidate cis-regulatory elements (cCREs) for advanced analysis.  

---

## Key Literature  

- **Chawla et al., 2025 (Nat Genet)**  
  *Single-nucleus chromatin accessibility profiling identifies cell types and functional variants contributing to major depression.*  
  [DOI link](https://doi.org/10.1038/s41588-025-02249-4)  
  Code for DAR (cell-type/cluster-specific cCREs & TFs): [GitHub repo](https://github.com/MGSSdouglas/snATAC_MDD/blob/0db3e853badaca009407e887e7c6064cd3bbf2fe/3_differential_analysis/README.md)

- **Zeng et al., 2024 (Science)**  
  *Genetic regulation of cell type–specific chromatin accessibility shapes brain disease etiology.*  
  [DOI link](https://doi.org/10.1126/science.adh4265)

- **Anderson et al., 2023 (Cell Genomics)**  
  *Single nucleus multiomics identifies ZEB1 and MAFB as candidate regulators of Alzheimer’s disease-specific cis-regulatory elements.*  
  [DOI link](https://doi.org/10.1016/j.xgen.2023.100263)  
  Code for peak calling: [Workflow](https://aanderson54.github.io/scMultiomics_AD/#peak-calling)

---

## Vignettes & Tutorials  

- [Data structures & object interaction (Signac)](https://stuartlab.org/signac/1.11.0/articles/data_structures)  
- [Linking peaks to genes (10x Multiome)](https://stuartlab.org/signac/articles/pbmc_multiomic)  
- [Calling Peaks with MACS2](https://stuartlab.org/signac/articles/peak_calling)  
- [Motif analysis with Signac](https://stuartlab.org/signac/articles/motif_vignette)  
- [Co-accessible networks with Cicero](https://stuartlab.org/signac/articles/cicero)

---

## Core Functions & References  

- **Signac**  
  - [`CallPeaks`](https://stuartlab.org/signac/reference/callpeaks) | [CRAN docs](https://search.r-project.org/CRAN/refmans/Signac/html/CallPeaks.html)  
  - [`FeatureMatrix`](https://stuartlab.org/signac/reference/featurematrix)  
  - [`RegionStats`](https://stuartlab.org/signac/reference/regionstats)  
  - [`LinkedPeaks`](https://stuartlab.org/signac/reference/linkpeaks)  

- **GenomicRanges**  
  - [`reduce`](https://bioconductor.org/packages/devel/bioc/vignettes/GenomicRanges/inst/doc/GenomicRangesIntroduction.html) | [IRanges methods](https://rdrr.io/bioc/IRanges/man/inter-range-methods.html)

- **Seurat v5+**  
  - [`AggregateExpression`](https://satijalab.org/seurat/reference/aggregateexpression)

---

## Custom Repo Practices  

- **Peak calling workflows:** [scMultiomics_AD repo](https://aanderson54.github.io/scMultiomics_AD/#peak-calling)  
- **Link detection strategies:**  
  - Filter MACS peak calls (e.g., by expression or cell type).  
  - Keep only genes with sufficient UMIs (e.g., ≥200).  
  - Restrict peak–gene links by distance (≤100 kb from TSS).  
  - Example: [link filtering](https://aanderson54.github.io/scMultiomics_AD/#links).  

---

## Behind-the-Scenes Tools & File Formats  

- **MACS Project:** [GitHub](https://github.com/macs3-project/MACS) | [Changelog](https://macs3-project.github.io/MACS/#changes-for-macs-3-0-3)  
- **Installation:** [MACS2 guide](https://github.com/macs3-project/MACS/wiki/Install-macs2/37ede4928925646fb41d25120a133900038c092e) | [MACS3 guide](https://macs3-project.github.io/MACS/docs/INSTALL.html)  
- **BED file format:** [UCSC FAQ](https://genome.ucsc.edu/FAQ/FAQformat#format1)  

---

## Outline  

- Chromatin Accessibility Downstream Analysis for High-Confidence Peaks  
- Background  
- Key Literature  
- Vignettes & Tutorials  
- Core Functions & References  
- Custom Repo Practices  
- Behind-the-Scenes Tools & File Formats  
