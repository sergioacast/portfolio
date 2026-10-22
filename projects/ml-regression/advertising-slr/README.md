The data shows us that for every $1000 spent on TV ads we would see approx 47 more units sold. TV ad spending explains about 2/3 of variation in sales, and its predictions are typically off by about 3200 units. TV ad spending moves together with sales, but this alone doesnt prove TV ads cause the extra sales. Radio's slope is bigger (about 204 units per $1,000), but it explains only about 26% of the variation, so it's much noisier. Newspaper explains almost nothing (3%). 

I would recommend allocating the $1000 into TV ads because it's the most reliable out of the three we analyzed. Next, I'd test TV and radio together in one model to see whether radio adds value on top of TV.

## Get the data

Source: `Advertising.csv` from *An Introduction to Statistical Learning* (ISLR), [statlearning.com resources](https://www.statlearning.com/resources-first-edition). The CSV is not included in this repo.

1. Download `Advertising.csv` from the page above.
2. Save it in this folder (`projects/ml-regression/advertising-slr/`) as `advertising.csv` (lowercase, the name the notebook reads), next to `TV_slr_model.ipynb`.
3. Start a local PostgreSQL and run the notebook top to bottom.
