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

En el análisis  **scRNA-seq** los datos no se generan directamente como matrices listas para análisis. A partir de archivos **FASTQ** con herramientas como `Cell Ranger`, se realiza el procesamiento inicial: alineamiento, cuantificación y filtrado de células.

Este proceso generará matrices de conteos génicos (features) y códigos de barras (barcodes) sin filtrar (*Unfiltered feature-barcode matrix*) y filtrados (*Filtered Feature-Barcode Matrix*) en dos formatos de archivo, el formato **MEX** y el **HDF5**, que describimos a continuación.

![Unicellular organisms](../images/scrnaseq_format_files.svg)


## ¿Por qué existen distintos formatos para los mismos datos?
{: .gradient-heading .toc }

A primera vista, esto puede generar confusión: **¿Son datos distintos? ¿Cambian los resultados? ¿Cuál debo usar?**

La respuesta corta es: **NO** cambia el contenido biológico, solo la forma en que están almacenados los datos.

En ambos formatos se representan el mismo contenido biológico, pero tienen estructuras distintas que impactan directamente la eficiencia del análisis y la forma en que interactuamos con los datos.

### Primera idea clave: mismo dato, distinta estructura
{: .gradient-heading .no_toc }

Ambos formatos contienen exactamente la misma información:

- matriz de conteos (genes × células)  
- barcodes celulares (UMIs) 
- anotación de features (genes, picos, etc.)  

La diferencia está en cómo se organiza la información:

| Formato | Estructura |
|--------|-----------|
| H5 | Un solo archivo binario jerárquico que puede comprimir y acceder a los datos de forma mucho más eficiente que los formatos de texto como MEX. |
| MTX | Tres archivos de texto separados |

### Estructura de los formatos
{: .gradient-heading .toc }

- Formato **H5 (.h5)**: formato de datos jerárquicos (Hierarchical Data Format)

  El nivel superior del archivo contiene un único grupo HDF5, denominado matriz, y metadatos almacenados como atributos HDF5. Dentro del grupo matriz se encuentran conjuntos de datos que contienen las dimensiones de la matriz, las entradas, características y los códigos de barras de las células asociadas a las filas y columnas de la matriz, respectivamente.
  ```plaintext
  (root)
    └── matrix [HDF5 group]
        ├── barcodes
        ├── data
        ├── indices
        ├── indptr
        ├── shape
        └── features [HDF5 group]
            ├─ _all_tag_keys
            ├─ target_sets [for Flex]
            │   └─ [target set name]
            ├─ feature_type
            ├─ genome
            ├─ id
            ├─ name
            ├─ pattern [Feature Barcode only]
            ├─ read [Feature Barcode only]
            └─ sequence [Feature Barcode only]
  ```
  La matriz se almacenan en formato de columna dispersa comprimida (CSC).

