---
title: "Modeling the Evolution of AI Language"
subtitle: "A Single-Firm Case Study"
author: "Marco Siliezar"
date: 2026-01-01
draft: false
description: "A longitudinal single-firm case study of Workday’s AI-related language, knowledge graph construction, and the Artificial Intelligence Intensity Index (AII)."
tags:
  - AI
  - Knowledge Graphs
  - NLP
  - SEC Filings
  - Finance
  - Workday
  - Time Series
categories:
  - Research
  - Data Science
---

*Marco Siliezar*  
Northwestern University  
[meatloaf02.github.io](https://meatloaf02.github.io)

## Abstract

With the popularity and fast evolution of artificial intelligence (AI) over the last several years, enterprise software firms have increasingly redistributed internal resources toward the development and integration of AI technologies. This research paper asks a focused question: **has this shareholder value been recognized?**

This study reviews the evolution of AI-related language, product capabilities, and risk disclosures from 2015 to the present for Workday, Inc., a business-to-business enterprise software company. Information about the company was extracted from Workday-authored documents collected from SEC filings, investor relations webpages, corporate blogs, and selected public news coverage. These documents were deduplicated and stored for the creation of a knowledge graph (KG). The KG serves both as a question-answering mechanism about the company and as the facility through which time-series analyses can be conducted.

The primary signal computed from the KG is an **Artificial Intelligence Intensity Index (AII)**. This index was used in a logistic regression framework to test whether AI-language intensity adds predictive value for next-quarter stock returns relative to a price-only baseline model.

The result is inconclusive. Price-only features dominate predictive accuracy, and adding AII as a feature does not improve model performance. AII appears to capture a real corporate disclosure pattern, but it does not translate into next-quarter return predictability for the stock on its own. Granger causality tests instead suggest a lagging disclosure narrative: management appears to discuss AI capabilities more intensely after favorable business periods. A natural next step is to expand the panel to 5 to 10 comparable enterprise firms to test whether AII performs better as a leading indicator or as a period sentiment measure in a broader comparative setting.

## Introduction and Problem Statement

Core enterprise software-as-a-service platform companies sell products to other businesses with the goal of increasing operational productivity. Workday, Inc. (NASDAQ: WDAY) is a publicly traded company that builds products for managing human resources, finances, payroll, and other core aspects of corporate and government operations. Because these systems are designed to improve efficiency, Workday provides an interesting case study for evaluating whether its claims of AI use appear meaningfully in public disclosures and whether those claims connect to signals relevant to financial outcomes.

In the United States, one important mechanism for disclosing and announcing claims of product functionality is through filings submitted to the Securities and Exchange Commission (SEC). These filings create transparency for the public and for investors, but the volume of material also creates an information access problem. Many useful questions about a company can only be answered across a large set of documents.

A knowledge graph database can help address this problem. Likely users of such an application include analysts, investors, management, and researchers who want structured knowledge about the company. In this project, the KG is **not** intended to directly answer the question, “Should I buy or sell WDAY?” Instead, it is designed to support structured analysis and to power a predictive modeling workflow.

The information contained in the KG is also used to generate predictive features. This research analyzes the change in AI-related language in disclosures over time and tests whether those language-based signals improve a model that predicts next-quarter stock direction. The model outputs the probability that the next quarter will be up or down based on a combination of knowledge-base signals and historical market signals.

## Literature Review

### Textual Analysis of SEC Filings as Financial Signal

Finance researchers have long studied the relationship between filing language and market outcomes. Loughran and McDonald (2011) showed that finance-specific word lists are more informative than generic dictionaries for understanding the impact of filing language on stock returns. Readability also matters: Li (2008) found that firms with weaker performance tend to produce annual reports that are more difficult to read. Together, these findings establish precedent for deriving signals from SEC filing text.

### Corporate Disclosures and Knowledge Graphs

Researchers have also explored the use of natural language processing and knowledge graphs to represent information in corporate disclosures. Cavar (2018) described a pipeline for processing 10-K filings, analyzing their semantics, and implementing a knowledge graph structure to store extracted information. This demonstrates the feasibility of domain-specific KGs for storing and analyzing disclosure data.

### AI-Related Language in Disclosures and Risks

The rise of AI-related language in public-company reporting has also been documented by industry analysis. Arize (2024) reported that Fortune 500 companies mentioned AI 250.1% more often in 2024 than in 2022, while AI-related risk language increased by more than 473.5% over the same period. This supports the broader framing of AI as both an opportunity and a risk in corporate narrative.

### Research Gap and Contribution

Prior work shows that filing language is informative, that signals can be extracted from corporate disclosures, and that knowledge graphs can represent otherwise non-obvious relationships. However, there is still limited work that integrates **longitudinal NLP**, **knowledge graph modeling**, and **firm-specific financial context** in a single framework. This project contributes by modeling Workday, Inc. as a longitudinal single-firm case study and constructing a KG that captures the evolution of AI-related language from 2015 to the present.

## Data

To build the dataset for predictive modeling, data from 2015 to 2026 was collected about Workday from SEC filings, official corporate webpages, earnings prepared remarks, and selected public news coverage. Most of the material ultimately used in the final analysis came from SEC filings. All documents were obtained from public and free sources.

The target scope was approximately **300 to 500 documents**, balancing comprehensive coverage with project feasibility. The minimum success criteria for ingestion were:

- at least one document for every quarter in the analysis period,
- inclusion of all SEC filings in scope, and
- at least 30 AI-relevant documents per year beginning in 2020.

The raw corpus initially contained **785 documents** across 2015 to 2025. Because SEC filings contain many supporting artifacts such as exhibits, attachments, and index pages, a canonical analysis layer was defined to reduce noise and preserve temporal comparability. Documents were deduplicated using accession numbers, and the analysis layer itself was versioned for future-proofing.

### Canonical Analysis Layer

The first canonical analysis layer, `canonical_v1`, included only the following primary narrative SEC documents:

- Form 10-K
- Form 10-Q
- Form 8-K
- DEF 14A proxy statements

Documents also needed to contain at least 100 characters to exclude near-empty XBRL shells.

A second version of the analysis layer excluded proxy statements because DEF 14A filings focus heavily on governance and executive compensation rather than AI-language evolution. After removing proxy statements, the final analysis corpus contained **104 documents**.

The final corpus scope for each fiscal year included:

- one Form 10-K,
- three Form 10-Q filings, and
- a variable number of Form 8-K filings.

### Limits of External Media Coverage

The project also attempted to gather public sentiment through focused web crawling. Those documents were excluded from the final research corpus for three reasons:

1. the crawler successfully retrieved usable material from only a narrow set of domains,
2. a random sample of fetched documents showed only **26% precision** for AI relevance, and
3. broader media crawling introduced substantial noise relative to the SEC-based corpus.

A separate dataset used for this project was Workday stock price history retrieved through Yahoo Finance.

## Methods

The overall pipeline consisted of five major stages:

1. web crawling and ingest,
2. document processing,
3. knowledge graph population,
4. quarterly signal construction, and
5. predictive modeling and evaluation.

![Figure 2. Architecture Overview](figure-2-architecture-overview.png)

### Ingest and Processing

Seed URLs were hand-curated to maximize high-signal coverage while minimizing noise. Sitemaps were also used to help the crawler discover relevant content deeper in target sites. The focused web crawler respected `robots.txt` and applied rate limiting for polite crawling. HTML content from SEC filings was processed using Beautiful Soup, while document metadata was stored in PostgreSQL using SQLAlchemy.

An intermediate manual step defined seed data for entities such as company products, technology capabilities, and risk topics. This improved information extraction quality and helped reduce downstream graph noise.

### Knowledge Graph Construction

The knowledge base was constructed using a curated method to leverage domain knowledge about Workday and its subsector. A simple but effective set of triples was defined to construct the graph. The KG was implemented in **Memgraph**, running in Docker, and queried in **Cypher**.

![Figure 3. Knowledge Graph Schema (Memgraph)](figure-3-kg-schema.png)

The KG schema includes RDF-compatible entities and relationships. It is designed to store documents that:

- mention technology capabilities,
- announce events, and
- disclose risk topics.

An event, for the purpose of this KG, was concretely defined as one of the following:

- earnings call,
- product launch,
- acquisition,
- partnership,
- leadership change,
- conference, or
- regulatory filing.

This narrow event definition reduced complexity and avoided needing a broader named entity recognition pipeline.

### Querying the KG

The repository supports querying the database in two ways:

1. through a Python connection module using the Neo4j driver over the Bolt protocol, and
2. directly through the Memgraph interface.

Memgraph was the more user-friendly option for this research and for the intended downstream users of the KG. One of the strengths of Cypher is that logically rich graph queries can be expressed in only a few lines.

![Figure 4. Example Cypher query in Memgraph](figure-4-cypher-query.png)

The example query shown above retrieves capabilities and product mentions for documents published on November 11, 2019. In that filing, Workday used an 8-K disclosure to mention artificial intelligence and machine learning capabilities alongside existing products.

The graph also supports broader portfolio-style questions. For example, a short query can return the top 10 most-mentioned products across the entire corpus from 2013 to 2025.

![Figure 5. Top 10 product mentions](figure-5-top-product-mentions.png)

### Artificial Intelligence Intensity Index (AII)

To support time-series analysis and predictive modeling, this project introduced an **Artificial Intelligence Intensity Index (AII)**. The purpose of AII is to measure how strongly Workday’s public narrative shifted toward artificial intelligence over time.

The AII is derived from `MENTIONS` edges in the KG. At the document level, a raw score is computed as the weighted sum of AI terms across term buckets. That raw score is then normalized by estimated token count, using character count divided by four as an approximation. At the quarterly level, AII is aggregated by combining weighted raw scores and token counts, while also incorporating document-type weights.

#### AI Term Bucket Weights

- **1.0x**: “artificial intelligence”, “machine learning”, “deep learning”
- **2.0x**: “generative ai”, “large language model”, “llm”, “copilot”, “foundation model”
- **0.75x**: “ai-powered”, “intelligent automation”, “predictive analytics”

#### Document-Type Weights

- **10-K = 1.5x**
- **10-Q = 1.2x**
- **8-K = 1.0x**

The rationale is that filing types carry different levels of signaling credibility and strategic importance. A 10-K is treated as the strongest narrative signal, followed by 10-Q updates, with 8-K event filings given the lowest weight.

Several robustness checks were used to evaluate AII as a modeling feature. Equalizing bucket weights and removing document-type stratification changed the signal only minimally. Normalizing by document count instead of token count produced more divergence, though the major signal peak remained aligned in **2020-Q1**.

## Results

### AI Intensity Index

The quarterly AII follows a clear pattern over the analysis period. AI-language intensity was near zero in 2012 to 2013, began to rise through late 2019, reached a maximum in **2020-Q1**, and then shifted toward generative-AI language beginning in **2023-Q2**.

![Figure 6. AII quarterly and AII change](figure-6-aii-quarterly.png)

The paper identifies four broad eras:

1. a near-zero era,
2. an early non-zero AI era,
3. an AI surge beginning in 2019-Q4, and
4. a generative-AI era beginning in 2023-Q2.

These eras suggest that AII captures a real change in strategic corporate disclosure over time.

### Model Results

The project addressed two prediction problems:

1. a **binary classification** task predicting whether the next quarter would be up or flat/down, and
2. a **regression** task predicting next-quarter log return.

AII was lagged by one quarter to reflect the fact that quarterly disclosure information is only complete at the end of the period. The main comparison was between a price-only baseline and AII-augmented models.

#### Feature Sets

| Feature Set | Variables |
|---|---|
| Price-only baseline | `log_return_lag1`, `log_return_lag2` |
| AII-augmented | `log_return_lag1`, `log_return_lag2`, `aii_delta_lag1`, `bucket_classic_ai_lag1`, `bucket_generative_ai_lag1`, `bucket_adj_auto_lag1` |

After exploring both XGBoost and logistic regression, logistic regression was retained because the price-only logistic model performed best in holdout evaluation.

#### Chronological Splits

| Split | Timeline | Duration (quarters) |
|---|---|---:|
| Train | 2015-Q1 to 2019-Q4 | 20 |
| Validation | 2020-Q1 to 2021-Q4 | 8 |
| Test | 2022-Q1 to 2025-Q4 | 16 |

#### Holdout Results

| Model | Val Acc | Val AUC | Test Acc | Test AUC |
|---|---:|---:|---:|---:|
| LR Price-Only | 0.625 | 0.0000 | 0.8125 | 0.9048 |
| LR AII-Only | 0.2500 | 0.2500 | 0.4375 | 0.4127 |
| LR Price + AII | 0.2500 | 0.4167 | 0.4375 | 0.5556 |

#### Economic Performance

| Strategy | Cumulative Return | Annual Return | Win Rate | Quarters in Market |
|---|---:|---:|---:|---|
| LR Price-Only | 195.30% | 19.80% | 75.00% | 16/24 |
| LR AII-Only | 0.00% | 0.00% | — | 0/24 (all-cash) |
| LR Price + AII | 0.00% | 0.00% | — | 0/24 (all-cash) |

The AII-augmented models did **not** outperform the price-only baseline. At this sample size, AII did not add incremental predictive value. The paper also notes that the baseline performance is fragile because:

- the validation AUC was 0.0000,
- the total number of test quarters was small (`n = 16`), and
- the apparent success of the baseline may partly reflect luck or regime-specific alignment.

### Granger Causality

Granger causality testing did not support the interpretation that AII leads returns. However, there was evidence at **lag = 4** that returns may lead AII (`p = 0.037`). This is consistent with a lagging disclosure narrative: strong business performance may be followed by stronger AI-related disclosure language rather than the reverse.

## Conclusions

The Artificial Intelligence Intensity Index is better interpreted as a **time-based sentiment measure** than as a leading financial indicator. The design of the index reinforces this interpretation.

Generative-AI terms first appeared in Workday disclosures in **2023-Q2**. For context, OpenAI’s ChatGPT launched in **2022-Q4**. That creates a roughly two-quarter lag between the public mainstreaming of generative-AI language and its appearance in Workday’s disclosure narrative. This is broadly consistent with SEC filing cycles.

Although AII was measurable and internally coherent as a signal, it did not deliver incremental predictive value in the modeling framework used here. Models using AII alone performed near chance, and combining AII with price features reduced predictive performance. Granger tests instead suggested that returns may lead AII, supporting the interpretation that management discusses AI more intensely after favorable periods.

A valuable output of the project is therefore the knowledge graph itself. The KG supports questions such as:

- what AI capabilities does Workday claim,
- where are those capabilities mentioned,
- which products are emphasized over time, and
- what risk topics are emerging?

The schema is also abstract enough to support expansion to additional B2B SaaS firms.

## Directions for Future Work

This study relied primarily on SEC disclosures, which limited the breadth of narrative coverage. Future work should improve external media ingestion and document precision by:

- filtering search-result pages,
- requiring “Workday” in article titles,
- improving AI-relevance screening for crawled documents,
- incorporating RSS feeds for incremental updates, and
- exploring GDELT as a source for event extraction.

Earnings call transcripts may also add richer context by capturing unscripted analyst Q&A and management responses.

A major research extension would be to expand from a single-firm case study to a small panel of comparable enterprise software firms. That would allow stronger testing of whether AII behaves as a leading indicator, a lagging narrative measure, or a broader sentiment proxy.

## Acknowledgements

The author would like to acknowledge Professor Thomas W. Miller for feedback, guidance, and time-series modeling jump-start code. The author also acknowledges the use of Claude Code software for general development and debugging assistance.

## Data Availability

SEC filing documents and information extracted via web crawling are available in the local project repository. The data collection process is reproducible through the code in the public repository:

- [KG GitHub Repository](https://github.com/meatloaf02/KG)

## Code Availability

The essential code to reproduce this research is available in the project repository:

- [KG GitHub Repository](https://github.com/meatloaf02/KG)

## References

- Anthropic. 2026. *Claude Sonnet 4.6*. Version 2.1.71.
- Arize AI, Inc. 2024. *The Rise of Generative AI in SEC Filings*.
- Aroussi, Ran. 2025. *yfinance documentation*.
- Bayer, Michael. 2026. *SQLAlchemy*.
- Cavar, Damir and Matthew Josefy. 2018. “Mapping Deep NLP to Knowledge Graphs: An Enhanced Approach to Analyzing Corporate Filings with Regulators.”
- Docker, Inc. 2026. *Docker Desktop*.
- Kejriwal, Mayank, Craig A. Knoblock, and Pedro Szekely. 2021. *Knowledge Graphs: Fundamentals, Techniques, and Applications*.
- Li, Feng. 2008. “Annual report readability, current earnings, and earnings persistence.”
- Loughran, Tim and Bill McDonald. 2011. “When is a Liability Not a Liability? Textual Analysis, Dictionaries, and 10-Ks.”
- Memgraph, Ltd. 2020. *Memgraph Lab*.
- Neo4j. 2026. *Cypher Manual*.
- Nickel, Maximillian, Kevin Murphy, Volker Tresp, and Evgeniy Gabrilovich. 2015. “A Review of Relational Machine Learning for Knowledge Graphs.”
- Richardson, Leonard. 2015. *Beautiful Soup*.
- U.S. Securities and Exchange Commission. 2024. “SEC Charges Two Investment Advisers with Making False and Misleading Statements About Their Use of Artificial Intelligence.”
- Workday. 2025. “Third Quarter Fiscal 2026 Prepared Remarks.”
- XGBoost Developers. 2025. *XGBoost*.
- Yahoo Finance. 2026. “Workday, Inc. (WDAY) Stock Historical Prices.”

## Appendix

### Table 1. Data Manifest

| URL | Content | Document Count |
|---|---|---:|
| `https://blog.workday.com/` | Corporate blog posts | 7 |
| `https://investor.workday.com/` | Investor Relations official website | 8 |
| `https://techcrunch.com/tag/workday/` | Public news coverage | 1 |
| `https://www.prnewswire.com/` | Press releases | 2 |
| `https://sec.gov` | 10-K, 10-Q, 8-K filings, proxy statements | 784 |

### Table 2. Seed Examples

| Entity | Seed Data Examples |
|---|---|
| Company | Workday |
| Product | Workday-HCM, Workday-Financials, Workday-Payroll |
| Capability | AI, ML, Predictive Analytics, Automation, LLM |
| Risk Topic | Cybersecurity-Risk, Data-Breach, Regulatory-Compliance, AI-Ethics |

### Table 3. KG Entities

| Node Label | Description |
|---|---|
| Company | Workday, Inc. |
| Document | SEC filing or public document with provenance metadata |
| Product | Named Workday product or module |
| TechnologyCapability | AI/ML capability described in filings |
| Event | Corporate event (acquisition, launch, partnership) |
| RiskTopic | Disclosed risk category |

### Table 4. KG Relationships

| Type | Direction | Meaning |
|---|---|---|
| MENTIONS | Document → Product/Capability | Named mention with evidence span |
| DISCLOSES | Document → RiskTopic | Risk factor disclosure |
| ANNOUNCES | Document → Event/Capability | Forward-looking announcement |
| OWNS | Company → Product | Company-product linkage |
