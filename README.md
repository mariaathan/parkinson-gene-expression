# Parkinson's Disease Gene Expression Analysis

A beginner bioinformatics project applying machine learning to classify 
Idiopathic Parkinson's Disease (IPD) from healthy controls using 
peripheral blood gene expression data from GEO dataset GSE99039.

---
## Project Overview

This project demonstrates a complete bioinformatics ML pipeline:

**Data Loading → EDA → Preprocessing → PCA → Random Forest Classification**

Starting from a raw microarray dataset of 558 samples and 54,675 probes,
the pipeline filters, cleans, compresses and classifies gene expression 
data to distinguish IPD patients from healthy controls.

---

## How to Run

1. Download `GSE99039_series_matrix.txt.gz` from 
   [GEO](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE99039)
   and place it in the `data/` folder
2. Run notebooks in order from 01 to 05

---
## Methods

### 1. Data Loading
The raw series matrix file from GEO was loaded and transposed so that rows represent 
samples and columns represent probes — the standard format for machine learning.

### 2. Exploratory Data Analysis
Sample labels were extracted from the series matrix metadata. The dataset contains 
multiple neurological conditions (IPD, DRD, CBD, MSA, GPD, Vascular dementia). 
Only IPD and Control samples were retained for this binary classification task. 
Gene expression distributions and variance were explored visually.

### 3. Preprocessing
- Removed Affymetrix control probes (AFFX prefix) — technical artifacts, not real human genes
- Applied variance-based feature selection — retained top 25% most variable genes (~13,000 probes)
- Applied StandardScaler normalization — mean=0, std=1 for each gene

### 4. Dimensionality Reduction (PCA)
Principal Component Analysis was applied to compress ~13,000 gene columns into 30 
principal components, explaining 80% of the total variance. The 2D PCA plot showed 
considerable overlap between IPD and Control groups, consistent with the subtle and 
heterogeneous nature of Parkinson's disease gene expression in peripheral blood.

### 5. Classification (Random Forest)
A Random Forest classifier (100 trees) was trained on an 80/20 train/test split 
with stratification to preserve class proportions. Model performance was evaluated 
using accuracy, confusion matrix, ROC curve and AUC score.

---

## Results

| Metric | Value |
|---|---|
| Test Accuracy | 55.7% |
| AUC Score | 0.584 |
| Model | Random Forest (100 trees) |
| Features | 30 PCA components |
| Training samples | 350 |
| Test samples | 88 |

### Interpretation
The classifier achieved modest performance above random chance (50% accuracy, AUC 0.50). 
The confusion matrix revealed the model identifies Control patients more reliably 
(64% recall) than IPD patients (46% recall), reflecting the heterogeneity of 
Parkinson's disease expression patterns in blood.