- Formato **MTX (.mtx + .tsv)**: formato de texto MEX (Market Exchange Format)

  La matriz se almacena en el formato MEX (Market Exchange Format). Contiene archivos TSV comprimidos con gzip, con secuencias de características y códigos de barras que corresponden a los índices de fila y columna, respectivamente. Por ejemplo, el directorio con los archivos de salida del `cellranger` tiene el siguiente aspecto:
  ```plaintext
  cd /cyntsc/runs/samplePBMC/outs
  tree filtered_feature_bc_matrix
  filtered_feature_bc_matrix
      ├── barcodes.tsv.gz
      ├── features.tsv.gz
      └── matrix.mtx.gz
  0 directories, 3 files
  ```
  En *features.tsv.gz* se tiene a los índices de fila. Para cada característica, el ID y el nombre se almacenan en la primera y segunda columna del archivo, respectivamente. La tercera columna identifica el tipo de característica, que será Expresión genética, Captura de anticuerpos, Captura de guía CRISPR, Captura multiplexada o PERSONALIZADA, según el tipo de característica. Por ejemplo:
  ```plaintext
  main ❯ gzip -cd features.tsv.gz | head
  ENSG00000290825	DDX11L2	Gene Expression
  ENSG00000243485	MIR1302-2HG	Gene Expression
  ENSG00000237613	FAM138A	Gene Expression
  ```
  Los códigos de las células estan en *barcodes.tsv.gz*. Por ejemplo:
  ```plaintext
  ❯ gzip -cd barcodes.tsv.gz | head
  AAACCAAAGGTGACGA-1
  AAACCCTGTGACGAGT-1
  AAACGAATCAGGCTAC-1
  ````
  En *matrix.mtx.gz* esta la matriz de expresión génica en formato disperso (sparse) basado en coordenadas.
  La idea central es guardar únicamente los valores distintos de cero, ya que en scRNA-seq la mayoría de genes NO se detectan en la mayoría de células. Por ejemplo:
  ```plaintext
  ❯ gzip -cd matrix.mtx.gz | head                       scrnaseq-seurat5
  %%MatrixMarket matrix coordinate integer general
  %metadata_json: {"software_version": "cellranger-9.0.0", "format_version": 2}
  38606 5710 19262779
  9 1 1
  19 1 3
  22 1 1
  ```
  En la primera fila estan los metadatos agregados por `cellranger`, la siguiente fila contiene las dimensiones de la matriz (38606 X 5710), pero solo almacenó 19262779 entradas NO vacías. Por lo tanto, las siguientes filas representan:
  ```plaintext
  fila columna valor 
  ```
  Análogo a: 
  ```plaintext
  gen célula conteo
  ```

### Segunda idea clave: eficiencia vs transparencia
{: .gradient-heading .no_toc }

Ahora que ya conoces las estructuras de ambos formatos, podemos concluir de la siguiente manera.

### H5 (HDF5)

- Archivo único  
- Lectura rápida  
- Menor tamaño  
- Ideal para pipelines  

Pensado para *eficiencia computacional*

### MTX (Matrix Market)

- Compuesto de tres archivos
- Formato legible  
- Fácil de inspeccionar  

Pensado para *transparencia y entendimiento*

Más brevemente:

- H5 optimiza el análisis
- MTX facilita el aprendizaje


## Aplicación directa en Seurat v5
{: .gradient-heading .toc }

¿Y como se ve esto en un flujo de análisis single-cell? Veamos

### Cargar datos desde H5: un solo archivo contiene todo lo necesario

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

### Cargar datos desde MTX: es un directorio con 3 archivos
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

Como puedes observar los objetos de clase *Seurat* que se crean son idénticos, aunque la estructura de datos de entrada es diferente. Mientras **.h5** tiene la ventaja de ser un solo archivo de lectura más rápida, **.mtx** es un directorio de archivos, como recibir un rompecabezas con tres bolsas, conteniendo piezas, instrucciones y etiquetas, para luego ensamblarlas.

El formato no cambia la biología, pero sí la experiencia de análisis. Aunque ambos formatos representan lo mismo, la forma en que interactuamos con los datos cambia nuestra comprensión del dato, generalmente con `MTX` entendemos la estructura y con `H5` nos enfocamos en el análisis.

>OJO. Ni el formato **HDF5** ni el formato **MEX/MTX** son exclusivos de 10x Genomics/Cell Ranger. Ambos son formatos relativamente estándar utilizados en bioinformática y análisis de datos dispersos (sparse matrices). 

Algo que sí puede ser específico de `Cell Ranger`, es la estructura del directorio  **filtered_feature_bc_matrix** organizado en 3 archivos, una convención específica de la organización, popularizada por 10x y hoy casi un “estándar de facto” en single-cell.

**MEX = Matrix Exchange format**, es un formato estándar definido por NIST Matrix Market para almacenar matrices dispersas, que es ampliamente  utilizado por muchísimas herramientas (Scanpy, STARsolo, kallisto, Alevin, SciPy, Bioconductor).

Y, **HDF5 = Hierarchical Data Format version 5**, es un estándar científico general, muy utilizado en astronomía, física, machine learning, imágenes, y ahora también en genómica y single-cell. Herramientas y ecosistemas enormes lo usan (AnnData, loompy, Seurat, TensorFlow, PyTorch)

Una diferencia clave en HDF5, es qué su uso no es universal, ya que es como un contenedor, donde  cada herramienta decide como organiza los grupos, datasets, nombres internos y metadatos. Por eso **.mtx** es casi un formato universal, mientras **.h5** depende de la estructura interna. 

**Eso explica por qué hoy existe tanta interoperabilidad entre herramientas single-cell.**

*Cynthia SC*

## Recursos de consulta
{: .gradient-heading .no_toc }

- [Feature-Barcode Matrices from Cell Ranger Pipelines](https://www.10xgenomics.com/support/software/cell-ranger/latest/analysis/outputs/cr-outputs-matrices)
- [Feature Barcode Matrices](https://www.10xgenomics.com/support/software/cell-ranger-arc/latest/analysis/outputs/feature-barcode-matrices)
- [Load in data from 10X](https://satijalab.org/seurat/reference/read10x)
- [Cell Ranger – Outputs Overview](https://www.10xgenomics.com/support/software/cell-ranger/latest/analysis/outputs/cr-outputs-overview)

---

[![Visit El Arkhe](https://img.shields.io/badge/Visit-El%20Arkhe-purple?style=for-the-badge)](https://el-arkhe.github.io)

![Visitors](https://visitor-badge.laobi.icu/badge?page_id=el-arkhe.el-arkhe)

<p style="text-align:center">
© 2026 El Arkhe MultiOmics · México
</p>

