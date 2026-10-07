# Gold Price Drivers – Macro Factor Model (XAUUSD)

An end-to-end, reproducible R pipeline that models **daily changes in the gold price (XAUUSD)** using three market factors:

| Factor | Ticker | Why it matters |
|---|---|---|
| US Dollar Index | DXY | A stronger dollar usually weighs on gold |
| US 10Y Treasury yield | US10Y | Opportunity cost of holding a non-yielding asset |
| Volatility Index | VIX | Market risk aversion ("safe haven" demand) |

## What the pipeline does

1. **Data ingestion** – downloads daily prices from Stooq with an automatic fallback to Yahoo Finance, handles HTML/limit responses and logs which source was actually used.
2. **Data preparation** – joins the series, handles missing values, optional weekly aggregation, and transforms levels into log-returns / differences.
3. **Statistical checks** – ADF stationarity tests, correlation matrix, multicollinearity (VIF).
4. **Modelling** – OLS regression with heteroskedasticity-robust (HC1) standard errors and 95% confidence intervals.
5. **Diagnostics** – Jarque-Bera, Breusch-Pagan, Durbin-Watson, runs test, residual plots, QQ-plot, ACF.
6. **Validation** – out-of-sample test set (RMSE, MAE, correlation, directional hit rate) plus **expanding-window walk-forward validation**.
7. **Forecasting** – factors forecast with `auto.arima`, then fed into the regression to build a scenario path for the gold price with uncertainty bands.
8. **Outputs** – all datasets, the fitted model (`.rds`), forecasts (`.csv`) and a Markdown summary are saved to disk.

## Tech stack

R · R Markdown · `dplyr` · `ggplot2` · `quantmod` · `forecast` · `lmtest` · `sandwich` · `tseries`

## How to run

1. Install R and RStudio.
2. Install the packages listed at the top of `gold_macro_model.Rmd` (the script tells you exactly which ones are missing).
3. Open `gold_macro_model.Rmd` and click **Knit** (PDF or HTML).

Parameters such as the date range, frequency (`daily` / `weekly`), test-set size and forecast horizon can be changed in the YAML header.

Generated files appear in `data_out/` and `output_out/`.

## Notes

- The report text and chart labels are in Polish (university project).
- The forecast is a statistical scenario, not investment advice.

## Authors

Marcin Ossoliński, Maksymilian Rak – Rzeszów University of Technology
