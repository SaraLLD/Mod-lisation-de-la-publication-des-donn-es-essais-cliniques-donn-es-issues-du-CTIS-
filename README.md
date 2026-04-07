# CTIS Clinical Trials — IA de prédiction de publication des résultats

> Projet exploratoire de machine learning appliqué aux données du **Clinical Trials Information System (CTIS)** de l'Agence Européenne des Médicaments (EMA).
Données CTIS extraites en date du 12 Mar 2026.
---

## Objectif

Prédire si un essai clinique enregistré sur le CTIS **publiera ses résultats** (`Trial results = Yes/No`), à partir de ses caractéristiques structurelles au moment de l'enregistrement.

Ce projet s'inscrit dans la problématique de **transparence réglementaire** : identifier les facteurs structurels associés au déficit de publication, en particulier dans les phases précoces et en pédiatrie.

---

## Structure du projet

```
ctis-ia/
│
├── data/
│   ├── CTIS_clean_data.csv          # Données brutes nettoyées
│   └── CTIS_clean_data_last.csv     # Données après typage (output notebook 01)
│
├── notebooks/
│   ├── 01_Analyse_qualitative.ipynb          # EDA — distributions, types, NA
│   ├── 02_Suite_analyse_qualitative.ipynb    # EDA — visualisations seaborn
│   ├── 03_modelisation_classification.ipynb  # Modélisation ML + SHAP
│   └── 04_github_structure.ipynb             # (à venir) déploiement
│
├── figures/
│   ├── roc_comparison.png           # Courbes ROC des 3 modèles
│   ├── confusion_matrix_xgboost.png # Matrice de confusion XGBoost
│   ├── shap_summary.png             # SHAP summary plot
│   ├── shap_importance.png          # SHAP bar plot
│   └── shap_waterfall_example.png   # SHAP waterfall individuel
│
├── models/
│   └── xgboost_ctis_pipeline.pkl    # Pipeline sklearn sérialisé (joblib)
│
├── requirements.txt
└── README.md
```

---

## Pipeline ML

| Étape | Détail |
|---|---|
| Variable cible | `Trial results` → binaire (Yes=1 / No=0) |
| Features | 4 numériques + 6 catégorielles (phase, sponsor, région, genre, âge, aire thérapeutique) |
| Preprocessing | Imputation médiane + StandardScaler (num) · Imputation + OneHotEncoder drop=first (cat) |
| Modèles comparés | Logistic Regression · Random Forest · XGBoost |
| Évaluation | StratifiedKFold (k=5) · AUC-ROC · F1-score |
| Interprétabilité | SHAP TreeExplainer (summary, importance, waterfall) |

---

## Résultats

| Modèle | AUC-ROC (CV) | F1-score (CV) |
|---|---|---|
| Logistic Regression | 0.921 ± 0.011| 0.435 ± 0.012 |
| Random Forest |0.954 ± 0.007| 0.360 ± 0.044|
| XGBoost |0.953 ± 0.007 | 0.591 ± 0.039 |

---

## Observations clés (EDA)

- Les essais de **Phase III/IV** publient significativement plus que les phases précoces
- Les **hôpitaux** peinent à recruter en pédiatrie vs les laboratoires pharmaceutiques
- Les essais **multinationaux hors-EEA** publient moins leurs résultats
- Signal global faible (r < 0.35) → justifie l'approche non-linéaire (XGBoost)

---

## Références

- EMA CTIS Public Portal : https://euclinicaltrials.eu
- SHAP : Lundberg & Lee (2017), *A Unified Approach to Interpreting Model Predictions*
- Règlement UE 536/2014 relatif aux essais cliniques

---

## Auteur
Sara LE LAY
Projet exploratoire — 2026
