---
title: "Getting started with single-cell"
parent: "1 Single-Cell RNA-seq"
nav_order: 1
permalink: /single_cell_RNA-seq/starting/
parent_permalink: /single_cell_RNA-seq/
---

# Single Cell RNA-seq Analysis with the "Cell Ranger Pipeline"
**Notebook:** 09-25-2025

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?logo=linkedin&logoColor=white&style=for-the-badge)](https://www.linkedin.com/in/cynthiacardinault)
<!--[![GitHub](https://img.shields.io/badge/-GitHub-181717?logo=github&logoColor=white&style=for-the-badge)](https://github.com/cyntsc)-->
[![Email](https://img.shields.io/badge/-Email-D14836?logo=gmail&logoColor=white&style=for-the-badge)](mailto:bioinformatic2019@gmail.com)


<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

- [Single Cell RNA-seq Analysis with the "Cell Ranger Pipeline"](#single-cell-rna-seq-analysis-with-the-cell-ranger-pipeline)
  - [What is single cell?](#what-is-single-cell)
  - [Starting with 10x Genomics Cell Ranger](#starting-with-10x-genomics-cell-ranger)
  - [Core Steps of Cell Ranger](#core-steps-of-cell-ranger)
  - [Modes to Run Cell Ranger](#modes-to-run-cell-ranger)
  - [Wrapping Up](#wrapping-up)
  - [Benchmarking Run Modes](#benchmarking-run-modes)
  - [References](#references)


<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

Before get into my notes, it’s worth highlighting that single-cell analysis isn’t limited to RNA—it can also be applied to DNA, proteins, and even metabolism. That said, my notes will focus on the transcriptomic side, which is probably the most widely adopted application of this technique.

Beyond that, there’s also the concept of multi-omics, where two or more of these approaches are combined. These may or may not be performed within the same experimental protocol, and integrating such diverse datasets is an entire topic of its own. For now, let’s focus on transcriptomics itself.

<p align="center">
  <img src="{{ '/images/scRNAseq/Gemini_Generated_minion.png' | relative_url }}" alt="Sketch 1" style="max-width:90%; height:auto;">
</p>

<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

<!--
[![LinkedIn post](https://img.shields.io/badge/See%20on-LinkedIn-blue?logo=linkedin)](https://www.linkedin.com/posts/...)

[![X post](https://img.shields.io/badge/See%20on-X-black?logo=twitter)](https://twitter.com/handle/status/1234567890123456789)

<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">
-->

## <span class="gradient-heading">What is single cell?</span>

When we think about biology, it’s easy to picture tissues, organs, or even whole organisms, but the real action happens at the level of individual cells. Each cell carries its own blueprint, reads out specific sets of instructions, and plays a unique role in the bigger picture. This is where single-cell analysis comes in. Instead of blending signals across millions of cells, this field lets us zoom in and explore each one on its own terms—its DNA (genomics), RNA (transcriptomics), proteins (proteomics), or even its metabolism (metabolomics). By teasing apart this cellular diversity, researchers can uncover hidden subpopulations, map cell states, and better understand how complex tissues function in health and disease.

And this is exactly why platforms like 10x Genomics Chromium and its companion software, Cell Ranger, were created. They bridge the gap between raw sequencing data and meaningful biological insights. In other words, they take the overwhelming complexity of single-cell experiments and make it accessible, reproducible, and ready for discovery.

In the following video (the first of nine-part mini learning series) discover the advantages of single cell techniques versus traditional bulk technologies, such as PCR, microarray, and bulk RNA-seq. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/pKngHhBCnHU?si=hGZhGpafV0DWzInK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## <span class="gradient-heading">Starting with 10x Genomics Cell Ranger</span>

If you’ve just generated your first single-cell dataset with the <strong style="color:#2563eb;">10x Genomics Chromium</strong> platform, chances are you’ll meet <strong style="color:#dc2626;">Cell Ranger</strong> 🧬 — the software suite that transforms raw sequencing output into clean, interpretable data.

<div style="background-color:#fef9c3; padding:12px; border-radius:8px; font-size:17px; font-weight:600; margin:15px 0;">
Think of Cell Ranger as your <strong>pipeline companion</strong>: it takes messy raw files and make the count matrices, quality metrics, and interactive reports.  
From there, you can dive into <span style="color:#16a34a; font-weight:700;">clustering</span>, <span style="color:#9333ea; font-weight:700;">dimensionality reduction</span>, and <span style="color:#e11d48; font-weight:700;">downstream biological insights</span>.
</div>

---

## <span class="gradient-heading">Core Steps of Cell Ranger</span>

Here’s a bird’s-eye view of the core steps:

<p align="center">
<img src="{{ '/images/scRNAseq/cellranger_workflow.png' | relative_url }}" alt="Sketch 1" width="300">
</p>

| Step                        | What Happens                                                                                 | What You Get                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **1. `cellranger mkfastq`** | Converts raw sequencing BCL files into FASTQ files and demultiplexes samples.                | FASTQ files                                                   |
| **2. `cellranger count`**   | Aligns reads, identifies barcodes and UMIs, filters noise, and builds the expression matrix. | Gene-barcode matrix, BAM file, summary report, `.cloupe` file |
| **3. `cellranger aggr`**    | Combines results from multiple runs into one dataset.                                        | Aggregated gene-barcode matrix                                |
| **4. `Secondary Analysis`**   | Calls real cells, reduces dimensions (PCA, UMAP/t-SNE), and clusters cells.                  | Filtered matrix + interactive Loupe Browser exploration       |

---

## <span class="gradient-heading">Modes to Run Cell Ranger</span>

Depending on your setup, you have a few options:

<p align="center">
  <img src="{{ '/images/scRNAseq/CellRanger_runs_ChatGPT_Image.png' | relative_url }}" alt="Sketch 1" style="max-width:90%; height:auto;">
</p>

<!--
### <span class="heading-subsection2">Local or Institutional HPC</span>
-->
**Local or Institutional HPC**

Most researchers run Cell Ranger on an institutional **High-Performance Computing (HPC)** cluster or a dedicated Linux server. A typical run may need **64 GB RAM or more** plus multiple CPU cores—so if your lab has access to an HPC, that’s often the easiest path.

<!--
### <span class="heading-subsection2">Genomics Cloud Analysis</span>
-->
**Genomics Cloud Analysis**

Don’t have a big server? No problem. The **10x Genomics Cloud Analysis** platform is available almost everywhere (except China, Hong Kong, and embargoed countries). Good news Mexico and most of Latin America are included.

* **Free tier:** Up to 50 analyses per month—perfect for starting out.  
* **Extras:** You can transfer projects between accounts, and collaborative features are on the way.  
* **Caveat:** Paid upgrades are mostly limited to the U.S. and Canada.  

<!--
### <span class="heading-subsection2">General Cloud Platforms</span>
-->
**General Cloud Platforms**

If you prefer flexibility, you can spin up a **virtual machine** on AWS, Google Cloud, or another provider. Just pick a high-memory instance, install Cell Ranger, upload your FASTQs, and pay only for what you use. This is a great option if you need scalability on demand.

<!--
### <span class="heading-subsection2">Alternatives to Cell Ranger</span>
-->
**Alternatives to Cell Ranger**

While Cell Ranger is the gold standard, you might explore community tools too:

* **STARsolo** – efficient and highly comparable to Cell Ranger.  
* **kallisto/bustools (Alevin-fry)** – much faster and lighter on memory.  

---

## <span class="gradient-heading">Wrapping Up</span>

Cell Ranger may look intimidating at first, but once you run it a couple of times you’ll realize it’s a **solid, reliable backbone for single-cell data processing**. Whether you use an HPC, the free cloud tier, or your own server, it’s the first step in turning sequencing reads into biological insights.

My advice? Start with the simplest setup you have access to (often your institution’s HPC or the 10x Cloud free tier), then scale up once your projects get bigger.

## <span class="gradient-heading">Benchmarking Run Modes</span>

Curious how they stack up? 

<div class="ref-box" markdown="1">
  <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC8848315/#:~:text=While%20STARsolo%2C%20Cell%20Ranger%206,which%20are%20likely%20mapping%20artefacts" target="_blank">
    <img src="{{ '/images/scRNAseq/benchmarking.png' | relative_url }}" alt="PMC">
  </a>
  <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC8848315/#:~:text=While%20STARsolo%2C%20Cell%20Ranger%206,which%20are%20likely%20mapping%20artefacts">
  </a>
</div>

---

Hope you have a useful starting point. Dont't miss my next notes talking about *How to prepare your data?* and *Run Cell Ranger* 

Feel free to share thoughts, suggestions, or resources here or in my social networks . Collaboration makes the whole community stronger.  

*Cynthia SC*

---

## <span class="gradient-heading">References</span>

[Tour of the Single Cell Analysis](https://youtube.com/playlist?list=PLfaSRwcfHcq0y256vji33nI0FBgQxOIqa&si=QrcV59MQ79UANZ4G)

[Installation tutorial](https://www.10xgenomics.com/support/software/cell-ranger/latest/tutorials/cr-tutorial-in)

[Download Cell Ranger](https://www.10xgenomics.com/support/software/cell-ranger/downloads#download-links)

[Cloud availability FAQ](https://www.10xgenomics.com/support/software/cloud-analysis/latest/faqs/CA-frequently-asked-questions#regional-availability)

[System Requirements](https://www.10xgenomics.com/support/software/cell-ranger/downloads/cr-system-requirements)

[getting started guide](https://www.10xgenomics.com/support/software/cell-ranger/latest/getting-started).


Disclaimer: The characters in this image are styled after the Minions, which are the copyrighted and trademarked property of Universal Studios / Illumination Entertainment. This image was generated by a large language model based on a user-provided concept and is intended for illustrative and educational purposes only.