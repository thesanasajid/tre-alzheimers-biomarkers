# Time-Restricted Eating and Alzheimer's Disease Staging

A biomarker-driven machine learning analysis using ADNI clinical data and CDC healthy aging population data.

## Overview

This project investigates whether a compact set of Time-Restricted Eating (TRE) relevant biomarkers can explain Alzheimer's disease staging nearly as well as a full clinical feature matrix. The analysis is grounded in the review by Gasmi et al. (2024), which identified TRE as a preventive strategy that influences brain glucose metabolism, inflammatory markers, and gut microbiota composition in Alzheimer's patients.

The core finding is that 20 TRE-relevant features achieved a Macro F1 of 0.6858 on five-class diagnosis staging, compared to 0.6734 from the full 351-feature model. This means the metabolic, cognitive, memory, structural, and genetic markers that TRE directly targets retain nearly all diagnostic signal.

## Key Results

| Metric | Full Feature Model | TRE Feature Model |
|---|---|---|
| Feature count | 351 | 20 |
| Test Macro F1 | 0.6734 | 0.6858 |
| Test Accuracy | 0.7960 | -- |
| 5-Fold CV Macro F1 | 0.6806 ± 0.0124 | -- |

Per-class performance (full model):

| Stage | Precision | Recall | F1 |
|---|---|---|---|
| CN (Cognitively Normal) | 0.80 | 0.93 | 0.86 |
| SMC (Subjective Memory Concern) | 0.25 | 0.05 | 0.08 |
| EMCI (Early MCI) | 0.73 | 0.76 | 0.75 |
| LMCI (Late MCI) | 0.78 | 0.83 | 0.81 |
| AD (Alzheimer's Disease) | 0.92 | 0.84 | 0.88 |

CN, LMCI, and AD are clearly separated. SMC is the weakest class due to small sample size (106 patients) and clinical overlap with normal aging.

## Datasets

| Dataset | Source | Rows | Columns | Role |
|---|---|---|---|---|
| [ADNI Alzheimer's Dataset](https://www.kaggle.com/datasets/sarthakkanjariya/alzheimer-dataset) | Kaggle (ADNI) | 1,737 | 371 | Patient-level modeling, SHAP, clustering |
| [CDC Alzheimer's Disease and Healthy Aging Data](https://www.kaggle.com/datasets/ananthu19/alzheimer-disease-and-healthy-aging-data-in-us) | Kaggle (CDC BRFSS) | 214,462 | 29 | Population-level cognitive decline and nutrition trends |

The ADNI dataset contains real clinical data from 1,737 patients across five cognitive stages sourced from the Alzheimer's Disease Neuroimaging Initiative. Features include cognitive test scores (MMSE, CDRSB, ADAS), memory assessments (RAVLT), brain region volumes (hippocampus, entorhinal, fusiform), FDG-PET glucose metabolism, ApoE4 genetic risk, and demographics.

The CDC dataset provides population-level behavioral risk factor surveillance data on cognitive decline, obesity, nutrition, and aging across US states from 2015 to 2020.

## TRE-Relevant Feature Set (20 features)

These features were selected because they map directly to the biological pathways TRE is hypothesized to influence:

| Category | Features |
|---|---|
| Metabolic | FDG_PET (brain glucose metabolism) |
| Cognitive | MMSE, CDRSB, ADAS11, ADAS13 |
| Memory | RAVLT_immediate, RAVLT_learning, RAVLT_forgetting, RAVLT_perc_forgetting |
| Brain structure | Hippocampus, Entorhinal, Fusiform, MidTemp, Ventricles, WholeBrain, Intra cranial volume |
| Demographic and genetic | Age, High_risk_ApoE4, Year_education, Gender |

## Analysis Pipeline

1. Exploratory data analysis across all five ADNI diagnosis stages
2. Preprocessing: drop all-null columns, encode categoricals, build full and TRE feature matrices
3. XGBoost classification with balanced class weighting and 5-fold stratified cross-validation
4. SHAP interpretation for both the full-feature and TRE-feature models
5. UMAP dimensionality reduction and KMeans clustering for patient phenotyping
6. CDC population analysis: cognitive decline trends, nutrition/obesity trends, state-level obesity vs cognitive decline correlation

## Important Notes

This analysis does not claim that TRE causes better Alzheimer's outcomes. The notebook frames results around TRE-relevant biomarkers and their diagnostic value for disease staging. The ADNI dataset does not contain direct TRE intervention data. The connection is that the features TRE is hypothesized to modulate (glucose metabolism, brain atrophy, cognition, memory) are shown here to be highly informative for AD staging.

## License

This project is for academic and research purposes. The ADNI dataset is subject to its own data use agreement. The CDC dataset is public domain.
