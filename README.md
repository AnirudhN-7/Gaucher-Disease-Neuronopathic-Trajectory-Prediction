# Gaucher Disease Neuronopathic Trajectory Prediction

> **A reproducible machine learning framework for predicting the
> neuronopathic trajectory of GBA1 missense variants by integrating
> computational annotations, literature-curated clinical evidence and
> supervised learning.**

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Platform](https://img.shields.io/badge/Platform-Google%20Colab%20%7C%20Jupyter-orange.svg)

------------------------------------------------------------------------

## Overview

Gaucher Disease is a rare lysosomal storage disorder caused by
pathogenic variants in the **GBA1** gene. Although hundreds of variants
have been reported, many remain classified as variants of uncertain
significance because experimental characterization is expensive and
limited.

This repository presents a complete computational workflow that:

-   Annotates GBA1 missense variants using multiple genomic resources.
-   Integrates structural, evolutionary, population and functional
    evidence.
-   Incorporates manual clinical curation from published literature.
-   Trains and evaluates multiple machine learning classifiers.
-   Predicts whether previously unclassified variants are likely to
    follow a **Neuronopathic** or **Non-Neuronopathic** disease
    trajectory.

The repository is designed to be **reproducible**, **modular**, and easy
to extend for future GBA1 studies.

------------------------------------------------------------------------

# Repository Structure

``` text
GBA1-Variant-Trajectory-Prediction/
│
├── preprocessing/
│   ├── get_alphamissense_parquet.ipynb
│   └── get_dbnsfp_parquet.ipynb
│
├── notebooks/
│   ├── Feature_Extraction_Pipeline.ipynb
│   └── Neuronopathy_Classification_Model.ipynb
│
├── data/
│   ├── input/
│   │   └── Final_Dataset.csv
│   ├── intermediate/
│   │   └── GBA1_Annotated_Final.csv
│   ├── processed/
│   │   └── GBA1_Annotated_Final_With_Types.csv
│   └── output/
│       └── GBA1_Trajectory_Predictions_V3.csv
│
├── requirements.txt
├── LICENSE
├── README.md
└── .gitignore
```

------------------------------------------------------------------------

# Pipeline Overview

``` text
Final_Dataset.csv
        │
        ▼
Google Colab Preprocessing
(AlphaMissense + dbNSFP)
        │
        ▼
Apache Parquet Files
        │
        ▼
Feature_Extraction_Pipeline.ipynb
        │
        ▼
GBA1_Annotated_Final.csv
        │
        ▼
Manual Literature Survey
(GD1, GD2, GD3 & Residual Activity)
        │
        ▼
GBA1_Annotated_Final_With_Types.csv
        │
        ▼
Neuronopathy_Classification_Model.ipynb
        │
        ▼
GBA1_Trajectory_Predictions_V3.csv
```

------------------------------------------------------------------------

# Pipeline Stages

## 1. Input Dataset

`Final_Dataset.csv` contains the curated collection of GBA1 missense
variants that serves as the starting point of the workflow.

## 2. Dataset Preprocessing (Google Colab)

The AlphaMissense and dbNSFP datasets are several gigabytes in size and
are **not distributed** with this repository.

The preprocessing notebooks are intended to be executed **only in Google
Colab**, where Google Drive is mounted to access these datasets
efficiently.

The notebooks:

-   convert the downloaded datasets into Apache Parquet format;
-   prepare them for fast querying during feature extraction.

Generated files:

-   `AlphaMissense.parquet`
-   `chr1.parquet`

> **Why only Chromosome 1?**\
> GBA1 is located on Chromosome 1; therefore only the chromosome 1
> annotations from dbNSFP are required.

## 3. Feature Extraction

`Feature_Extraction_Pipeline.ipynb` can be executed in **Google Colab**
or **locally (VS Code/Jupyter)** after updating dataset paths.

The notebook integrates annotations from:

  Resource        Purpose
  --------------- -----------------------------------
  AlphaMissense   Missense pathogenicity prediction
  dbNSFP          Functional annotations
  gnomAD          Population allele frequency
  GERP++          Evolutionary conservation
  InterVar        Clinical interpretation
  MutPred2        Functional impact
  DynaMut2        Structural stability

Output:

`GBA1_Annotated_Final.csv`

## 4. Manual Clinical Curation

The computationally annotated dataset is manually curated using
published literature.

The following fields are added wherever available:

-   GD1
-   GD2
-   GD3
-   Residual enzyme activity

Using these annotations, every labelled variant is assigned to either:

-   Neuronopathic
-   Non-Neuronopathic

Output:

`GBA1_Annotated_Final_With_Types.csv`

## 5. Machine Learning

`Neuronopathy_Classification_Model.ipynb` may be executed either in
**Google Colab** or **locally**.

Workflow includes:

-   Missing value imputation
-   Feature scaling
-   SMOTE
-   Cross-validation
-   Model benchmarking
-   SHAP explainability
-   Feature importance analysis

Models evaluated include Logistic Regression, SVM, Decision Tree, Random
Forest, Gradient Boosting, AdaBoost, HistGradientBoosting, MLP,
LightGBM, XGBoost and CatBoost.

Final output:

`GBA1_Trajectory_Predictions_V3.csv`

------------------------------------------------------------------------

# Quick Start

1.  Clone this repository.
2.  Download the external datasets.
3.  Run the preprocessing notebooks in **Google Colab**.
4.  Execute `Feature_Extraction_Pipeline.ipynb`.
5.  Perform manual literature curation.
6.  Execute `Neuronopathy_Classification_Model.ipynb`.

------------------------------------------------------------------------

# External Datasets

## AlphaMissense

Required file:

`AlphaMissense_isoforms_hg38.tsv`

Cloud to download the dataset:

https://console.cloud.google.com/storage/browser/dm_alphamissense;tab=objects?prefix=&forceOnObjectsSortingFiltering=false

## dbNSFP

Required file:

`dbNSFP5.3.1a_variant.chr1.gz`

Official website:

https://sites.google.com/dbnsfp.org/home/download

These datasets are intentionally excluded from this repository because
of their size and licensing.

------------------------------------------------------------------------

# Running the Repository

## Google Colab

Recommended for all users.

-   Run both preprocessing notebooks.
-   Mount Google Drive.
-   Generate the Parquet datasets.
-   Continue with the Feature Extraction and Machine Learning notebooks.

## Local (VS Code / Jupyter)

Supported after the Parquet files have already been generated.

Simply update the dataset paths inside the notebooks to point to your
local copies.

------------------------------------------------------------------------

# Installation

``` bash
pip install -r requirements.txt
```

------------------------------------------------------------------------

# Outputs

  --------------------------------------------------------------------------------------
  File                                  Description
  ------------------------------------- ------------------------------------------------
  Final_Dataset.csv                     Initial variant dataset

  GBA1_Annotated_Final.csv              Computational annotations

  GBA1_Annotated_Final_With_Types.csv   Literature-curated labelled dataset

  GBA1_Trajectory_Predictions_V3.csv    Final predictions
  --------------------------------------------------------------------------------------

------------------------------------------------------------------------

# Reproducibility

The preprocessing notebooks are designed specifically for Google Colab
because they process large external datasets stored on Google Drive.

After the Parquet files have been generated, the remaining workflow can
be executed either in Google Colab or in a local Python environment.

------------------------------------------------------------------------

# Citation

If this repository contributes to your research, please consider citing
it once a formal publication or DOI is available.

------------------------------------------------------------------------

# Acknowledgements

This work makes use of publicly available resources including
AlphaMissense, dbNSFP, gnomAD, GERP++, InterVar, SpliceAI, MutPred2,
DynaMut2, GTEx, STRING and the GWAS Catalog. Their developers and
maintainers are gratefully acknowledged.

------------------------------------------------------------------------

# License

Released under the MIT License. See `LICENSE` for details.
