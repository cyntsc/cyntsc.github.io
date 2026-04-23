---
title: "Unicellular organisms"
parent: "1 Single-Cell RNA-seq"
nav_order: 1
permalink: /single_cell_RNA-seq/unicellular_orgs/
description: "Unicellular organisms in single-cell RNA sequencing"
---

## <span class="gradient-heading">Unicellular organisms</span>

![Unicellular organisms](../images/unicellular_orgs.png)

# ¿Tiene sentido hacer *single-cell* en organismos unicelulares?
## Poblaciones, muestras, variabilidad y pseudorreplicación

Cuando pensamos en tecnologías de célula única, como  **single-cell RNA-seq (scRNA-seq)**, lo más intuitivo es asociarlas con organismos multicelulares: tejidos complejos, múltiples tipos celulares, interacciones entre células, etc.

Pero surge una duda muy válida:

#### **Si un organismo es unicelular, ¿no estamos haciendo simplemente RNA-seq “normal”?**

Por ejemplo, si secuenciamos 10,000 células de microalgas, ¿eso equivale a tener 10,000 muestras independientes?

Estas preguntas no solo son razonables, son clave para entender el verdadero valor del enfoque *single-cell*.



## La primera confusión: unicelular ≠ homogéneo

Tomemos como ejemplo a *Chlamydomonas reinhardtii*, una microalga ampliamente utilizada como modelo experimental.

Cada célula es, efectivamente, un organismo completo.  
Entonces parecería lógico pensar:

👉 *“RNA-seq bulk debería ser suficiente”*

Pero aquí está el punto fundamental:

#### **Aunque cada célula es un organismo, una población de células NO es homogénea**

Incluso en un mismo cultivo, bajo las mismas condiciones, las células pueden estar en:

- diferentes fases del ciclo celular  
- distintos estados metabólicos  
- respuestas variables al estrés (nutrientes, luz, etc.)

Esto significa que **el promedio (bulk RNA-seq) oculta información crítica**

#### Entonces, ¿qué mide realmente el single-cell en organismos unicelulares?

El scRNA-seq permite capturar:

- la **variabilidad entre individuos** (en unicelulares)  
- o la **variabilidad entre células** (en multicelulares)  

La diferencia no está en la tecnología, sino en la interpretación:

| Sistema | Qué representa cada célula |
|--------|---------------------------|
| Multicelular | un tipo celular dentro de un organismo |
| Unicelular | un individuo dentro de una población |


## La segunda confusión: ¿10,000 células = 10,000 muestras?

Aunque en organismos unicelulares:

- cada célula es un organismo ✔  

👉 **NO equivale a tener 10,000 muestras independientes**

¿Por qué?

Porque todas las células provienen de:

- el mismo cultivo  
- la misma condición experimental  


### Concepto clave

**Las células son unidades de observación, no unidades experimentales**

Esto aplica tanto para:

- humanos  
- tejidos  
- microalgas  


## 🚨 El riesgo: pseudorreplicación

Si tratamos cada célula como una réplica independiente para comparar condiciones, caemos en:

👉 **pseudorreplicación**

La forma correcta de analizar diferencias entre condiciones es:

- tener **múltiples cultivos independientes** (réplicas biológicas)  
- y luego analizar las células dentro de cada réplica  


## 🔁 ¿Y el pseudobulk? Otra duda común:

#### ¿Este tipo de experimentos son asimilables a pseudobulk?

La respuesta es **no** en el diseño experimental, pero **sí** puede construirse después del análisis.  

A partir de datos single-cell, podemos:

- agrupar células  
- sumar counts  

→ y generar un perfil tipo “bulk” (**pseudobulk**) para análisis estadístico robusto  


## 💡 La idea clave

> **El valor del single-cell no depende de la complejidad del organismo, sino de la heterogeneidad del sistema**


## 🌊 ¿Por qué esto importa?

En sistemas como microalgas:

- permite entender dinámicas celulares invisibles en bulk  
- revela estados biológicos relevantes para biotecnología  
- abre la puerta a estudiar sistemas más complejos (como ecosistemas marinos)  


## Reflexión

En multicelulares exploramos diversidad de tipos celulares.  
En unicelulares exploramos diversidad de estados celulares.

Ambos enfoques responden a la misma pregunta:

> **¿Qué tan heterogéneo es el sistema que estamos estudiando?**

---

## 🔗 Recursos de consulta

Puedes consultar estos artículos para profundizar en el tema: 

[Single-cell RNA sequencing of batch Chlamydomonas cultures reveals heterogeneity in their diurnal cycle phase](https://pmc.ncbi.nlm.nih.gov/articles/PMC8226295/)

---

CSC. February 2026

![Visitors](https://visitor-badge.laobi.icu/badge?page_id=el-arkhe.el-arkhe)

<p style="text-align:center">
© 2026 El Arkhe MultiOmics · México
</p>
