# Project-of-Julia-Klompmaker-Reproducing-Dataset-Diversity-Findings
## Purpose
This notebook documents the exploratory analyses that informed the adaptation of the Dataset Diversity Evaluation framework to the Harvard-GF ophthalmology dataset.

The overall goal of the project is to investigate whether previously reported relationships between dataset diversity metrics and downstream classification performance can be reproduced on a different medical imaging dataset.

## Reproducing previous work
This project reproduces the methodology of Sourget et al. on the Harvard-GF dataset.

The original work evaluated the relationship between:

    image diversity metrics,
    metadata diversity metrics,
    downstream classification performance.

The original experiments were conducted on MorphoMNIST and PadChest. Here the same framework is applied to Harvard-GF.

## My contributions
The following work was carried out as part of this project:

* Exploration and analysis of the Harvard-GF dataset.
* Investigation of RNFLT value distributions and dataset-specific characteristics.
* Analysis of the meaning of special values (-2, -1 and 0).
* Investigation of different normalisation strategies.
* Development of Harvard-GF preprocessing code.
* Creation of a Harvard-GF PyTorch dataset compatible with the original framework.
* Integration of Harvard-GF into the diversity metric pipeline.
* Adaptation of the training and prediction pipelines.
* Evaluation of diversity metrics and downstream classification performance.
* Analysis of the relationship between diversity metrics and AUC.

## Original implementation
The following components originate from the implementation released by Sourget et al.:

* Diversity metric implementations (Inception Score, FID, Vendi Score variants, metadata similarity).
* Original MorphoMNIST and PadChest processing pipelines.
* Original model training framework.
* Original evaluation methodology and ranking-based comparison approach.
* The code was adapted where necessary to support Harvard-GF.
