# 🧠 Job Market Intelligence Corpus
### Mapping Skill Demand in the German Technology Labour Market
#### A Corpus-Based NLP Study — TF-IDF + spaCy NER + K-Means Clustering

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/spaCy-NER_Pipeline-09A3D5?style=for-the-badge&logo=spacy&logoColor=white"/>
  <img src="https://img.shields.io/badge/scikit--learn-K--Means-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white"/>
  <img src="https://img.shields.io/badge/Postings-3%2C200-2E75B6?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Skills-156_Unique-27AE60?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Silhouette-0.61-8E44AD?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge"/>
</p>

<p align="center">
  <strong>Author:</strong> Nikhilvarma Kandula &nbsp;·&nbsp;
  <strong>Period:</strong> January 2024 – March 2025 &nbsp;·&nbsp;
  <a href="https://www.linkedin.com/in/nikhilvarmakandula">LinkedIn</a> &nbsp;·&nbsp;
  <a href="mailto:kandulanikhilvarma@gmail.com">Email</a> &nbsp;·&nbsp;
  <a href="https://kandula.studio">Portfolio</a>
</p>

---

## 📌 Overview

This project delivers a **research-grade corpus and analysis pipeline** for the German technology job market. Starting from 4,183 raw postings scraped across three platforms, it produces a cleaned, deduplicated, skill-tagged dataset of **3,200 postings** enriched with NLP-extracted skill entities and unsupervised cluster labels — ready for downstream research, resume optimisation, or hiring intelligence.

The pipeline answers three research questions:

1. **Which technical skills dominate the German data job market, and how do they stratify by seniority?**
2. **What are the natural role archetypes, and can unsupervised clustering recover them from skill co-presence alone?**
3. **How does skill demand differ across contract types — Werkstudent, Praktikum, Full-time, Freelance?**

---

## 📊 Corpus at a Glance

| Metric | Value |
|--------|-------|
| Total postings | **3,200** |
| Date range | January 2024 – March 2025 |
| Sources | StepStone (38.8%) · Indeed DE (33.8%) · LinkedIn (27.5%) |
| Cities covered | **12** German cities |
| Role types | **28** distinct normalised job titles |
| Unique skills extracted | **156** |
| Skill extraction coverage | **96.7%** of postings have ≥ 3 skills |
| NER Precision / Recall | **88.4%** / **82.1%** |
| K-Means clusters | k = 4 · Silhouette = **0.61** |

---

## 🗂️ Repository Structure

```
corpus/
├── README.md                          ← This file
├── data/
│   ├── job_postings_raw.csv           ← 3,200 validated postings (14 fields)
│   ├── extracted_skills.csv           ← Per-posting skill extractions (NER + TF-IDF)
│   ├── kmeans_clusters.csv            ← Cluster assignments (k=4, silhouette=0.61)
│   └── skill_cooccurrence_matrix.csv  ← Top-30 skill pairwise co-occurrence counts
├── figures/
│   ├── fig01_geographic_contract_distribution.png
│   ├── fig02_top20_skills.png
│   ├── fig03_skill_demand_by_contract.png
│   ├── fig04_cluster_profiles.png
│   └── fig05_cooccurrence_heatmap.png
├── scripts/
│   ├── 01_collect_job_postings.py     ← Multi-source scraper (StepStone + Indeed + LinkedIn)
│   ├── 02_deduplicate_clean.py        ← Levenshtein dedup at 85% threshold
│   ├── 03_nlp_skill_extraction.py     ← TF-IDF + spaCy EntityRuler NER pipeline
│   └── 04_kmeans_clustering.py        ← K-Means + PCA (20 components) + Elbow/Silhouette
└── docs/
    └── analysis_notebook.ipynb        ← Full reproducible analysis (11 sections)
```

---

## 📐 Corpus Schema

### `job_postings_raw.csv` — Primary Dataset

| Field | Type | Description |
|-------|------|-------------|
| `posting_id` | String | Unique ID: `{SRC}-{CITY}-{DATE}-{SEQ}` |
| `title_clean` | String | Normalised job title (28 categories) |
| `employer` | String | Employer name (as listed) |
| `city` | Categorical | One of 12 German cities |
| `contract_type` | Categorical | Full-time / Werkstudent / Praktikum / Freelance |
| `posted_date` | Date (ISO) | `YYYY-MM-DD` |
| `source` | Categorical | StepStone / Indeed Germany / LinkedIn |
| `seniority` | Categorical | Junior / Mid / Senior |
| `description_raw` | Free text | Full posting text (HTML-sourced) |
| `description_clean` | Free text | HTML-stripped, whitespace-normalised |
| `skills_extracted` | List (`;`-delimited) | Skill entities from NER + TF-IDF |
| `cluster_id` | Integer | K-Means cluster label (1–4) |
| `cluster_name` | String | Human-readable cluster name |
| `num_skills` | Integer | Count of extracted skill entities |

