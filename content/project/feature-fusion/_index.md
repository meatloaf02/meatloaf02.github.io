---
title: Feature Fusion for Multi-Modal Learning
date: 2024-12-12
summary: Exploring feature fusion techniques for combining multiple data modalities
tags:
  - Machine Learning
  - Multi-Modal Learning
  - Deep Learning
image:
  filename: feature_fusion.png
  focal_point: Smart
  preview_only: false
---
## Overview
Feature Fusion for Multi-Modal Learning explores how heterogeneous textual representations—ranging from word-level n-grams to character-level features and semantic phrase embeddings—can be combined to improve similarity search, classification, and clustering performance in labor-market alignment tasks. The work investigates how distinct feature classes encode complementary signals, and how their integration boosts retrieval accuracy for aligning vocational programs with job postings.

This research forms a core component of a larger applied NLP pipeline designed to assess the alignment between educational program descriptions and workforce demand. The fusion approach demonstrates that multi-modal text representations can outperform single-feature baselines in sparse, domain-specific corpora.

## Objectives
- **Evaluate** how individual text-representation families (word n-grams, character n-grams, phrase-level features, dense embeddings) behave across retrieval, classification, and clustering tasks.
- **Determine** which combinations of these features produce the strongest gains when fused.
- **Design** a fusion strategy that remains computationally efficient while incorporating multiple modalities.
- **Establish** a repeatable framework for multi-feature experimentation, allowing systematic comparison of fusion architectures.

## Datasets
The experiments draw from two primary corpora:

### Vocational Program Descriptions
Publicly available program and course descriptions from California community colleges, representing a wide range of career-technical fields (healthcare, IT, trades, etc.). These entries vary in structure and quality, making them an excellent testbed for robust text-representation methods.

### Job Postings (Adzuna API)
A large-scale set of U.S. job postings containing structured fields (title, category, location) and unstructured text (description, responsibilities, requirements). These postings serve as the target documents for retrieval and alignment experiments.

Both datasets present natural sparsity, domain variation, and uneven length—conditions under which feature fusion methods can offer substantial advantages.

## Methodology
The experiment pipeline consisted of four stages:

### 1. Feature Engineering Across Modalities
Multiple independent feature families were generated, each capturing distinct linguistic signals:

- **Word-level n-grams** (1–2 grams): high-precision keyword signal representing domain-specific terminology.
- **Character-level n-grams** (3–5 grams): robust handling of noisy text, abbreviations, and morphological variants.
- **Phrase-level features** via termhood and frequency thresholds: capturing multi-word concepts (e.g., *medical assistant*, *cybersecurity operations*).
- **Sparse TF-IDF vectors** for all features to maintain interpretability and speed.

### 2. Late Fusion Architecture
Each feature family produced its own similarity scores between programs and job postings. Fusion occurred at the score level, not the vector level, allowing each representation to contribute its strengths.

The fused similarity score was computed using a weighted sum of normalized cosine similarities across features. This preserved modality-specific nuance while reducing the risk of any single modality overwhelming the others.

### 3. Evaluation Across Multiple Tasks
Each fusion configuration was assessed using a unified experiment runner:

- **Retrieval (Alignment) Performance:** Precision@5 and Mean Reciprocal Rank (MRR)  
- **Classification Performance:** Macro-F1 using program-to-job alignment labels  
- **Clustering Quality:** Silhouette score across program vectors  

This multi-metric evaluation ensured the fusion did not improve one task at the expense of others.

### 4. Iterative Experimentation
Over a dozen fusion variants were tested, including:

- Additive models  
- Thresholded phrase inclusion  
- Weighted feature contributions  
- Feature ablations to isolate marginal gains  

The best-performing fusion model combined word 1–2 n-grams, character 3–5 n-grams, and phrase embeddings with a moderate termhood threshold.

## Technologies
- **Python (Scientific Stack):** NumPy, SciPy, pandas  
- **NLP Libraries:** scikit-learn (`TfidfVectorizer`, cosine similarity), spaCy for preprocessing  
- **Experiment Framework:** Custom experiment runner with reproducible pipelines and metric logging  
- **Visualization Tools:** matplotlib, seaborn for experiment diagnostics  

## Key Findings
- **Fusion Significantly Improves Retrieval**  
  The strongest fusion model delivered a **25%+ improvement in Precision@5** over the TF-IDF baseline, consistently retrieving more domain-aligned job postings.

- **Complementary Features Matter**  
  Word n-grams contributed precision; character n-grams contributed robustness; phrase-level features captured domain-specific concepts. Their combination produced measurable gains across tasks.

- **Phrase Thresholding Is Highly Effective**  
  Filtering phrases by a frequency-based threshold reduced noise and improved alignment quality—suggesting that conceptual phrases carry outsized signal in vocational domains.

- **Sparse Representations Work Well in Fusion**  
  Even without dense embeddings, carefully engineered sparse features delivered competitive performance, faster runtime, and better interpretability.

- **Fusion Enhances Stability Across Domains**  
  Fused models performed more consistently across fields (healthcare, IT, trades) than single-modal baselines, indicating better generalization.

## Applications
This feature-fusion approach provides a replicable method for multi-modal learning in education-to-work alignment and adjacent domains:

- **Program-to-Job Alignment Tools**  
  Supporting institutions in evaluating the labor-market relevance of their offerings.

- **Curriculum Design & Revision**  
  Identifying skill gaps or emerging areas by comparing programs to real-time workforce demand.

- **Career Pathway Recommendation Systems**  
  Improving recommendation quality through more nuanced similarity signals.

- **Labor Market Analytics Dashboards**  
  Offering richer, more stable insights for policymakers and institutional leaders.

- **Generalizable Text Similarity Engines**  
  Applicable anywhere multi-modal representations strengthen retrieval over noisy or heterogeneous corpora.
