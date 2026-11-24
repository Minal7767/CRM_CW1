# CRM_CW1: S&P 500 Share Segmentation (Risk-Return and Adapted RFM)

Coursework for 6BUIS017W CWK1. The notebook `CRM.ipynb` builds share-level features for S&P 500 constituents, clusters them with Ward agglomerative clustering and K-means, and reviews the results.

[Open in Colab](https://colab.research.google.com/github/Minal7767/CRM_CW1/blob/main/CRM.ipynb)

## Data

| Item | Value |
|---|---|
| Share selection | Current S&P 500 constituents from Wikipedia (503 tickers, retrieved 2026-11-23 15:06 UTC) |
| Price source | Yahoo Finance via `yfinance` (Close, Adj Close, Volume, splits, dividends) |
| Period | 2021-01-01 to 2024-12-31 (daily) |
| Benchmark | S&P 500 index (`^GSPC`) |
| Observation date | 2024-12-31 (60-session RFM window: 2024-10-07 to 2024-12-31) |
| Shares included | 486 (17 excluded, with reasons, in `inclusion_exclusion_log.csv`) |

## Notebook contents

- **Task 1: Data preparation**
  - 1.1 Ticker selection and data acquisition (saves a data snapshot)
  - 1.2 Validation and alignment to the benchmark trading calendar, including corporate action checks
  - 1.3 Mean weekly return and weekly beta
  - 1.4 Adapted RFM: Recency (days since last high-volume day), Frequency (high-volume days in the last 60 sessions), Monetary (sum of Close x Volume). A high-volume day is volume above twice the previous 20-session average.
  - 1.5 Worked example (AAPL), checks and the final feature table
- **Task 2: Clustering**
  - 2.1 Feature preparation (log1p of Monetary, standardisation)
  - 2.2 Ward agglomerative clustering with dendrograms
  - 2.3 K-means clustering with elbow and silhouette plots
  - 2.4 Cluster profiles, labelled visualisations and algorithm comparison (ARI)
  - 2.5 RFM correlations and sensitivity to removing Monetary
- **Task 3: Review of results**
  - 3.2.1 Cluster profiles and sector composition

## How to run

1. Open the notebook in Google Colab using the link above.
2. Run all cells from top to bottom (Runtime > Run all). Task 1 downloads the data, so an internet connection is needed.
3. Output CSV files and figures are written to the Colab working directory.

Yahoo Finance data and the Wikipedia constituent list change over time, so a fresh run may differ slightly. The CSV files in this repository are the snapshot used in the report.

## Package versions

Python 3, pandas 2.2.3, numpy 2.4.6, yfinance 0.2.66, scikit-learn 1.6.1, scipy 1.16.3, matplotlib. Random seed: 42.

## Output files

| File | Contents |
|---|---|
| `sp500_tickers.csv` | Constituent list with sector and retrieval date |
| `sp500_daily_2021_2024.csv.gz` | Daily price and volume data snapshot |
| `inclusion_exclusion_log.csv` | Validation checks and exclusion reasons per share |
| `weekly_returns.csv` | Weekly returns for each share and the benchmark |
| `task1_feature_table.csv` | One row per included share: mean weekly return, beta, Recency, Frequency, Monetary |
| `task2_prepared_features.csv` | Features after transformation, before standardisation |
| `task2_2_ward_rr_k_table.csv`, `task2_2_ward_rfm_k_table.csv` | Ward silhouette and merge height by k |
| `task2_3_kmeans_rr_k_table.csv`, `task2_3_kmeans_rfm_k_table.csv` | K-means inertia and silhouette by k |
| `task2_cluster_assignments.csv` | Cluster labels for the four baseline solutions |
| `task2_4_profile_*.csv` | Cluster profiles for each solution |
| `task2_5_rfm_correlations.csv` | RFM correlation table |
| `task2_5_traded_value_with_M.csv`, `task2_5_traded_value_without_M.csv` | Monetary removal sensitivity |
| `task2_feature_table_with_clusters.csv` | Final feature table with all cluster assignments |
