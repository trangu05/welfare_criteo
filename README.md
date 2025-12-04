# Welfare-Optimal Targeting: Uplift Modeling & Policy Evaluation

This project analyzes the Criteo Uplift v2 dataset to estimate heterogeneous treatment effects (CATE), evaluate uplift modeling methods, and design data-driven targeting policies. The goal is to compare “treat-all,” “treat-none,” and personalized treatment policies, and quantify how much incremental conversion each policy can generate.
The workflow covers pre-cleaning, exploratory data analysis, baseline analysis and CATE estimation.


## Workflow

welfare-optimal-targeting/
├── data/
│   ├── raw/                    # Original Criteo csv.gz (no need for processed here)
├── notebooks/
│   ├── 00_pre_clean.ipynb      # Initial cleaning / sampling
│   ├── 01_data_exploration.ipynb # Checking some descriptives 
│   ├── 02_baseline_analysis.ipynb # Getting the ITT and LATE
│   └── 03_causal_forest_policy_upgraded.ipynb   # CATE estimation using CF
├── requirements.txt            # Python dependencies
└── README.md

## Datasets
- Criteo Uplift Prediction Dataset v2:
Download page: https://ailab.criteo.com/criteo-uplift-prediction-dataset/
Direct CSV: http://go.criteo.net/criteo-research-uplift-v2.1.csv.gz
Dataset includes:
- Treatment indicator
- Outcome (conversion, visit)
- 11 engineered features (f0–f10)
- Randomized experiment structure enabling clean causal identification

## How to Run

1. Clone or download the repository.
2. Place the dataset into data/raw/.
3. Install dependencies:  pip install -r requirements.txt
4. Open the notebooks in order (01_eda → 02_baseline_ate → 03_cate_and_policy).
5. Run all cells


