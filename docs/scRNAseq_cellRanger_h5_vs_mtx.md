---
title: "Formatos de archivos H5 y MTX"
parent: "1 Single-Cell RNA-seq"
nav_order: 4
permalink: /single_cell_RNA-seq/scRNAseq_h5_vs_mtx/
description: "Diferencias entre formatos H5 y MTX en single-cell RNA-seq"
---

Author: *Cynthia SC* (04-27-2026)

---

## <span class="gradient-heading">Formatos H5 vs MTX en single-cell RNA-seq</span>
{: .no_toc }
### Estructura, interpretación y uso práctico en Seurat v5
{: .gradient-heading .no_toc }

## Contenido
{: .no_toc }

1. TOC
{:toc}

---

## ¿Qué son los formatos H5 y MTX?
{: .gradient-heading .toc }

En el análisis de *single-cell RNA-seq (scRNA-seq)*, los datos no se generan directamente como matrices listas para análisis. A partir de archivos sin procesar - raw data (**FASTQ**), herramientas como `Cell Ranger` realizan el procesamiento inicial: alineamiento, cuantificación y filtrado de células. Como resultado de este flujo se obtienen matrices de expresión conocidas como *Filtered Feature-Barcode Matrix*, que se entregan generalmente en formatos: *H5* y *MTX*.

En ambos formatos se representan el mismo dato biológico, es decir, la relación entre genes y células, pero tienen estructuras distintas que impactan directamente la eficiencia del análisis y la forma en que interactuamos con los datos en herramientas como **Seurat v5**.


![Unicellular organisms](../images/scrnaseq_format_files.svg)


## ¿Por qué existen distintos formatos para los mismos datos?
{: .gradient-heading .toc }

Cuando trabajamos con datos de **single-cell RNA-seq (scRNA-seq)** generados por plataformas como *10x Genomics*, es común encontrarnos con dos formatos principales:

- **H5 (.h5)**
- **MTX (.mtx + .tsv)**

A primera vista, esto puede generar confusión:

**¿Son datos distintos? ¿Cambian los resultados? ¿Cuál debo usar?**

La respuesta corta es: **NO**, no cambia el contenido biológico, solo la forma en que están almacenados los datos.

### Primera idea clave: mismo dato, distinta estructura
{: .gradient-heading .no_toc }

Ambos formatos contienen exactamente la misma información:

- matriz de conteos (genes × células)  
- barcodes celulares (UMIs) 
- anotación de features (genes, picos, etc.)  

La diferencia está en cómo se organizan:

| Formato | Estructura |
|--------|-----------|
| H5 | Un solo archivo binario jerárquico |
| MTX | Tres archivos de texto separados |


### Segunda idea clave: eficiencia vs transparencia
{: .gradient-heading .no_toc }

### H5 (HDF5)

- Archivo único  
- Lectura rápida  
- Menor tamaño  
- Ideal para pipelines  

Pensado para *eficiencia computacional*

### MTX (Matrix Market)

- Compuesto de tres archivos:
  - `matrix.mtx`
  - `barcodes.tsv`
  - `features.tsv`  
- Formato legible  
- Fácil de inspeccionar  

Pensado para *transparencia y entendimiento*

En resumen:

- H5 optimiza el análisis
- MTX facilita el aprendizaje


## Aplicación directa en Seurat v5
{: .gradient-heading .toc }

### Cargar datos desde H5

```r
library(Seurat)
data <- Read10X_h5("filtered_feature_bc_matrix.h5")
seurat_obj <- CreateSeuratObject(counts = data)
seurat_obj
```

Salida: 
```r
An object of class Seurat 
38606 features across 5710 samples within 1 assay 
Active assay: RNA (38606 features, 0 variable features)
 1 layer present: counts
```

### Cargar datos desde MTX
```r
library(Seurat)
data <- Read10X(data.dir = "filtered_feature_bc_matrix/")
seurat_obj <- CreateSeuratObject(counts = data)
```
Salida: 
```r
An object of class Seurat 
38606 features across 5710 samples within 1 assay 
Active assay: RNA (38606 features, 0 variable features)
 1 layer present: counts
```

Como puedes observar los objetos de clase Seurat que se crean son idénticos, aunque con estructura diferente. Mientras **.h5** tiene la ventaja de ser un solo archivo de lectura más rápida, **.mtx** es como recibir un rompecabezas en tres bolsas con piezas, instrucciones, etiquetas y luego ensamblarlo.

### Estructura de archivos MTX
```plaintext
filtered_feature_bc_matrix/
├── matrix.mtx
├── barcodes.tsv
└── features.tsv
```

El formato no cambia la biología, pero sí la experiencia de análisis. Aunque ambos formatos representan lo mismo, la forma en que interactuamos con los datos cambia nuestra comprensión del dato, generalmente con `MTX` entendemos la estructura y con `H5` nos enfocamos en el análisis.


## Recursos de consulta
{: .gradient-heading .no_toc }

- [10x Genomics – Feature Barcode Matrices](https://www.10xgenomics.com/support/software/cell-ranger-arc/latest/analysis/outputs/feature-barcode-matrices)
- [Load in data from 10X](https://satijalab.org/seurat/reference/read10x)
- [Cell Ranger – Outputs Overview](https://www.10xgenomics.com/support/software/cell-ranger/latest/analysis/outputs/cr-outputs-overview)

---

[![Visit El Arkhe](https://img.shields.io/badge/Visit-El%20Arkhe-purple?style=for-the-badge)](https://el-arkhe.github.io)

![Visitors](https://visitor-badge.laobi.icu/badge?page_id=el-arkhe.el-arkhe)

<p style="text-align:center">
© 2026 El Arkhe MultiOmics · México
</p>

