# Sergio Castillo — Data Analytics Portfolio

Data analyst working in Python and SQL/PostgreSQL.

**Live site:** https://sergioacast.github.io/portfolio/

Projects are grouped by area of work. Each project folder has its own README, a project page, the rendered notebook(s) and the original `.ipynb`.

## Trading

Strategy research and backtests, judged after costs.

### [EURUSD Intraday](projects/trading/eurusd-intraday/)

Two intraday EURUSD strategies, specified before coding and backtested bar-by-bar after spreads. Neither survives costs; the only hint of an edge (Asia-session mean reversion) is smaller than the spread, and an ML filter didn't help.

**Tools:** Python, pandas, NumPy, PostgreSQL, SQLAlchemy, scikit-learn, matplotlib

- Strategy A (London breakout), final 2024–25 test: **−0.18 R** per trade after costs on 343 trades (+0.00 before costs).
- Strategy B (Asia mean reversion), final EURUSD test: **+0.05 R** on 179 trades, inside its ±0.06 noise band; the same settings on GBPUSD gave **−0.10 R** on 165 trades.
- ML filter: logistic regression’s validation gain (+0.08 R) came from only 51 trades with noise of about ±0.14, so no filter was adopted.

[Project README](projects/trading/eurusd-intraday/README.md) · [Verdict](projects/trading/eurusd-intraday/reports/verdict.md) · [Strategy specs](projects/trading/eurusd-intraday/reports/strategy_specs.md) · [Notebooks](projects/trading/eurusd-intraday/notebooks/) · [Live page](https://sergioacast.github.io/portfolio/projects/trading/eurusd-intraday/)

### [Trend Backtest](projects/trading/trend-backtest/)

**Question:** Does a simple EMA crossover trend rule on SPY, QQQ and GLD beat buy-and-hold after costs (2005–2025), and does an ML filter improve it?

**Tools:** Python, pandas, NumPy, yfinance, PostgreSQL, SQLAlchemy, scikit-learn, matplotlib

- On the untouched 2020–2025 test, the tuned EMA 75/100 rule returned **7.2%** a year on SPY vs **15.0%** for buy-and-hold (Sharpe 0.52 vs 0.78), with a slightly smaller max drawdown (-30.4% vs -33.7%).
- EMA 75/100 was chosen from 29 pairs tested on 2005–2019 only (in-sample Sharpe 0.79); walk-forward picked it in 8 of 10 years.
- ML filter: logistic regression and random forest both scored **63.5%** vs a 64.1% always-up baseline, so the filter was dropped.

[Project README](projects/trading/trend-backtest/README.md) · [Research note](projects/trading/trend-backtest/reports/research_note.md) · [Notebooks](projects/trading/trend-backtest/notebooks/) · [Live page](https://sergioacast.github.io/portfolio/projects/trading/trend-backtest/)

## Analytics

Exploratory and SQL analysis of public datasets.

### [Late Deliveries (Olist)](projects/analytics/late-deliveries/)

**Question:** Which origin→destination routes and seller areas miss the promised delivery window most, and is lateness getting worse?

**Tools:** Python, pandas, PostgreSQL, SQLAlchemy, SQL (CTEs, FILTER, BOOL_OR), matplotlib

- Overall late rate: **8.11%** (7826 of 96470 late-eligible delivered orders); pandas and SQL agree.
- Monthly late rate peaked at **21.36%** in March 2018 (15.99% in February 2018, 14.31% in November 2017).
- In the Feb–Mar 2018 spike, **SP→CE** went from 9.43% late (normal) to **48.95%**.

[Project README](projects/analytics/late-deliveries/README.md) · [Notebook (.ipynb)](projects/analytics/late-deliveries/late_deliveries.ipynb) · [Live page](https://sergioacast.github.io/portfolio/projects/analytics/late-deliveries/)

### [NYC Airbnb 2019](projects/analytics/nyc-airbnb/)

**Question:** How are NYC Airbnb listing prices distributed, and how do they vary by borough, room type, reviews, availability and minimum nights?

**Tools:** Python, pandas, NumPy, matplotlib

- Prices range from $0 to $10,000; mean 152.720687 vs median 106.0.
- “Expensive typical” cell: **Manhattan Entire home**, median ~$191 on 13199 listings. Staten Island Entire’s high mean is a luxury-tail story (median ~$100).
- availability_365 == 0 for **17,533** listings (~36%), kept rather than silently dropped.

[Project README](projects/analytics/nyc-airbnb/README.md) · [Notebook (.ipynb)](projects/analytics/nyc-airbnb/nyc_airbnb.ipynb) · [Live page](https://sergioacast.github.io/portfolio/projects/analytics/nyc-airbnb/)

### [Superstore EDA](projects/analytics/superstore/)

**Question:** Where does Superstore lose profit, how much of it is driven by discounting, and what would a 20% discount cap change?

**Tools:** Python, pandas, matplotlib

- Discounts above 20% lose money: 21–30% bin −10369.2774, 31–50% −48447.7273, 51–80% −76559.0513 total profit.
- Worst cell: **Binders at 51–80% discount**, −38510.4964 profit on 613 lines. Tables lose −17725.4811 overall.
- A 20% discount cap (1393 lines affected) moves modelled profit from 286397.0217 to **505588.0159**.

[Project README](projects/analytics/superstore/README.md) · [Notebook (.ipynb)](projects/analytics/superstore/superstore_eda.ipynb) · [Live page](https://sergioacast.github.io/portfolio/projects/analytics/superstore/)

## ML regression

Three supervised regression exercises (scikit-learn, data loaded into PostgreSQL). The published notebooks connect without a password in the connection string; the local Postgres password lives in `~/.pgpass`.

### [Advertising: simple linear regression](projects/ml-regression/advertising-slr/)

**Question:** Which single ad channel (TV, radio or newspaper) best explains sales?

- TV fits best on the test set: **R² 0.677**, RMSE 3.19 (slope 0.0465). Radio R² 0.263, newspaper R² 0.030.

[Project README](projects/ml-regression/advertising-slr/README.md) · [Notebook (.ipynb)](projects/ml-regression/advertising-slr/TV_slr_model.ipynb) · [Live page](https://sergioacast.github.io/portfolio/projects/ml-regression/advertising-slr/)

### [Insurance charges: multiple linear regression](projects/ml-regression/insurance-mlr/)

**Question:** How well can a linear model predict medical insurance charges, and does a smoker × BMI interaction help?

- Interaction lifts test R² from 0.784 to **0.865**; MAE **$2,757**; 5-fold CV RMSE **$4,850 ± $392**.

[Project README](projects/ml-regression/insurance-mlr/README.md) · [Notebook (.ipynb)](projects/ml-regression/insurance-mlr/insurance_nb.ipynb) · [Live page](https://sergioacast.github.io/portfolio/projects/ml-regression/insurance-mlr/)

### [Auto MPG: polynomial regression](projects/ml-regression/auto-mpg-poly/)

**Question:** Does a polynomial in horsepower predict mpg better than a line, and which degree works best?

- Degree 2 cuts test RMSE from 4.71 to **4.29** mpg; higher degrees add little. A 6-feature linear model reaches RMSE **3.29**, R² 0.787.

[Project README](projects/ml-regression/auto-mpg-poly/README.md) · [Notebook (.ipynb)](projects/ml-regression/auto-mpg-poly/mpg_PR.ipynb) · [Live page](https://sergioacast.github.io/portfolio/projects/ml-regression/auto-mpg-poly/)

## Repository structure

```
.
├── index.html              home page (GitHub Pages, served from main branch root)
├── style.css               shared stylesheet
├── .nojekyll               serve files as-is (no Jekyll build)
├── projects/
│   ├── trading/
│   │   ├── eurusd-intraday/    project page, README, notebooks/, reports/
│   │   └── trend-backtest/     project page, README, notebooks/, reports/, charts
│   ├── analytics/
│   │   ├── late-deliveries/    project page, README, notebook.html, .ipynb, charts
│   │   ├── nyc-airbnb/
│   │   └── superstore/
│   ├── ml-regression/
│   │   ├── advertising-slr/    project page, README, notebook.html, .ipynb, charts
│   │   ├── insurance-mlr/
│   │   └── auto-mpg-poly/
│   └── <old-slug>/             redirect stubs for pre-reorganisation URLs
└── README.md
```

Old URLs such as `/portfolio/projects/superstore/` still work: the folders directly under `projects/` (other than the three area folders) contain only small meta-refresh pages pointing at the new location.

## How notebooks are rendered

Notebooks are rendered to static HTML with `jupyter nbconvert --to html` from their **saved outputs** (no re-execution), so the pages show exactly what the notebook printed. The rendered `.html` sits next to the `.ipynb` in each project folder and is linked from the project page.

## Data and credentials

- Raw datasets (Kaggle / ISLR CSVs) are **not** committed; `*.csv` is git-ignored. Each project README links to the original source so the data can be downloaded.
- Notebooks that use PostgreSQL connect without a password in the connection string (`postgresql+psycopg2://postgres@localhost:5432/postgres`); the local password is read from `~/.pgpass`, which is never committed. `.env` files are also git-ignored.
