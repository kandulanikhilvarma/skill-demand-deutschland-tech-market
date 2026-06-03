# Job Market Intelligence Corpus
# Mapping Skill Demand in the German Technology Labour Market
## Repository Structure

```
corpus/
├── README.md                          ← This file
├── data/
│   ├── job_postings_raw.csv           ← 3,200 validated job postings (11 fields)
│   ├── extracted_skills.csv           ← Per-posting skill extraction (NER + TF-IDF)
│   ├── kmeans_clusters.csv            ← Cluster assignments (k=4, silhouette=0.61)
│   └── skill_cooccurrence_matrix.csv  ← Top-30 skill pairwise co-occurrence
├── figures/
│   ├── fig01_geographic_contract_distribution.png
│   ├── fig02_top20_skills.png
│   ├── fig03_skill_demand_by_contract.png
│   ├── fig04_cluster_profiles.png
│   └── fig05_cooccurrence_heatmap.png
├── scripts/
│   ├── 01_collect_job_postings.py     ← Web scraper (StepStone + Indeed)
│   ├── 02_deduplicate_clean.py        ← Levenshtein dedup + quality filter
│   ├── 03_nlp_skill_extraction.py     ← TF-IDF + spaCy NER pipeline
│   └── 04_kmeans_clustering.py        ← K-Means + PCA + Elbow method
└── docs/
    └── analysis_notebook.ipynb        ← Full reproducible analysis
```

---

## Corpus Summary

| Metric | Value |
|--------|-------|
| Total postings | 3,200 |
| Date range | January 2024 – March 2025 |
| Sources | StepStone (38.8%), Indeed DE (33.8%), LinkedIn (27.5%) |
| Cities covered | 12 German cities |
| Role types | 28 distinct job titles |
| Unique skills extracted | 156 |
| Overall quality score | 96.7% |
| NER Precision | 88.4% |
| NER Recall | 82.1% |
| K-Means clusters | k=4, Silhouette=0.61 |

---

## Corpus Schema (job_postings_raw.csv)

| Field | Type | Description |
|-------|------|-------------|
| posting_id | String | Unique ID: `{SRC}-{CITY}-{DATE}-{SEQ}` |
| title_clean | String | Normalised job title |
| employer | String | Employer name |
| city | Categorical | One of 12 German cities |
| contract_type | Categorical | Full-time / Werkstudent / Praktikum / Freelance |
| posted_date | Date (ISO) | YYYY-MM-DD |
| source | Categorical | StepStone / Indeed Germany / LinkedIn |
| seniority | Categorical | Junior / Mid / Senior |
| description_raw | Free text | Full posting text |
| description_clean | Free text | HTML-stripped, normalised text |
| skills_extracted | List (;-delimited) | Extracted skill entities |
| cluster_id | Integer | K-Means cluster (1–4) |
| cluster_name | String | Cluster label |
| num_skills | Integer | Count of extracted skills |

---

## Key Findings

| Finding | Detail |
|---------|--------|
| Dominant skills | Python (84%), SQL (78%), Excel (59%) |
| BI tool preference | Power BI (50%) outranks Tableau (35%) — Microsoft ecosystem dominance |
| Cloud preference | Azure (40%) > AWS (30%) > GCP (20%) |
| Seniority gradient | Azure/Spark/Airflow: 5–11% in student roles vs 25–47% in full-time |
| Cluster 1 (student) | 87% of Werkstudent/Praktikum postings fall in Cluster 1 |
| Top student city | Berlin (24.4%), Munich (19.4%) |

---

## Cluster Profiles

| Cluster | Label | n (%) | Top Skills | Student % |
|---------|-------|-------|------------|-----------|
| 1 | Core Analytics & Student Entry | 1,248 (39%) | Python, SQL, Excel, Power BI, Git | 71% |
| 2 | Cloud Data Engineering | 864 (27%) | Azure, dbt, Spark, Airflow, Databricks | 83% Full-time |
| 3 | ML & Research | 608 (19%) | Scikit-learn, TensorFlow, Pandas, R | 62% Full-time |
| 4 | Enterprise BI & Reporting | 480 (15%) | SAP, SSRS, MicroStrategy, Excel (Adv.) | 78% Full-time |

---

## How to Reproduce

```bash
# 1. Install dependencies
pip install requests beautifulsoup4 pandas numpy scikit-learn spacy matplotlib seaborn
python -m spacy download de_core_news_sm

# 2. Collect data (requires internet access to StepStone / Indeed)
python scripts/01_collect_job_postings.py --source all --pages 10

# 3. Deduplicate and clean
python scripts/02_deduplicate_clean.py

# 4. Extract skills
python scripts/03_nlp_skill_extraction.py

# 5. Run clustering
python scripts/04_kmeans_clustering.py

# 6. Open notebook for full analysis
jupyter notebook docs/analysis_notebook.ipynb
```

---

## Data Collection Methodology

Data was collected between January 2024 and March 2025 from three platforms:
- **StepStone** and **Indeed Germany** via HTML scraping using `requests` + `BeautifulSoup`
- **LinkedIn** via public JSON search endpoint

Rate limiting was addressed through randomised delays (2–8 seconds) and user-agent rotation.
Cross-platform duplicates were removed using Levenshtein similarity at an 85% threshold on
a composite key of (title, employer, city). The final corpus of 3,200 records represents
the cleaned, deduplicated subset of 4,183 initially collected postings.

---

## Citation

If you use this corpus, please cite:

> Kandula, N. (2026). *Job Market Intelligence Corpus: Mapping Skill Demand in the German
> Technology Labour Market*.

---

* All data collected from publicly accessible
job posting platforms. Not for commercial redistribution.*
