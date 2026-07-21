# Gaucher-Disease-Neuronopathic-Trajectory-Prediction

A machine learning framework for predicting the **neuronopathic trajectory** of **GBA1 missense variants** by integrating functional, structural, evolutionary, population, and clinical annotations from multiple genomic resources.

The objective of this project is to assist in the interpretation of novel **GBA1** variants by leveraging publicly available annotation tools and supervised machine learning models to estimate the likelihood of a variant exhibiting a **Neuronopathic** or **Non-Neuronopathic** phenotype.

---

## Overview

Mutations in the **GBA1** gene are responsible for **Gaucher Disease**, a rare lysosomal storage disorder with highly variable clinical manifestations. While several pathogenic variants have been identified, many newly discovered variants remain difficult to classify because experimental validation is expensive and time-consuming.

This project presents an end-to-end computational pipeline that:

* Annotates GBA1 missense variants using multiple genomic resources.
* Extracts biologically relevant predictive features.
* Trains several supervised machine learning models.
* Predicts whether a variant is likely to be **Neuronopathic** or **Non-Neuronopathic**.

The repository is intended as a reproducible research implementation and serves as a reference workflow for computational variant interpretation.

---

## Repository Structure

```
GBA1-Variant-Trajectory-Prediction/
│
├── notebooks/
│   ├── Feature_Extraction_Pipeline.ipynb
│   └── Neuronopathy_Classification_Model.ipynb
│
├── scripts/
│   ├── convert_alphamissense_to_parquet.py
│   └── convert_dbnsfp_to_parquet.py
│
├── data/
│   ├── input/
│   │   └── Final_Dataset.csv
|   |
│   ├── intermediate/
│   |   └── GBA1_Annotated_Final.csv
|   |
│   ├── processed/
│   │   └── GBA1_Annotated_Final_With_Types.csv
│   │
│   └── output/
│       └── GBA1_Trajectory_Predictions.csv
│
├── README.md
├── requirements.txt
├── LICENSE
└── .gitignore
```

---

# Workflow

The complete workflow consists of four stages.

```
Initial Variant Dataset
          │
          ▼
Feature Extraction Pipeline
          │
          ▼
Annotated Dataset
          │
          ▼
Machine Learning Pipeline
          │
          ▼
Trajectory Predictions
```

---

## Stage 1 — Input Dataset

The pipeline begins with

```
Final_Dataset.csv
```

which contains manually curated **GBA1 missense variants** collected for this study.

Each record contains the genomic coordinates and mutation information required for downstream annotation.

---

## Stage 2 — Feature Extraction

The notebook

```
Feature_Extraction_Pipeline.ipynb
```

automatically annotates every GBA1 missense variant using multiple publicly available genomic resources.

The extracted features include information from:

* AlphaMissense
* dbNSFP
* gnomAD
* InterVar
* MutPred2
* DynaMut2
  
These annotations collectively describe:

* evolutionary conservation
* pathogenicity predictions
* structural stability
* protein function
* allele frequencies
* clinical evidence
* functional impact

The final output is

```
GBA1_Annotated_Final.csv
```
This dataset contains the complete set of computationally generated annotations for every variant and serves as the foundation for downstream analysis.

---

## Stage 3 — Manual Clinical Annotation

The computationally annotated dataset is then manually curated through an extensive literature survey.

For every variant with available experimental or clinical evidence, the following information is added:

Gaucher Disease Type 1 (GD1)
Gaucher Disease Type 2 (GD2)
Gaucher Disease Type 3 (GD3)
Residual enzyme activity

Using these manually curated annotations, each variant is assigned a final phenotype label (Neuronopathic or Non-Neuronopathic) that is used for supervised machine learning.

The resulting labelled dataset is:

```
GBA1_Annotated_Final_With_Types.c
```

---

## Stage 4 — Machine Learning

The notebook

```
Neuronopathy_Classification_Model.ipynb
```

performs:

* data preprocessing
* missing value handling
* feature scaling
* class balancing using SMOTE
* cross-validation
* model comparison
* feature importance analysis
* SHAP explainability

The following classifiers were evaluated:

