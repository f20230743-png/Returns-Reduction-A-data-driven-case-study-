# Returns-Reduction-A-data-driven-case-study-# Fashion Returns: A Data-Driven Case Study

Where do fashion returns actually come from, and what would move the needle on them? This project digs into a public online-fashion order dataset to find a concrete, addressable driver of returns, then sizes up whether a product fix is worth building.

## Dataset

Uses the Data Mining Cup 2016 online-fashion dataset, mirrored on Kaggle as *Predicting Returns of Discounted Articles Sales* — about 2.33M real order line items from an anonymized fashion retailer, with size, category, and return outcome per line. It's CC0 licensed. Raw data isn't included in this repo; the notebook pulls it from Kaggle at runtime.

- Dataset: https://www.kaggle.com/datasets/oscarm524/predicting-returns-of-discounted-articles-sales
- Reproducibility reference: https://github.com/myBytesResearch/fashion-returns-analysis

This is public third-party data, not any specific retailer's internal numbers — treat the category and size breakdowns as directional, not as a claim about any particular company's return rates.

## What the analysis finds

- Overall return rate in this dataset sits around 52% at the order-line level.
- The clearest lever is **size bracketing** — customers ordering the same item in multiple sizes and returning the rest. Brackets make up 16.6% of line items but carry a 73% return rate, and account for 23.5% of all returns.
- Not all of that is fixable with better recommendations — only the brackets where the customer ultimately keeps exactly one size (40.5% of brackets) are addressable by size prediction.
- A leakage-free size recommender, tested on that addressable pool, picked the size the customer actually kept 28.8% of the time, versus 19.1% for a naive item-level baseline.
- Translated into total returns, that recommender captures roughly 2% of all returns, against a theoretical ceiling of about 6.9% from purchase-history signals alone.
- At a public estimate of €3.60–€5.14 cost per return, a 2% reduction across 1M order lines works out to roughly €37k–€53k in avoided cost before factoring in what the intervention itself costs to build and run. This is a back-of-envelope scaling exercise, not a forecast for any real platform.

## Product takeaway

Fix the size table before reaching for AR. The data supports investing in three things, roughly in this order:

1. Personalized size recommendations based on a customer's own kept/returned history, falling back to category-level patterns for new users
2. Size charts based on actual garment measurements rather than generic S/M/L labels, with flags on SKUs where real order behavior contradicts the listed size
3. Fit signals mined from review text ("runs small," "true to size," etc.) once there's enough review volume per SKU

AR try-on is treated as a secondary, category-specific lever — useful where "how does this look on me" matters more than raw measurements, not a general fix for a broken size table.

## Repo contents

- `fashion_returns_analysis.ipynb` — the full notebook: data loading, return-rate breakdowns by category and size, the bracketing analysis, and the impact estimate
- `README.md` — this file

## Running it

Open the notebook and run top to bottom. It downloads the dataset via `kagglehub` on first run, so you'll need Kaggle credentials set up locally. No other setup required beyond the standard pandas/numpy/matplotlib stack.

## Caveats

- All figures come from a public, anonymized dataset — not from any real company's internal data, and category/size patterns will differ across retailers.
- The 2% return-capture number is a measured result for a purchase-history-only recommender; the fuller proposed fix (size chart + review signals + optional AR) hasn't been measured here, only argued for.
- The 1M-order cost estimate is illustrative math, meant to be swapped out with real order volume, margin, and return-handling costs if applied to an actual business.
