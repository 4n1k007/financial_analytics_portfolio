# 📊 FIN 4333 – Financial Analytics Project Portfolio

**Team Hemisphere** | BBA Finance | Financial Analytics Course

> A three-project portfolio applying Python-based quantitative methods to real equity and macroeconomic data — from dataset construction to volatility modelling to return forecasting.

---

## 📁 Repository Structure

```
fin4333-financial-analytics/
├── project_1_dataset_construction/
│   ├── raw_data/
│   │   ├── stock/
│   │   ├── firm_reports/
│   │   └── macro_api_raw/
│   ├── processed_data/
│   ├── final_data/
│   ├── notebooks/
│   ├── logs/
│   └── outputs/
│
├── project_2_garch_volatility/
│   ├── raw_data/
│   ├── processed_data/
│   ├── notebooks/
│   ├── outputs/
│   └── memos/
│
├── project_3_return_forecasting/
│   ├── raw_data/
│   ├── processed_data/
│   ├── final_data/
│   ├── notebooks/
│   ├── outputs/
│   │   ├── plots/
│   │   ├── model_comparison_table.csv
│   │   └── 30_day_return_forecast.csv
│   ├── memos/
│   ├── ai_appendix/
│   ├── portfolio_evidence/
│   ├── linkedin_evidence/
│   └── references/
│
└── README.md
```

---

## 🗂️ Project Overview

### Project 1 — Monthly Analytics-Ready Dataset Construction

**Objective:** Build a clean, reproducible monthly financial dataset for a Bangladeshi listed company by combining stock, firm-level, and macroeconomic data.

**Period:** January 2020 – December 2024 (one row per month)

**Data Sources:**
- Daily stock prices from a legitimate file-based source (CSV/Excel), transformed into monthly close prices and monthly returns
- Annual firm fundamentals extracted from official company annual reports (revenue, total assets, EPS)
- Bangladesh macroeconomic indicators retrieved via the **World Bank Indicators API** (inflation, GDP growth)

**Key Steps:**
1. Import and clean raw daily stock price data
2. Transform daily prices into monthly end-of-period prices and calculate monthly stock returns
3. Extract annual firm variables from company reports
4. Call the World Bank API and parse macroeconomic data for Bangladesh
5. Apply a **year-based alignment rule** — annual firm and macro values are assigned to every monthly row within the same calendar year
6. Merge all three components into one final monthly dataset
7. Run data quality checks (missing values, duplicates, date consistency, data types, suspicious values)

**Final Dataset Variables:**

| Variable | Description |
|---|---|
| `company_name` | Name of the selected company |
| `ticker_or_code` | Stock ticker or exchange code |
| `date` | Month-end date |
| `year` | Calendar year |
| `month` | Calendar month |
| `month_end_price` | Month-end closing stock price |
| `monthly_stock_return` | Monthly return calculated from month-end prices |
| `annual_revenue` | Annual revenue or net sales |
| `annual_total_assets` | Annual total assets |
| `annual_eps` | Annual earnings per share |
| `bd_inflation_annual_pct` | Bangladesh inflation, consumer prices (annual %) |
| `bd_gdp_growth_annual_pct` | Bangladesh GDP growth (annual %) |

**Deliverables:**
- `final_data/` — clean monthly CSV dataset
- `notebooks/` — fully commented, reproducible Jupyter notebook
- `logs/` — source log and quality-check sheet
- `outputs/` — mini data dictionary and project memo

**Tech Stack:** `Python` · `pandas` · `requests` · `World Bank API` · `Jupyter Notebook`

---

### Project 2 — GARCH Volatility Forecasting & Risk Memo

**Objective:** Estimate a GARCH(1,1) model on daily equity log-returns to quantify market risk and produce a forward-looking 5-day conditional volatility forecast for an investment committee.

**Company:** Target Corp (TGT) | **Period:** 2020–2025

**Key Findings:**

- **Stationarity:** ADF test confirmed structural stationarity of the log-return series (p < 0.01)
- **Mean model:** Exhaustive AIC grid search across 72 ARIMA(p,d,q) permutations selected **ARIMA(0,0,0)** — mean drift coefficient statistically indistinguishable from zero (μ = 0.0001, p = 0.838), confirming a pure random walk
- **Distribution:** Extreme non-normality confirmed — excess kurtosis of **36.51**, negative skewness of **-2.19**, necessitating a heavy-tailed **Student's t-distribution**
- **GARCH(1,1)-t parameters:**
  - ω = 0.7135 (long-run variance baseline)
  - α₁ = 0.1248 (shock sensitivity)
  - β₁ = 0.7196 (variance persistence)
  - **Persistence (α₁ + β₁) = 0.8444** → variance shocks have a half-life of ~4.1 days

**5-Day Conditional Volatility Forecast:**

