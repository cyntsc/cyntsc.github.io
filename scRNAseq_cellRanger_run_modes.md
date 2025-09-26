---
title: "Cell Ranger Run Modes"
parent: "3 scRNA-seq analysis"
nav_order: 1
permalink: ingle_cell_RNA-seq/run_modes/
parent_permalink: /single_cell_RNA-seq/
---

# Single Cell RNA-seq Analysis
## Cell Ranger Run Modes
**Notebook:** 09-25-2025

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?logo=linkedin&logoColor=white&style=for-the-badge)](https://www.linkedin.com/in/cynthiacardinault)
<!--[![GitHub](https://img.shields.io/badge/-GitHub-181717?logo=github&logoColor=white&style=for-the-badge)](https://github.com/cyntsc)-->
[![Email](https://img.shields.io/badge/-Email-D14836?logo=gmail&logoColor=white&style=for-the-badge)](mailto:bioinformatic2019@gmail.com)

<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

<div style="background-color:#fff9db; padding:12px; border-radius:6px; border-left:5px solid #facc15;" markdown="1">
<strong>📌 Note:</strong> For a better understanding of the design of this notebook, please read the
<a href="{{ page.parent_permalink | relative_url }}" style="font-weight:bold; color:#2563eb;">main section page</a> first. Thanks 🙏
</div>


- [Single Cell RNA-seq Analysis](#single-cell-rna-seq-analysis)
  - [Cell Ranger Run Modes](#cell-ranger-run-modes)
  - [How Can You Run It?](#how-can-you-run-it)
    - [🔹 Local or Institutional HPC](#-local-or-institutional-hpc)
    - [10x Genomics Cloud Analysis](#10x-genomics-cloud-analysis)
    - [General Cloud Platforms](#general-cloud-platforms)
    - [Alternatives to Cell Ranger](#alternatives-to-cell-ranger)
  - [Wrapping Up](#wrapping-up)


<hr style="border: none; height: 4px; background-color: #444; margin: 30px 0;">

## How Can You Run It?

Depending on your setup, you have a few options:

### 🔹 Local or Institutional HPC

Most researchers run Cell Ranger on an institutional **High-Performance Computing (HPC)** cluster or a dedicated Linux server. A typical run may need **64 GB RAM or more** plus multiple CPU cores—so if your lab has access to an HPC, that’s often the easiest path.

[Installation tutorial](https://www.10xgenomics.com/support/software/cell-ranger/latest/tutorials/cr-tutorial-in)

---

### 10x Genomics Cloud Analysis

Don’t have a big server? No problem. The **10x Genomics Cloud Analysis** platform is available almost everywhere (except China, Hong Kong, and embargoed countries). Good news: Mexico and most of Latin America are included.

* **Free tier:** Up to 50 analyses per month—perfect for starting out.  
* **Extras:** You can transfer projects between accounts, and collaborative features are on the way.  
* **Caveat:** Paid upgrades are mostly limited to the U.S. and Canada.  

👉 [Cloud availability FAQ](https://www.10xgenomics.com/support/software/cloud-analysis/latest/faqs/CA-frequently-asked-questions#regional-availability)

---

### General Cloud Platforms

If you prefer flexibility, you can spin up a **virtual machine** on AWS, Google Cloud, or another provider. Just pick a high-memory instance, install Cell Ranger, upload your FASTQs, and pay only for what you use. This is a great option if you need scalability on demand.

---

### Alternatives to Cell Ranger

While Cell Ranger is the gold standard, you might explore community tools too:

* **STARsolo** – efficient and highly comparable to Cell Ranger.  
* **kallisto/bustools (Alevin-fry)** – much faster and lighter on memory.  

👉 Curious how they stack up? Here’s a [benchmarking comparison](https://pmc.ncbi.nlm.nih.gov/articles/PMC8848315/#:~:text=While%20STARsolo%2C%20Cell%20Ranger%206,which%20are%20likely%20mapping%20artefacts).

---

## Wrapping Up

Cell Ranger may look intimidating at first, but once you run it a couple of times you’ll realize it’s a **solid, reliable backbone for single-cell data processing**. Whether you use an HPC, the free cloud tier, or your own server, it’s the first step in turning sequencing reads into biological insights.

✨ My advice? Start with the simplest setup you have access to (often your institution’s HPC or the 10x Cloud free tier), then scale up once your projects get bigger.

---

Thanks for reading through these notes.
I hope they give you a useful starting point or spark new ideas for your own analysis.  

Feel free to share thoughts, suggestions, or resources. Collaboration makes the whole community stronger.  

Until next time, happy exploring!

---

*Cynthia SC*

