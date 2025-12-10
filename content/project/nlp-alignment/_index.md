---
title: NLP Alignment Research
date: 2024-01-01
summary: Research on aligning large language models with human values and preferences
tags:
  - NLP
  - AI
  - Machine Learning
  - Alignment
image:
  caption: ''
  focal_point: ''
---

## NLP Alignment Project

### Overview
This project explores how well vocational education programs in California align with real-world labor market demand. Using natural language processing (NLP), I analyzed the semantic similarity between job postings and program descriptions to understand where curricula strongly match—or diverge from—industry needs. The project integrates core NLP concepts from preprocessing to vectorization, clustering, classification, feature engineering, and retrieval evaluation.

### Objectives
- Evaluate alignment between job postings and vocational programs at both the domain and sub-domain level (e.g., clinical vs. administrative healthcare).
- Identify feature-engineering and vectorization strategies that maximize retrieval accuracy.
- Test multiple machine learning methods—including n-gram features, Word2Vec/Doc2Vec embeddings, clustering, classification, and topic modeling—to understand their impact on alignment quality.
- Produce interpretable outputs and visualizations that quantify where programs map well to job market skills and where gaps exist.

### Approach
**1. Corpus Construction**
- Scraped and curated two parallel corpora: vocational program descriptions and job postings from Adzuna.
- Applied preprocessing techniques aligned with the course’s emphasis on tokenization, lemmatization, POS tagging, segmentation, and corpus statistics.

**2. Feature Engineering & Vectorization**
- Generated TF-IDF n-grams (word 1–2, character 3–5) and high-frequency phrase features.
- Built and compared multiple vector representations including TF-IDF, section-aware TF-IDF, Word2Vec, and hybrid fusion embeddings.

**3. Similarity & Retrieval Modeling**
- Applied cosine similarity and reciprocal-rank fusion to evaluate job–program alignment.
- Ran retrieval experiments measuring Precision@5 and MRR to compare model variants.

**4. Clustering & Topic Modeling**
- Used k-means, hierarchical clustering, and Latent Dirichlet Allocation (LDA) to examine natural groupings of programs and job postings.

**5. Classification Experiments**
- Labeled programs and job postings by domain and trained classical ML classifiers (e.g., logistic regression, SVMs, Naïve Bayes) to evaluate feature discriminability.

**6. Subdomain-Level Analysis**
- Segmented healthcare into clinical vs. administrative subdomains to evaluate alignment within specialized labor market segments.

### Key Technologies
- Python, Jupyter
- spaCy, NLTK, scikit-learn, gensim
- TF-IDF, n-gram models, Word2Vec, Doc2Vec
- Clustering (k-means, hierarchical), LDA topic modeling
- Similarity & retrieval metrics (cosine similarity, P@5, MRR)
- Data pipeline and preprocessing frameworks

### Current Status
Complete

### Results & Findings
- Feature-fusion TF-IDF model achieved the strongest retrieval performance, outperforming baselines by ~25% in P@5.
- Section-aware representations (splitting inputs into requirements, outcomes, description, skills, etc.) improved alignment by emphasizing semantically rich parts of the text.
- Phrase features with thresholding contributed significantly by capturing skill-specific language absent from unigrams/bigrams.
- Clustering analysis revealed clear separation on domain labels but more overlap within subdomains—highlighting areas where vocational programs lack specificity.
- Subdomain alignment in healthcare showed:
  - Programs labeled clinical produced substantially higher similarity to clinical job postings.
  - Administrative programs displayed weaker alignment, suggesting either overly generic curricular language or mismatch with industry expectations.
- Silhouette scores and embedding behavior confirmed that sparse corpora and domain-specific terminology benefited from TF-IDF methods more than dense neural embeddings.
