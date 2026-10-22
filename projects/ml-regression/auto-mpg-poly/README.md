# Auto MPG: polynomial regression

Notebook: [mpg_PR.ipynb](mpg_PR.ipynb) · [Rendered notebook](notebook.html)

**Question:** Does a polynomial in horsepower predict fuel economy (mpg) better than a straight line, which degree works best, and how does a multi-feature model compare?

**Data:** [Auto MPG](https://archive.ics.uci.edu/dataset/9/auto+mpg) (UCI Machine Learning Repository; CSV copy from [Kaggle](https://www.kaggle.com/datasets/uciml/autompg-dataset)), 398 cars; 6 rows with missing horsepower dropped, leaving 392. Loaded into a local PostgreSQL table `auto_mpg`; metrics are logged to `model_results`. The CSV is not included.

**Methods:** 80/20 train/test split (`random_state=42`); linear vs degree-2 and degree-3 polynomial on horsepower; a degree sweep (1–10) with train, test and 5-fold CV RMSE; a 6-feature linear model (displacement, horsepower, weight, model year, origin dummies).

**Results (from the notebook outputs):**

| Model | Test RMSE (mpg) | R² |
|---|---|---|
| Linear, horsepower only | 4.71 | 0.566 |
| Degree 2, horsepower only | 4.29 | 0.639 |
| Degree 3, horsepower only | 4.30 | 0.638 |
| Linear, 6 features | 3.29 | 0.787 |

- Degree sweep: CV RMSE drops from 4.94 (degree 1) to 4.37 (degree 2) and then stays between 4.35 and 4.47 up to degree 10.
- The last cell fits a scaled degree-2 pipeline on the 6 features but does not print its metrics.

Charts: [fitted_curves.png](fitted_curves.png) (blue linear, green degree 2, yellow degree 3), [rmse_by_degree.png](rmse_by_degree.png).

The published notebook connects with `postgresql+psycopg2://postgres@localhost:5432/postgres`; the local Postgres password lives in `~/.pgpass` and is not in this repository.

## Get the data

Source: [Auto MPG](https://archive.ics.uci.edu/dataset/9/auto+mpg) (UCI Machine Learning Repository); CSV copy on [Kaggle](https://www.kaggle.com/datasets/uciml/autompg-dataset). The CSV is not included in this repo.

1. Download `auto-mpg.csv` from the Kaggle page above.
2. Place it in this folder (`projects/ml-regression/auto-mpg-poly/`), next to `mpg_PR.ipynb`.
3. Start a local PostgreSQL and run the notebook top to bottom.
