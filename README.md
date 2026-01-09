# Welfare-Optimal Targeting: Uplift Modeling & Policy Evaluation

This project analyzes the Criteo Uplift v2 dataset to estimate heterogeneous treatment effects (CATE), evaluate uplift modeling methods, and design data-driven targeting policies. Using causal machine learning techniques, we identify which users benefit most from advertising treatment and develop optimal targeting strategies that maximize campaign profitability.

## Key Findings

- **Substantial Treatment Heterogeneity**: Predicted uplift ranges from near-zero to 59 percentage points, with mean CATE of 13.8pp (std: 15.4pp)
- **Strong Model Calibration**: Causal Forest successfully ranks individuals by treatment responsiveness—deciles with higher predicted uplift exhibit proportionally higher realized treatment effects (up to 50pp in top decile vs. near-zero in bottom decile)
- **Optimal Targeting Policy**: When accounting for treatment costs (R=$30 revenue per conversion, c=$10 cost per impression), the profit-maximizing strategy targets 66% of the population, achieving $31.24 maximum profit
- **Robust Uplift Ranking**: AUUC metrics (≈0.193) and policy values (≈0.250) remain stable across alternative nuisance model specifications (GBM vs. RandomForest+Logistic) and hyperparameters
- **Substantial Gains Over Baselines**: Targeted policy outperforms both treat-all (42.4% conversion rate) and random targeting across all targeting fractions

## Methodology Overview

This project employs **Causal Forest with Double Machine Learning (DML)** to estimate conditional average treatment effects in a randomized experiment setting.

**Causal Identification**: We leverage experimental randomization of ad exposure to satisfy unconfoundedness and overlap assumptions, enabling clean causal inference. The estimand of interest is τ(x) = E[Y(1) - Y(0) | X=x], the user-specific treatment effect.

**Estimation Approach**:
- **Causal Forest (Wager & Athey, 2018)**: Honest random forest that estimates heterogeneous treatment effects via adaptive splitting
- **Double Machine Learning (Chernozhukov et al., 2018)**: Doubly robust estimator that orthogonalizes treatment effect estimation using flexible gradient boosting models for nuisance functions (propensity score e(x) and outcome regression μ(x))
- **Cross-Fitting**: 3-fold cross-validation ensures Neyman orthogonality and prevents overfitting in nuisance estimation

**Policy Evaluation**: We use inverse propensity weighting (IPW) to estimate counterfactual policy values under top-q% targeting rules, accounting for treatment costs via profit = R × E[Y^π] - c × P(treat).

**Validation Methods**:
- Uplift curves and Area Under Uplift Curve (AUUC) for ranking quality
- Decile-level calibration plots comparing predicted vs. realized uplift
- Robustness checks across nuisance model specifications and hyperparameters


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
3. Install dependencies (Python 3.8+ required):
   ```bash
   pip install -r requirements.txt
   ```
4. Open the notebooks in order:
   - `00_pre_clean.ipynb` → Data loading and balance checks
   - `01_data_exploration.ipynb` → Exploratory data analysis
   - `02_baseline_analysis.ipynb` → Intent-to-Treat (ITT) and LATE estimation
   - `03_causal_forest_policy_upgraded.ipynb` → CATE estimation and policy learning
5. Run all cells sequentially

## Tech Stack

- **Core Libraries**: pandas, numpy, scipy
- **Machine Learning**: scikit-learn, xgboost
- **Causal Inference**: EconML (CausalForestDML)
- **Visualization**: matplotlib, seaborn
- **Environment**: Jupyter Notebook

## Limitations & Future Work

**Current Limitations**:
- **Computational Constraints**: Analysis uses stratified sample (50k observations per treatment-outcome cell) rather than full 14M dataset due to local compute limitations
- **Feature Interpretability**: Criteo features (f0-f11) are engineered/anonymized, limiting substantive interpretation of which user characteristics drive heterogeneity
- **External Validity**: Findings are specific to Criteo's advertising context; generalization to other platforms or product categories requires validation
- **Cost Parameters**: Profit optimization uses illustrative values (R=$30, c=$10); real deployment requires calibration to actual business metrics

**Potential Extensions**:
- **Alternative CATE Estimators**: Compare Causal Forest against meta-learners (S-learner, T-learner, X-learner) and neural network-based methods (DragonNet, TARNet)
- **Doubly Robust Policy Evaluation**: Extend policy value estimation to use AIPW (Augmented IPW) for increased efficiency
- **Constraint-Based Policy Learning**: Incorporate operational constraints (minimum treatment rate, fairness constraints, budget limits) into targeting optimization
- **Feature Importance Analysis**: Apply SHAP values or tree-based feature importance to identify key drivers of treatment heterogeneity
- **Multi-Outcome Optimization**: Extend to multi-objective setting (e.g., jointly optimize conversions and customer lifetime value)
- **Online Policy Learning**: Develop adaptive policies using contextual bandits or reinforcement learning for real-time optimization