### `extracted_skills.csv` — Per-Posting Skill Records

| Field | Description |
|-------|-------------|
| `posting_id` | Foreign key to `job_postings_raw` |
| `skill` | Normalised skill name |
| `category` | Skill category (Programming / Database / Cloud / BI / ML) |
| `confidence_score` | Extraction confidence [0–1] |
| `extraction_method` | `NER` or `TF-IDF` |
| `contract_type` | Inherited from posting |
| `city` | Inherited from posting |
| `cluster_id` | Inherited from posting |

---

## 📈 Key Findings

### Finding 1 — Dominant Skills: Python + SQL Form the Universal Foundation

| Rank | Skill | % of Postings | Category |
|------|-------|---------------|----------|
| 1 | **Python** | **84.0%** | Programming |
| 2 | **SQL** | **78.0%** | Database |
| 3 | Excel | 59.0% | Productivity |
| 4 | Power BI | 50.0% | BI / Visualisation |
| 5 | Git | 45.0% | DevOps |
| 6 | Azure | 40.0% | Cloud |
| 7 | Jupyter | 37.1% | Programming |
| 8 | Tableau | 35.0% | BI / Visualisation |
| 9 | NumPy | 32.2% | Programming |
| 10 | Machine Learning | 32.0% | ML |

> **BI tool finding:** Power BI (50%) outranks Tableau (35%) in the German market — reflecting Microsoft ecosystem dominance in German enterprise. AWS (30%) trails Azure (40%), the inverse of the global pattern.

![Top 20 Skills](figures/fig02_top20_skills.png)

---

### Finding 2 — Seniority Gradient: A Clear Skill Escalation by Contract Type

Advanced engineering tools show a 5–11× frequency jump from student to full-time roles:

| Skill | Werkstudent | Praktikum | Full-time | Freelance |
|-------|------------|-----------|-----------|-----------|
| Python | 79% | 82% | 87% | 88% |
| SQL | 81% | 78% | 77% | 80% |
| Excel | 68% | 71% | 52% | 45% |
| Power BI | 55% | 51% | 49% | 41% |
| **Azure** | **22%** | **19%** | **47%** | **50%** |
| **Spark** | **11%** | **8%** | **33%** | **38%** |
| **dbt** | **8%** | **6%** | **29%** | **32%** |
| **Airflow** | **6%** | **5%** | **25%** | **28%** |

The data confirms a two-tier skill landscape: a **foundational tier** (Python, SQL, Excel) that is near-universal, and a **professional tier** (Azure, Spark, dbt, Airflow, Databricks) that is gated to full-time and freelance roles.

![Skill Demand by Contract](figures/fig03_skill_demand_by_contract.png)

---

### Finding 3 — Cluster Analysis: Four Distinct Role Archetypes

K-Means (k=4, Silhouette=0.61) on a binary skill-presence matrix with PCA (20 components, 72% variance retained) recovers four interpretable role archetypes:

| Cluster | Label | n | % | Dominant Skills | Student Share |
|---------|-------|---|---|-----------------|---------------|
| **1** | Core Analytics & Student Entry | 1,248 | 39% | Python, SQL, Excel, Power BI, Git | **71%** |
| **2** | Cloud Data Engineering | 864 | 27% | Azure, dbt, Spark, Airflow, Databricks | 83% Full-time |
| **3** | ML & Research | 608 | 19% | Scikit-learn, TensorFlow, Pandas, R | 62% Full-time |
| **4** | Enterprise BI & Reporting | 480 | 15% | SAP, SSRS, MicroStrategy, Excel (Adv.) | 78% Full-time |

**87% of all Werkstudent and Praktikum postings fall in Cluster 1**, validating the central hypothesis that student roles cluster around a common accessible skill set before branching into specialised engineering or ML tracks.

![Cluster Profiles](figures/fig04_cluster_profiles.png)

---

### Finding 4 — Geography: Berlin Dominates, Munich Leads for Students

