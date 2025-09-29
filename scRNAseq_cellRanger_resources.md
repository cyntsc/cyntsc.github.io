---
title: "Getting started"
parent: "1 Single-Cell RNA-seq"
nav_order: 1
permalink: /single_cell_RNA-seq/starting/
parent_permalink: /single_cell_RNA-seq/
---

# Starting with Single Cell RNA-seq
**Notebook:** 09-25-2025

*Cynthia S. Cardinault*

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?logo=linkedin&logoColor=white&style=for-the-badge)](https://www.linkedin.com/in/cynthiacardinault)
<!--[![GitHub](https://img.shields.io/badge/-GitHub-181717?logo=github&logoColor=white&style=for-the-badge)](https://github.com/cyntsc)-->
[![Email](https://img.shields.io/badge/-Email-D14836?logo=gmail&logoColor=white&style=for-the-badge)](mailto:bioinformatic2019@gmail.com)


<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

- [Starting with Single Cell RNA-seq](#starting-with-single-cell-rna-seq)
  - [But wait, What is single cell?](#but-wait-what-is-single-cell)
  - [Starting with Cell Ranger](#starting-with-cell-ranger)
  - [The 4 Core Steps of Cell Ranger](#the-4-core-steps-of-cell-ranger)
  - [References](#references)


<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

Before get into the technical, it’s worth highlighting that single-cell analysis isn’t limited to RNA. Single cell can be applied to DNA, proteins, and even metabolism. That said, my notes will focus on the **transcriptomic** side, which is probably the most widely adopted application of this technique until now. There is also a single-cell version of ATAC-seq, key for identifying cell states, differentiation trajectories, and the specific gene regulatory elements active in rare cell populations. 

Beyond that, the actual frontiers in single-cell analysis is expanding beyond the initial individual "omics" layers into two major, interconnected areas: **Multi-Omics** and **Spatial** context. These may or may not be performed within the same experimental protocol, and integrating such diverse datasets is an entire topic of its own. For now, let’s focus on **transcriptomics** itself.

<p align="center">
  <img src="{{ '/images/scRNAseq/Gemini_Generated_minion.png' | relative_url }}" alt="Sketch 1" style="max-width:90%; height:auto;">
</p>

As way of illustration, the diagram represents 4 key players. All it happens looking at inside the cell, *Genomics* is holding the "instruction manual" (the DNA double helix), showing what the cell could possibly do. While in *Transcriptomics* is acting as the "loudspeaker" broadcasting which instructions are currently being read out.
*Proteomics* is the muscular "workforce", representing the actual functional molecules that build and perform all the cell's tasks. *Metabolomics* is running around with all the stuff, keeping track of the "energy and supplies" (the small chemicals consumed and produced) to power the whole operation.

<!--
<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

[![LinkedIn post](https://img.shields.io/badge/See%20on-LinkedIn-blue?logo=linkedin)](https://www.linkedin.com/posts/...)

[![X post](https://img.shields.io/badge/See%20on-X-black?logo=twitter)](https://twitter.com/handle/status/1234567890123456789)

<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">
-->

---

## <span class="gradient-heading">But wait, What is single cell?</span>

When we think about biology, it’s easy to picture tissues, organs, or even whole organisms, but the real action happens at the level of individual cells. Each cell carries its own blueprint, reads out specific sets of instructions, and plays a unique role in the bigger picture. This is where single-cell analysis comes in. Instead of blending signals across millions of cells, this field lets us zoom in and explore each one on its own terms -- *genomics, transcriptomics, epigenomics, proteomics, metabolomics*. By teasing apart this cellular diversity, researchers can uncover hidden subpopulations, map cell states, and better understand how complex tissues function in health and disease.

And this is exactly why platforms like **10x Genomics Chromium** and its companion software, **Cell Ranger**, were created. They bridge the gap between raw sequencing data and meaningful biological insights. 

In the following video (the first of nine-part mini learning series) you can explore more about single cell techniques versus traditional bulk technologies, such as PCR, microarray, and bulk RNA-seq. 

<br>

<iframe width="560" height="315" src="https://www.youtube.com/embed/pKngHhBCnHU?si=hGZhGpafV0DWzInK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
The video is part of the official collection released by **10x Genomics**.

---

## <span class="gradient-heading">Starting with Cell Ranger</span>

If you’ve just generated your first single-cell dataset with the <strong style="color:#2563eb;">10x Genomics Chromium</strong> platform, chances are you’ll meet <strong style="color:#dc2626;">Cell Ranger</strong>. The software suite that transforms raw sequencing output into clean, interpretable data.

Think of Cell Ranger as your pipeline companion: it takes messy raw files and make the count matrices, quality metrics, and interactive reports.  From there, you can dive into <span style="color:#16a34a; font-weight:700;">clustering</span>, <span style="color:#9333ea; font-weight:700;">dimensionality reduction</span>, and <span style="color:#e11d48; font-weight:700;">downstream biological insights</span>.

---

## <span class="gradient-heading">The 4 Core Steps of Cell Ranger</span>

Here’s a bird’s-eye view of the core steps:

<p align="center">
<img src="{{ '/images/scRNAseq/cellranger_workflow.png' | relative_url }}" alt="Sketch 1" width="300">
</p>

<br> 

| Step                        | What Happens                                                                                 | What You Get                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **`cellranger mkfastq`** | Converts raw sequencing BCL files into FASTQ files and demultiplexes samples.                | FASTQ files                                                   |
| **`cellranger count`**   | Aligns reads, identifies barcodes and UMIs, filters noise, and builds the expression matrix. | Gene-barcode matrix, BAM file, summary report, `.cloupe` file |
| **`cellranger aggr`**    | Combines results from multiple runs into one dataset.                                        | Aggregated gene-barcode matrix                                |
| **`Secondary Analysis`**   | Calls real cells, reduces dimensions (PCA, UMAP/t-SNE), and clusters cells.                  | Filtered matrix + interactive Loupe Browser exploration       |

---

Hope you have a useful starting point. Dont't miss my next notes talking about **Modes to run Cell Ranger**.

*Cynthia SC*

---

## <span class="gradient-heading">References</span>

[Tour of the Single Cell Analysis](https://youtube.com/playlist?list=PLfaSRwcfHcq0y256vji33nI0FBgQxOIqa&si=QrcV59MQ79UANZ4G)


---

<div class="disclaimer" markdown="1">
Disclaimer: The characters in the image(s) are styled after the Minions, which are the copyrighted and trademarked property of Universal Studios / Illumination Entertainment. The image(s) was generated by a large language model based on a user-provided concept and is intended for illustrative and educational purposes only.
</div>