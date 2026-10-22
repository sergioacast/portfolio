# Concrete strength: support vector regression

Notebook: [concrete_svr.ipynb](concrete_svr.ipynb) · [Rendered notebook](notebook.html)

**Question:** How well can support vector regression predict concrete compressive strength, and how much do scaling and hyperparameter tuning matter compared with linear and polynomial baselines?

**Data:** [Concrete Compressive Strength](https://archive.ics.uci.edu/dataset/165/concrete+compressive+strength) (UCI Machine Learning Repository), 1,030 mixes, 8 inputs plus an engineered `wc_ratio` (water / cement) for 9 features. Loaded into a local PostgreSQL table `concrete`. The data file is not included.

**Methods:** 80/20 train/test split (`random_state=42`); linear and scaled degree-2 polynomial baselines; RBF SVR unscaled, then scaled RBF and linear SVR; `GridSearchCV` (5-fold, RMSE) over C, epsilon and gamma; a C × gamma heatmap at epsilon 3.

**Results (from the notebook outputs):**

| Model | Test RMSE (MPa) | MAE | R² |
|---|---|---|---|
| Linear regression | 9.73 | 7.64 | 0.633 |
| Polynomial degree 2 (scaled) | 7.38 | 5.93 | 0.788 |
| SVR RBF, unscaled | 14.16 | – | – |
| SVR RBF, scaled (defaults) | 9.37 | – | – |
| SVR linear, scaled (defaults) | 10.88 | – | – |
| SVR RBF, tuned (C=1000, epsilon=3, gamma=0.05) | 5.84 | 4.19 | 0.868 |

- Best 5-fold CV RMSE in the grid: 5.96 (C=1000, epsilon=3, gamma=0.05).
- Only RMSE is printed for the untuned SVR models.

Charts: [svr_rbf_tuned.png](svr_rbf_tuned.png) (actual vs predicted, tuned SVR), [heatmap_C_gamma.png](heatmap_C_gamma.png) (CV RMSE by C and gamma).

The published notebook connects with `postgresql+psycopg2://postgres@localhost:5432/postgres`; the local Postgres password lives in `~/.pgpass` and is not in this repository.
