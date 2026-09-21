Cancer Treatment Analytics & Cost Prediction System

An end-to-end oncology analytics project combining survival analysis, biomarker analysis, and machine learning to predict treatment costs — built as a demonstration of how real-world evidence (RWE) style data can support both clinical insight and payer-facing cost forecasting (e.g. for a health insurance company estimating expected treatment spend per patient profile).

Live demo: Gradio web app for interactive cost prediction (see screenshot below) Notebook: Open in Colab — full pipeline from raw data to deployed model

Dataset
5,000 synthetic patient records, 14 fields, no missing values or duplicates
Fields: demographics (age, gender, smoking status), biomarkers (EGFR, ALK, PD-L1 expression), first-line treatment (7 regimens including immunotherapy, targeted therapy, chemotherapy combinations), and outcomes (progression-free survival, overall survival, treatment cost, hospital admissions)
Note: this is a reconstructed synthetic dataset (sourced via Kaggle), not real patient data — used here to demonstrate methodology, not for clinical claims
What this project does
Data quality & EDA — distribution checks across age, treatment, PD-L1 expression, and cost
Patient & treatment profiling — outcome comparisons across demographic/clinical subgroups
Survival analysis — Kaplan-Meier curves per treatment, with a multivariate log-rank test confirming progression-free survival differs significantly across the 7 treatment regimens (p < 0.001)
Biomarker analysis — EGFR/ALK mutation status and PD-L1 expression vs. survival outcomes
Cox Proportional Hazards model — multivariate survival modeling adjusting for age, PD-L1, and biomarker status
Treatment cost prediction (ML) — compared three regression models trained only on variables known at or near treatment start (age, gender, smoking status, EGFR, ALK, PD-L1, first-line treatment), deliberately excluding outcome variables like PFS/OS to avoid data leakage
Deployment — best model packaged into a Gradio web app for interactive "what-if" cost estimation

Linear Regression was selected as the final model — simplest, most interpretable, and best-performing on held-out data.The Camparision of different models are also shown 

What actually drives cost

Permutation feature importance on the final model showed one variable dominates:

First-line treatment regimen — by far the largest driver of predicted cost
PD-L1 expression, EGFR/ALK status, age, gender, and smoking status had near-zero marginal impact on cost once treatment choice was accounted for

Takeaway for a payer/insurer use case: treatment-cost risk in this dataset is driven almost entirely by which regimen a patient receives, not by their demographic or biomarker profile — which has direct implications for utilization management and prior-authorization cost modeling.

Tech stack

Python · pandas · scikit-learn · lifelines (survival analysis) · matplotlib · Gradio

Limitations
Synthetic/reconstructed dataset — not validated against real clinical or claims data
Descriptive/observational comparisons should not be read as proof of clinical superiority between treatments
Cost model R² (~0.67) leaves meaningful unexplained variance — a production payer model would need claims-level cost drivers (site of care, dosing, adverse events, etc.) not present here