| Horizon | Daily Volatility | Annualised Volatility |
|---|---|---|
| T+1 | 1.74% | 27.62% |
| T+2 | 1.81% | 28.70% |
| T+3 | 1.86% | 29.59% |
| T+4 | 1.91% | 30.32% |
| T+5 | 1.95% | 30.92% |
| **Cumulative (5-day)** | **4.15%** | — |

**Strategic Recommendations (Investment Committee Memo):**
- Delta-neutral hedging via options market to offset zero-drift directional exposure
- Long gamma/vega positioning (near-the-money straddles/strangles) to capture expanding variance
- De-leverage positions by at least 25% or widen stop-loss bands given annualised volatility exceeding 30%

**Tech Stack:** `Python` · `pandas` · `arch` · `statsmodels` · `matplotlib` · `scipy` · `Jupyter Notebook`

---

### Project 3 — ARIMA vs LSTM Return Forecasting

**Objective:** Forecast daily stock log-returns using two competing models — a classical ARIMA benchmark and a deep-learning LSTM network — then evaluate which approach offers better out-of-sample performance and justify the recommendation.

**Data:** Daily log-return series from Project 2 company | Strict chronological train / validation / test split (no data leakage)

**Models:**

| Model | Design | Selection Method |
|---|---|---|
| ARIMA(p,d,q) | AIC-optimised grid search; ADF stationarity test on log-returns | AIC + Ljung-Box residual diagnostics |
| LSTM | Sliding-window design (10-day lookback → next-day return); single LSTM layer (50 units) + Dense output | Validation loss monitoring |

**Evaluation Metrics:**

| Metric | Role |
|---|---|
| **RMSE** | Primary criterion — penalises large forecast errors |
| MAE | Stability check |
| Directional accuracy (%) | Secondary business metric — sign prediction |

**Decision Rule:** If LSTM shows only a marginal improvement or unstable performance, ARIMA is preferred — it is simpler, faster, and easier to explain to a non-technical investment committee.

**Model Comparison Table:**

| Model | Forecast Target | Test RMSE | Directional Accuracy | Interpretation |
|---|---|---|---|---|
| ARIMA(p,d,q) | Daily log return | [Insert] | [Insert %] | Statistical benchmark |
| LSTM | Daily log return | [Insert] | [Insert %] | Deep-learning challenger |

**Key Outputs:**
- Interactive actual-vs-forecast plots (Plotly HTML)
- 30-trading-day return outlook with uncertainty caveats
- Professional forecasting memo (executive summary, model table, selected model, business implication)
- AI appendix with full prompt log, error identification, corrections, and responsible-use statement

**Tech Stack:** `Python` · `pandas` · `NumPy` · `statsmodels` · `TensorFlow/Keras` · `scikit-learn` · `Plotly` · `Jupyter Notebook`

---

## 🛠️ Tech Stack & Skills Demonstrated

| Category | Tools & Libraries |
|---|---|
| Data handling | Python, pandas, NumPy |
| API integration | World Bank Indicators API (requests) |
| Statistical modelling | statsmodels (ARIMA, ADF, Ljung-Box), arch (GARCH) |
| Machine learning | TensorFlow/Keras (LSTM), scikit-learn (MinMaxScaler, metrics) |
| Visualisation | matplotlib, Plotly |
| Environment | Jupyter Notebook, GitHub |
| Documentation | Markdown, Excel (source logs), Word/PDF (memos) |

---

## ⚙️ How to Reproduce

### Prerequisites

```bash
pip install pandas numpy requests statsmodels arch tensorflow scikit-learn plotly matplotlib openpyxl jupyter
```

### Run the notebooks in order

```bash
# Project 1
jupyter notebook project_1_dataset_construction/notebooks/

# Project 2
jupyter notebook project_2_garch_volatility/notebooks/

# Project 3
jupyter notebook project_3_return_forecasting/notebooks/project3_arima_lstm_return_forecasting.ipynb
```

> All notebooks include line-by-line code comments, pre-block explanations, and post-block interpretations so that any reader can follow the workflow step by step.

---

## 🤖 AI Disclosure

AI tools (including large language models) were used at various stages of this project for code drafting, explanation, and memo writing. All AI-generated outputs were reviewed, tested, corrected where necessary, and verified against course requirements and financial logic. A detailed AI audit log is included in `project_3_return_forecasting/ai_appendix/`. No AI output was submitted without human review and validation.

---

## 🌐 Portfolio

A published online portfolio presenting all three projects — including visuals, memos, and model outputs — is available at:

> 🔗 **[Insert portfolio link here]**

---

## 👥 Team

**Team Hemisphere** — FIN 4333 Financial Analytics
Instructor: Prof. Md Mohan Uddin, PhD

---

## ⚠️ Disclaimer

All datasets, models, and forecasts in this repository are produced for academic purposes only. Nothing in this repository constitutes financial advice or a trading recommendation. Return forecasts are noisy and should not be treated as guaranteed trading signals.
