# raw_data/stock/

**File:** `Grameenphone_Stock_Price_History.csv` — real daily price history for
**Grameenphone (DSE: GP)**, 1 January 2020 – 30 December 2024 (1,163 trading days).

**Source:** investing.com → Grameenphone Ltd → Historical Data → *Download Data*.
Format: newest-row-first, US date `MM/DD/YYYY`. Columns: `Date, Price, Open, High, Low,
Vol., Change %`. The pipeline uses **`Price`** as the daily closing price; it parses the
date, sorts ascending, and resamples to month-end (other columns are ignored).

To refresh the data, re-download the full history from the same source and overwrite this
file (keep the same columns), then re-run the notebook.
