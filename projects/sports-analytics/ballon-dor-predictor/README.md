# Ballon d'Or Predictor

> **Portfolio copy.** This is the project README as written. The rendered notebooks and the `.ipynb` files are in [`notebooks/`](notebooks/). The scripts, SQL schema, raw data, `notes.md` and CSV files mentioned below are not part of this portfolio folder.

Can player stats and team success predict who wins the Ballon d'Or? This project builds a Postgres database of nominees (2010-2025), turns it into one feature row per nominee per season, and compares three models with a leave-one-season-out test. It also ranks the 30 nominees for 2026.

**Stack:** Python (pandas, scikit-learn, SQLAlchemy), PostgreSQL, Jupyter.

## Data
- **Nominees and final ranks:** Wikipedia (2010-2025 via `scripts/wiki_reader.py`). The 2026 nominees come from a hand-made CSV because there are no ranks yet.
- **Player stats:** [FBref](https://fbref.com) (Big 5 European leagues), pulled with `soccerdata`.
- **Hand-made tables in `data/manual/`:** team success (league title and Champions League stage), international tournament stage, and name and club fixes.
- **Credit:** FBref and Wikipedia remain the property of their owners. This repo is for learning and portfolio use.
- Raw downloads in `data/raw/` are not in the repo. Regenerate them with notebooks 01 and 06 and `wiki_reader.py`.

## How it is built
CSV -> `sql/schema.sql` tables -> loader scripts in `scripts/` -> the `ballon_dor_features` view (one row per nominee per season: 408 labelled rows plus 30 for 2026).

Decisions (season mapping, the 0-6 stage scale, same-name players, missing stats) are documented in `notes.md`.

## Evaluation
Only 15 winners in 408 rows, so accuracy would be misleading. I used leave-one-season-out: train on 14 seasons, predict the 15th, and count how often the real winner is ranked first or in the top 3.

| Model | Top 1 | Top 3 |
|---|---|---|
| Baseline (player with most goals) | 4/15 | not measured |
| Logistic regression | 7/15 | 9/15 |
| Random forest (200 trees, balanced) | 8-9/15 (depends on seed) | 11/15 |

Random forest: 8-9/15 top 1 depending on seed (42: 8, 0: 9, 1: 8), 11/15 top 3 in all three.

Misses: 2012 (Messi), 2018 (Modric) and 2024 (Rodri) were never in the top 3. Voter narrative and star power are not in the features.

## 2026 prediction
Random forest trained on all 408 labelled rows. Top 5 by probability: Lamine Yamal (0.115), Harry Kane (0.050), Ferran Torres (0.035), Fabian Ruiz (0.030), Ousmane Dembele (0.030). Over 1000 random seeds, Yamal ranked first 912 times and Kane 72 times.

This is **not scored yet**: the 2026 winner has not been announced. See `data/predictions_2026.csv` and `data/mc_top_counts_2026.csv`.

## How to run
1. Create a Python environment (3.12 was used) and install the dependencies: `pip install -r requirements.txt`.
2. Create a PostgreSQL database and a `.env` file in the project root with a connection URL, for example `DB_URL=postgresql+psycopg2://YOUR_USER@localhost:5432/postgres`. The `.env` file is gitignored and never committed.
3. Keep the database password out of the URL and the code: put it in `~/.pgpass` (one line, `localhost:5432:postgres:YOUR_USER:YOUR_PASSWORD`, then `chmod 600 ~/.pgpass`). The Postgres driver reads it automatically.
4. Create the tables from `sql/schema.sql`. Run it table by table: the file contains `drop` statements.
5. Run the loader scripts in `scripts/`.
6. Open `notebooks/07_predict_2026.ipynb` and run it top to bottom. Notebooks 01-06 are the exploration log and were not designed to be re-run in order (some re-download data and would overwrite `data/raw/`).

## Limitations and next steps
- Small sample: 15 winners.
- No voter or narrative features (for example news or social sentiment); a possible v2.
- Stats cover Big 5 league players only. Players outside them get a `has_stats = 0` flag and median-filled values.
- The 2026 prediction is unverified until the award is announced.

## License
Code is released under the MIT License (see `LICENSE`). The underlying data belongs to FBref and Wikipedia and is not covered by this license.
