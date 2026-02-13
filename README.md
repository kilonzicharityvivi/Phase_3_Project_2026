# Phase_3_Project
Predicting Customer Churn
# SyriaTel Customer Churn Prediction (Phase 3)

## Executive summary
SyriaTel loses revenue when customers churn. This project builds a **binary classifier** that predicts whether a customer is likely to churn, so the business can **prioritize retention outreach** (calls, service recovery, plan adjustments) for the customers most at risk.

This model is designed to answer a practical question: **“Are there predictable patterns in customer behavior that signal churn early enough to intervene?”**

## Business understanding
- **Objective:** identify customers at elevated churn risk and reduce preventable churn.
- **Primary success metric:** **Recall for churners** (catch as many true churners as possible), while monitoring precision so the outreach list stays actionable.
- **How the score is used:** rank customers by churn probability; choose a threshold (or “top N%”) based on campaign capacity.

## Data understanding
- Dataset: 3,333 customer records with account details and usage patterns (day/eve/night/international), plan features, and customer service interactions.
- Target: `churn` (True/False).
- Key cleaning choices:
  - Dropped `phone number` (identifier, not predictive).
  - Dropped charge columns (charges are deterministic transforms of minutes in this dataset and create redundancy).

## Data preparation
- Stratified train/test split to preserve churn prevalence.
- Preprocessing built with **pipelines** to avoid leakage:
  - Numeric: median imputation + standard scaling
  - Categorical: most-frequent imputation + one-hot encoding

## Modeling (iterative)
Models were trained and compared using a consistent evaluation framework:
- Logistic regression baseline (interpretable)
- Decision tree baseline (captures non-linear relationships)
- Random forest ensemble benchmark (stronger predictive performance)
- **Threshold tuning** to match the business tradeoff between “catch more churners” and “avoid too many false alarms”.

## Evaluation (test set)
A random forest with a business-tuned threshold performed well:

- Decision threshold: **0.35**
- Share of customers flagged for outreach: **14.3%**
- Recall (churners caught): **73.6%**
- Precision (flagged customers who truly churn): **74.8%**
- F1: **0.742**
- ROC-AUC: **0.885**

Confusion matrix at threshold 0.35:
- True negatives: 683
- False positives: 30
- False negatives: 32
- True positives: 89

## Recommendations (business)
1. **Operationalize a retention list:** score customers weekly/monthly and route the flagged group to retention outreach.
2. **Use “capacity-based targeting”:** start with the current threshold (≈14% flagged) and adjust based on how many customers the team can realistically contact.
3. **Match interventions to likely drivers:** prioritize customers with frequent customer service calls, heavy usage, or plan features associated with higher churn signals.
4. **Measure lift with A/B tests:** compare “model-driven outreach” vs. “business as usual” to quantify revenue saved and optimize the offer strategy.

## Repository navigation
- `notebooks/index_improved.ipynb` – end-to-end analysis (business framing → modeling → evaluation → saved visuals)
- `data/` – dataset CSV
- `images/` – exported visuals used in the presentation (confusion matrix, ROC, PR curve, feature importance, threshold tradeoffs)
- `presentation/SyriaTel_Churn_Prediction_Phase3.pptx` – stakeholder-facing slide deck
- `requirements.txt` – minimal Python dependencies

## Links
- Presentation: `presentation/SyriaTel_Churn_Prediction_Phase3.pptx`
- Visuals folder: `images/`
- Notebook: `notebooks/index_improved.ipynb`

## Sources
- Dataset (public): Kaggle “Customer Churn Dataset” (commonly distributed as the *bigml_59c288...* SyriaTel churn CSV):
  - https://www.kaggle.com/datasets/muhammadshahidazeem/customer-churn-dataset
- Background on telecom churn modeling and common approaches:
  - Ahmad et al. (2019), “Customer churn prediction in telecom using machine learning in big data platform”:
    - https://link.springer.com/article/10.1186/s40537-019-0191-6
