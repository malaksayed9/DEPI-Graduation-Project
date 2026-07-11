# 🎓 CUCAS Fú Scholarship — Data & Analytics Project

An end-to-end data engineering, analytics, and machine learning project built around **CUCAS**, China's official platform for international scholarship applications. The project centralizes scattered scholarship data into a governed data warehouse, surfaces insights through Power BI, and adds predictive capabilities to support fairer, more data-driven scholarship decisions.

---

## 📌 Table of Contents

- [Problem Statement](#-problem-statement)
- [Solution](#-solution)
- [System Overview](#-system-overview)
- [Data Overview](#-data-overview)
- [Data Model](#-data-model)
- [Data Warehouse Architecture](#-data-warehouse-architecture)
- [Data Preprocessing](#-data-preprocessing)
- [Machine Learning Workflow](#-machine-learning-workflow)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Key Insights](#-key-insights)
- [Recommendations](#-recommendations)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)

---

## 🧩 Problem Statement

- There is no centralized system that collects or organizes scholarship information.
- Decision-makers lack clear visibility into who receives scholarships, which fields are supported, and how opportunities are distributed.
- This makes it difficult to ensure fair, efficient, and data-driven scholarship allocation.

## ✅ Solution

The project delivers three core capabilities:

| Pillar | Description |
|---|---|
| **Data-Insight** | Help organizations make data-driven decisions by showing clear information on how scholarships are distributed, current trends, and gaps. |
| **Gap-Analysis** | Surface underserved fields, degrees, and applicant segments to guide better resource allocation. |
| **Impact-Maximization** | Optimize scholarship distribution to be fair, efficient, and to maximize impact for both recipients and organizations. |

## 🏗 System Overview

Data flows from source → data warehouse → consumption layer:

```
Source                 Data Warehouse (SQL Server)                 Consume
──────                 ────────────────────────────                ───────
Web Scraping   ──┐      Bronze Layer  →  Silver Layer  →  Gold Layer
CSV Files      ──┼──►   (Raw Data)       (Clean/Std)      (Business-Ready)  ──►  BI & Reporting
                 │                                                            ──►  Ad-Hoc SQL Queries
             CUCAS DB                                                         ──►  Machine Learning
                                                                               ──►  Python Analysis
```

- **Bronze Layer** — Raw data, tables, full load / truncate & insert, no transformations, no data model.
- **Silver Layer** — Cleaned & standardized data: cleansing, standardization, normalization, derived columns, enrichment.
- **Gold Layer** — Business-ready data as views: integrations, aggregations, business logic, modeled as a **Star Schema** (with flat and aggregated tables).

## 🗂 Data Overview

- Data gathered from the **CUCAS Scholarships official platform** (Chinese scholarships, available in 6 languages).
- Initially scraped via **BeautifulSoup** into CSV files; the dataset was then completed by generating additional synthetic records with **Faker**, carefully respecting data rules, table structures, and entity relationships.
- Handled missing values, outliers, inconsistent formats, and rare categories to ensure data quality and consistency.
- Normalized multi-entity data and applied business rules for constraints and logic.

## 🔗 Data Model

**Entities include:** University, Scholarship, FinancialDetails, Application, Applicant, Payment, FinancialSupports, Recommenders, Interviewer, Committee, Reviewer, Interview, Exam, Review.

An **ER Diagram** and a derived **Star Schema** were built for analytical consumption:

**Fact table:** `Fact-Application`

**Dimension tables:**
- Scholarship Dim
- Applicant Dim
- Exam Dim
- Interview Dim
- Date Dim
- Payment Dim
- Financial Support Dim
- Recommendation Dim
- Committee Dim

## 🏛 Data Warehouse Architecture

Built on **Microsoft SQL Server** using the **Medallion Architecture** (Bronze → Silver → Gold):

- **Source:** Web Scraping + CSV Files → staged in `Cucas DB`
- **Bronze:** Raw copies of `University`, `Scholarship`, `FinancialDetails`, `Applicant`, `Application`, `ExamTypes`, `ExamResults`, `Interviewer`, `Interviews`, `Committee`, `CommitteeMember`, `Support`, `ApplicantSupport`, `Payments`, `Recommender`, `Recommendation` — no constraints or relations.
- **Silver:** Cleaned & standardized versions of the same entities.
- **Gold:** Denormalized dimension/fact views ready for analytics — `Scholarship DIM`, `Applicant DIM`, `Fact Application`, `Exam Result DIM`, `Interviews DIM`, `Committee DIM`, `Financial Support DIM`, `Payment DIM`, `Recommendation DIM`, `Date DIM`.

## 🧹 Data Preprocessing

1. **Cleaning (Scraped Data)** — generated extra records based on rules, removed duplicates, fixed inconsistent values, ensured realistic relationships.
2. **Cleaning Synthetic Data (Faker)** — generated extra tables based on rules, removed duplicates, fixed inconsistent values, ensured realistic relationships.
3. **Warehouse Preparation (Bronze → Silver → Gold)**:
   - Bronze: raw copy, no constraints or relations
   - Silver: cleaned & standardized data
   - Gold: denormalized views for analytics

## 🤖 Machine Learning Workflow

### 1. Exploratory Analysis
- Assessed structure, missing values, and inconsistencies.
- Identified key numerical & categorical features.
- Guided feature engineering decisions.

### 2. Feature Engineering
- Created **Expected Total Applicant Payment** (average tuition, accommodation, living, and service fees).
- Built an **Attractiveness Score** combining rating, coverage ratio, and normalized payment.
- Removed leakage columns and encoded categorical data (Label + One-Hot Encoding).

### 3. Predictive Modeling (Regression)
- Train/test split evaluated with Linear Regression, Random Forest, XGBoost, and CatBoost.
- **Best Payment Model:** XGBoost — R² ≈ 0.97, MAPE ≈ 9%.
- **Attractiveness Regression:** Random Forest — R² ≈ 0.89.

### 4. Classification
- Converted Attractiveness Score into **Low / Medium / High** classes.
- Trained Random Forest, Logistic Regression, and XGBoost.
- **Best Classification Model:** XGBoost — Accuracy ≈ 90%.

| Class | Precision | Recall | F1-score | Support |
|---|---|---|---|---|
| High | 0.89 | 0.84 | 0.87 | 523 |
| Low | 0.95 | 0.95 | 0.95 | 568 |
| Medium | 0.86 | 0.90 | 0.88 | 609 |
| **Accuracy** | | | **0.90** | 1700 |

### 5. Clustering & Statistical Analysis
- **K-Means** segmentation using cost, attractiveness, ratings, and encoded features.
- Identified the optimal K (elbow method) and interpreted cluster profiles.
- **ANOVA** used to detect features with statistically significant differences across clusters.

## 📊 Power BI Dashboard

An interactive multi-page Power BI report, **Scholarships Analysis**, providing an integrated view of student applications, financial support, interviews, exams, and committee evaluations — all in one system.

**Pages include:**
- Home (KPI overview)
- Scholarships / Scholarships Overview / Financial Details Analysis / University Analysis
- Applicants / Academic Performance / Support & Demographics
- Applications
- Committees
- Recommendation
- Exams
- Interviews
- Payments / Financial Support
- Predictive Overview / Cost Expectation / Attractiveness Analysis / Attractiveness Segments / Clusters Analysis

**Headline metrics:** 280K applications, 200K applicants, 129 universities, 8,496 scholarships, 626M total payments, 79K recommendations, 164K interviews, 107K financial support requests.

**Dynamic parameters (12 groups)** allow users to switch chart dimensions on the fly, covering scholarship info, category, applicant profile, scholarship features/identity, location & education, nationality, financial info, payment parameters, detailed scholarship info, predictive modeling parameters, and pie chart selectors (Recurring / ApprovalStatus).

**Tooltips** show real-time application status (Accepted / Rejected / Pending) per entity.

**Slicers:** Location, University, Gender, Major, Current Level, Category, Approval Status, Exam Result, Is Valid, Degree, Teaching Language, Attractiveness Label.

## 💡 Key Insights

- **Programs:** Most scholarships target Bachelor's & Master's degrees; non-degree programs are rare.
- **Languages:** English (67%) dominates over Chinese (32%) as the teaching language.
- **Fields:** Engineering, Computer Science, Business, Medicine, and Science lead in scholarship volume.
- **Costs & Coverage:** Non-degree and Medicine programs have the highest costs; 61% of scholarships exclude living/accommodation coverage; average total cost ≈ $9.2K.
- **Universities:** 129 universities in the dataset; Jiangsu offers the most scholarships; Beijing Jiaotong, Shandong, and Southwest rank as top-rated.
- **Applicants:** ~200K applicants; average GPA of 2 (on the dataset's scale); average age 25; gender-balanced; 53% require financial support.
- **Applications & Exams:** 280K exams (65% fail rate); 164K interviews (60% pass rate); performance is STEM-heavy and consistent across languages and degrees.
- **Financial Aid:** 107K requests totaling $550M; 70% rejected; 51% recurring; top needs are Accommodation, Research, Tuition, and Living Expense grants.
- **Attractiveness:** Doctoral programs are the most attractive; Master's has the highest success rate; Intercultural Studies, Acting, and Economics are popular; Chinese-taught programs and scholarships under $10K tend to be more attractive; Agriculture & Social Sciences dominate the high-attractiveness segment.
- **Review & Recommendations:** 4 committees, 80 reviewers, averaging 16 years of experience; most reviewers come from top Chinese and global universities, indicating a slight concentration risk.

## 🎯 Recommendations

- **Coverage & Costs:** Expand living/accommodation support; increase affordable and non-degree program offerings.
- **Programs & Languages:** Promote English-taught programs; prioritize STEM fields and top-rated universities.
- **Applicants & Equity:** Support early-stage and high-achieving students; maintain gender balance; target high-demand cities and majors.
- **Exams & Interviews:** Address high failure rates; scale up the interviewer pool; focus resources on key programs.
- **Financials & Aid:** Optimize scholarships under $10K; review rejection criteria; prioritize high-demand grant categories.
- **Attractiveness:** Highlight long-term, high-demand, high-attractiveness programs (Intercultural Studies, Agriculture, Social Sciences).

## 🛠 Tech Stack

- **Scraping & Data Generation:** Python (BeautifulSoup, Faker)
- **Data Warehouse:** Microsoft SQL Server (Bronze / Silver / Gold — Medallion Architecture)
- **Modeling:** Star Schema (Fact-Application + 9 dimensions)
- **Machine Learning:** Python (scikit-learn, XGBoost, CatBoost) — regression, classification, clustering, ANOVA
- **BI & Visualization:** Power BI
- **Querying:** SQL (ad-hoc queries against the Gold layer)

## 📁 Project Structure

```
cucas-fu-scholarship/
├── data/
│   ├── raw/                  # Scraped CSVs & Faker-generated data
│   └── processed/            # Cleaned, standardized datasets
├── sql/
│   ├── bronze/                # Bronze layer scripts (raw load)
│   ├── silver/                # Silver layer scripts (cleaning/standardization)
│   └── gold/                  # Gold layer views (star schema)
├── ml/
│   ├── eda.ipynb               # Exploratory analysis
│   ├── feature_engineering.ipynb
│   ├── regression_models.ipynb
│   ├── classification_models.ipynb
│   └── clustering_analysis.ipynb
├── powerbi/
│   └── CUCAS_Scholarships_Analysis.pbix
├── docs/
│   ├── ER_Diagram.png
│   ├── Star_Schema.png
│   └── DWH_Architecture.png
└── README.md
```

## 📄 License

This project is provided for educational and portfolio purposes. Please review and adapt licensing terms as needed before public distribution.

## 🙌 Acknowledgements

Data sourced from the **CUCAS Scholarships** official platform, supplemented with synthetic data generated via Faker to complete the dataset for analytical and modeling purposes.