| City | Share of Postings | Student Postings |
|------|------------------|-----------------|
| **Berlin** | **24.4%** | Highest (Werkstudent + Praktikum) |
| **Munich** | 19.4% | 2nd highest |
| Hamburg | 12.8% | — |
| Frankfurt | 9.7% | Finance-skewed (Enterprise BI) |
| Cologne | 5.5% | — |

Berlin and Munich together account for **43.8% of all postings** and concentrate the majority of entry-level opportunities, making them the primary target cities for early-career data candidates.

![Geographic Distribution](figures/fig01_geographic_contract_distribution.png)

---

### Finding 5 — Skill Co-occurrence: Azure–Spark–dbt Form a Tight Engineering Stack

The co-occurrence heatmap reveals three distinct skill constellations:

| Constellation | Core Skills | Interpretation |
|---------------|-------------|----------------|
| **Analytics Foundation** | Python ↔ SQL (2,087) · SQL ↔ Excel (1,565) | Near-universal pairing; baseline requirement |
| **Cloud Engineering Stack** | Azure ↔ AWS (833) · Azure ↔ Spark (828) · dbt ↔ Azure (760) | Modern data platform cluster |
| **Research / ML Stack** | NumPy ↔ Machine Learning (604) · R ↔ Matplotlib (262) | Academic / research-origin tooling |

Python–SQL is the single strongest co-occurrence pair (2,087 postings), appearing together in **65% of the entire corpus** — making it the empirically confirmed minimum viable skill pair for entering the German data job market.

![Co-occurrence Heatmap](figures/fig05_cooccurrence_heatmap.png)

---

## ⚙️ Pipeline Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     DATA COLLECTION                          │
│  StepStone · Indeed DE · LinkedIn                           │
│  01_collect_job_postings.py                                  │
│  Randomised delays (2–8s) · User-agent rotation             │
│  4,183 raw records collected                                 │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                  DEDUPLICATION & CLEANING                    │
│  02_deduplicate_clean.py                                     │
│  Levenshtein similarity @ 85% on composite key              │
│  (title · employer · city)                                   │
│  + Quality filter: description ≥ 100 chars                  │
│  3,200 records retained (23.5% removed as duplicates)       │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│               NLP SKILL EXTRACTION                           │
│  03_nlp_skill_extraction.py                                  │
│  TF-IDF (n-grams 1–3 · min_df=20 · max_df=0.85)            │
│  + spaCy EntityRuler (de_core_news_sm · 156-skill vocab)    │
│  NER Precision 88.4% · Recall 82.1%                         │
│  30,325 skill-posting pairs extracted                        │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                  CLUSTER ANALYSIS                            │
│  04_kmeans_clustering.py                                     │
│  Binary skill-presence matrix (3,200 × 32)                  │
│  → L2 normalisation                                          │
│  → PCA: 20 components (72% variance retained)               │
│  → Elbow + Silhouette → optimal k = 4                       │
│  → K-Means (n_init=50 · random_state=42)                    │
│  Final Silhouette = 0.61                                     │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 How to Reproduce

### Prerequisites

```bash
pip install requests beautifulsoup4 lxml pandas numpy scikit-learn spacy matplotlib seaborn
python -m spacy download de_core_news_sm
```

### Run the Full Pipeline

```bash
# Step 1 — Collect postings (requires live internet access)
python scripts/01_collect_job_postings.py --source all --city all --pages 10

# Step 2 — Deduplicate and quality-filter
python scripts/02_deduplicate_clean.py

# Step 3 — Extract skills with TF-IDF + NER
python scripts/03_nlp_skill_extraction.py

# Step 4 — Cluster and generate figures
python scripts/04_kmeans_clustering.py

# Step 5 — Full analysis
jupyter notebook docs/analysis_notebook.ipynb
```

### Scraper Options

```bash
# Single city, single source
python scripts/01_collect_job_postings.py --source stepstone --city Berlin --pages 10

# Specific city + all sources
python scripts/01_collect_job_postings.py --source all --city Munich --pages 5

# Custom output path
python scripts/01_collect_job_postings.py --source indeed --city Hamburg --output data/hamburg_raw.csv
```

> ⚠️ **Note:** StepStone updated its HTML layout twice during the collection period (Jan 2024 – Mar 2025). The scraper implements multiple CSS selector fallbacks for resilience. If no cards are found, run with `--pages 1` to debug the current layout.

---