* Logistic Regression
* Support Vector Machine
* Decision Tree
* Random Forest
* Gradient Boosting
* AdaBoost
* HistGradientBoosting
* Multi-layer Perceptron
* LightGBM
* XGBoost
* CatBoost

The best-performing model is then used to predict the clinical trajectory of previously unclassified variants.

---

## Stage 5 — Predictions

The final predictions are stored in

```
GBA1_Trajectory_Predictions.csv
```

which contains:

* predicted phenotype
* prediction probability
* supporting annotations

---

# External Datasets

This repository relies on two large public genomic datasets that are **not distributed** with this repository because of their size and licensing.

## 1. AlphaMissense

Download the **AlphaMissense isoforms (hg38)** dataset from the official DeepMind AlphaMissense release.

Required file:

```
AlphaMissense_isoforms_hg38.tsv
```

Official repository:

https://github.com/google-deepmind/alphamissense

---

## 2. dbNSFP

Download **dbNSFP v5.3.1a** from the official dbNSFP website.

Required file:

```
dbNSFP5.3.1a_variant.chr1.gz
```

Official website:

https://sites.google.com/site/jpopgen/dbNSFP

> **Note:** Only **Chromosome 1** is required because the **GBA1** gene is located on Chromosome 1.

---

# Preparing the External Datasets

The raw datasets are converted into **Apache Parquet** format to significantly improve lookup performance.

Run the following scripts:

```bash
python scripts/convert_alphamissense_to_parquet.py
python scripts/convert_dbnsfp_to_parquet.py
```

The scripts generate

```
AlphaMissense.parquet
```

and

```
chr1.parquet
```

These files are used by the feature extraction notebook.

---

# Running the Project

## Option 1 — Google Colab (Recommended)

The project was originally developed and tested using **Google Colab**.

1. Clone or upload the repository to your Google Drive.
2. Download the required external datasets.
3. Convert them to Parquet using the provided scripts.
4. Store the generated Parquet files in your Google Drive.
5. Mount Google Drive inside the notebooks.
6. Update the dataset paths if necessary.
7. Execute the notebooks sequentially.

---

## Option 2 — Local Execution (VS Code / Jupyter)

The notebooks can also be executed locally.

1. Clone the repository.

```bash
git clone https://github.com/<your-username>/GBA1-Variant-Trajectory-Prediction.git
```

2. Install the required packages.

```bash
pip install -r requirements.txt
```

3. Download the external datasets.

4. Convert them to Apache Parquet.

5. Replace the Google Drive paths in the notebooks with the locations of the generated Parquet files on your local machine.

No other changes to the pipeline are required.

---

# Why Apache Parquet?

The original AlphaMissense and dbNSFP datasets contain millions of records.

Repeated searches on TSV or compressed text files are computationally expensive.

Apache Parquet provides:

* substantially faster lookups
* efficient columnar storage
* lower memory usage
* improved scalability for large genomic datasets

The conversion is performed only once before running the annotation pipeline.

---

# Python Dependencies

All required Python packages are listed in

```
requirements.txt
```

Install them using

```bash
pip install -r requirements.txt
```

---

# Outputs

The repository produces three primary outputs.

| File                                  | Description                                |
| ------------------------------------- | ------------------------------------------ |
| `Final_Dataset.csv`                   | Initial curated GBA1 variant dataset       |
| `GBA1_Annotated_Final.csv`            | Annotated dataset after feature extraction |
| `GBA1_Annotated_Final_With_Types.csv` | Dataset containing GD type classification  |
| `GBA1_Trajectory_Predictions.csv`  | Final model predictions                    |

---

# Reproducibility

This repository is intended to provide a reproducible implementation of the complete workflow described in this project.

Because external datasets are maintained independently and are continuously updated, users should download them directly from their respective official sources before running the pipeline.

The notebooks have been developed and validated using **Google Colab**, but can also be executed locally after updating the dataset paths.

---

# Acknowledgements

This work makes use of several publicly available resources, including:

* AlphaMissense
* dbNSFP
* gnomAD
* InterVar
* MutPred2
* DynaMut2


We gratefully acknowledge the developers and maintainers of these resources for making them publicly accessible to the research community.

---

# License

This project is released under the MIT License.

See the `LICENSE` file for details.
