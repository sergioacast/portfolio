# Insurance charges: multiple linear regression

Notebook: [insurance_nb.ipynb](insurance_nb.ipynb) · [Rendered notebook](notebook.html)

**Question:** How well can a multiple linear regression predict individual medical insurance charges from age, sex, BMI, children, smoking status and region?

**Data:** [Medical Cost Personal Datasets](https://www.kaggle.com/datasets/mirichoi0218/insurance) (Kaggle, `insurance.csv`, 1,338 rows). Loaded into a local PostgreSQL table `insurance`; model metrics are logged to `model_results`. The CSV is not included.

**Methods:** sex and smoker encoded 0/1, region one-hot encoded (drop first), a `smoker_bmi` interaction (smoker × BMI), 80/20 train/test split (`random_state=42`), scikit-learn `LinearRegression`, a log-target variant, statsmodels OLS summary, VIF, and 5-fold cross-validation.

**Results (from the notebook outputs):**

| Model (as logged in `model_results`) | R² | RMSE | MAE |
|---|---|---|---|
| mlr_baseline (no interaction) | 0.784 | 5,796 | 4,181 |
| mlr_log (log charges, back-transformed) | 0.607 | 7,815 | 3,889 |
| with smoker × BMI interaction | 0.865 | 4,574 | 2,757 |

- 5-fold cross-validated RMSE for the interaction model: 4,850 ± 392.
- Smokers average $32,050 in charges vs $8,434 for non-smokers (274 vs 1,064 people).

Note: the notebook was run out of order, so some cells (the coefficient printout and the VIF table) reflect the model without the interaction term.

The published notebook connects with `postgresql+psycopg2://postgres@localhost:5432/postgres`; the local Postgres password lives in `~/.pgpass` and is not in this repository.

## Get the data

Source: [Medical Cost Personal Datasets](https://www.kaggle.com/datasets/mirichoi0218/insurance) (Kaggle). The CSV is not included in this repo.

1. Download `insurance.csv` from the Kaggle page above.
2. Place it in this folder (`projects/ml-regression/insurance-mlr/`), next to `insurance_nb.ipynb`.
3. Start a local PostgreSQL and run the notebook top to bottom.