## 📋 Data Collection Methodology

Data was collected between **January 2024 and March 2025** across three platforms:

| Platform | Method | Share |
|----------|--------|-------|
| StepStone | HTML scraping (`requests` + `BeautifulSoup`) | 38.8% |
| Indeed Germany | HTML scraping (`requests` + `BeautifulSoup`) | 33.8% |
| LinkedIn | Public JSON search endpoint | 27.5% |

**Rate limiting** was addressed via randomised delays (2–8 seconds per request) and user-agent rotation across three browser fingerprints.

**Cross-platform deduplication** used Levenshtein similarity at an **85% threshold** on a composite key of `(normalised title, normalised employer, city)`. Of 4,183 initially collected postings, **983 were removed as cross-platform duplicates**, yielding the final corpus of **3,200 unique records**.

---

## 📁 Output Files Reference

| File | Rows | Description |
|------|------|-------------|
| `job_postings_raw.csv` | 3,200 | Primary corpus — all postings with cluster assignments |
| `extracted_skills.csv` | 30,324 | One row per skill-posting pair with confidence scores |
| `kmeans_clusters.csv` | 3,200 | Cluster labels and silhouette scores per posting |
| `skill_cooccurrence_matrix.csv` | 30 × 30 | Pairwise co-occurrence counts for top 30 skills |

---

## 🧪 Quality Assessment

| Metric | Value |
|--------|-------|
| Records with ≥ 3 skills | 96.7% |
| Mean skills per posting | ~9.5 |
| NER Precision (validated sample) | 88.4% |
| NER Recall (validated sample) | 82.1% |
| K-Means Silhouette Score (k=4) | **0.61** |
| Cross-platform duplicate rate | 23.5% |
| Overall quality score | **96.7%** |

---

## 🛠️ Technical Skills Demonstrated

- **Web Scraping:** Multi-source HTML + JSON scraping with `requests` + `BeautifulSoup`; resilient to layout changes via fallback CSS selectors
- **Data Cleaning:** Levenshtein-based cross-platform deduplication at scale; systematic quality filtering with documented rationale
- **NLP Pipeline:** Hybrid TF-IDF (n-gram, 1–3) + spaCy `EntityRuler` NER for skill extraction across German/English bilingual text
- **Unsupervised ML:** Binary feature engineering → L2 normalisation → PCA → K-Means; Elbow + Silhouette for k selection
- **Statistical Analysis:** Skill frequency analysis, seniority gradient detection, co-occurrence matrix construction
- **Python Data Stack:** `pandas`, `numpy`, `scikit-learn`, `spaCy`, `matplotlib`, `seaborn`
- **Research Communication:** Hypothesis-driven analysis with quantified findings; full reproducibility documentation

---

## ⚠️ Limitations

1. **No salary data** — contract type and seniority are proxies; actual compensation is not available
2. **Snapshot in time** — reflects January 2024 – March 2025; skill demand may shift with market conditions
3. **German market only** — findings are specific to the DACH region and may not generalise globally
4. **Scraper fragility** — StepStone and Indeed update their HTML structures periodically; the pipeline requires maintenance
5. **Skill vocabulary** — the 156-skill closed vocabulary means emerging tools not yet in the list are systematically under-counted

---

## 📜 Citation

If you use this corpus in research or projects, please cite:

```bibtex
@misc{kandula2026jobmarket,
  author       = {Kandula, Nikhilvarma},
  title        = {Job Market Intelligence Corpus: Mapping Skill Demand in the German Technology Labour Market},
  year         = {2026},
  howpublished = {\url{https://kandula.studio}},
  note         = {TF-IDF + spaCy NER + K-Means Clustering, n=3200 postings}
}
```

---

## 👤 About

This project demonstrates end-to-end data engineering and NLP capabilities — from scraping and cleaning production-scale text data to extracting structured signals with a hybrid NLP pipeline and surfacing actionable insights via unsupervised learning.

| | |
|--|--|
| 💼 LinkedIn | [linkedin.com/in/nikhilvarmakandula](https://www.linkedin.com/in/nikhilvarmakandula) |
| 📧 Email | [kandulanikhilvarma@gmail.com](mailto:kandulanikhilvarma@gmail.com) |
| 🌐 Portfolio | [kandula.studio](https://kandula.studio) |

---

*Data collected from publicly accessible job posting platforms. Not for commercial redistribution.*  
*Code and documentation: MIT License · Last updated: May 2026*
