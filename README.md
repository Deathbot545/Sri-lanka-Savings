# Protecting household savings in Sri Lanka

What happens to Rs. 100,000 set aside for a one-year expense if a household puts it into a fixed deposit, buys Colombo-listed shares, or divides it between the two? This coursework project measures the purchasing power left at maturity and investigates the price risk of needing to sell shares early.

[Open the executed notebook](Sri_Lanka_Savings_Expanded.ipynb) · [Read the analysis and report guide](REPORT_GUIDE.md)

## Results at a glance

| Historical question | Result |
|---|---|
| How did a broad equity investment fare during 2022? | Rs. 100,000 ended at Rs. 69,440 including reinvested dividends, equivalent to Rs. 43,618 in opening-date purchasing power. |
| Did a one-year FD preserve purchasing power during the same crisis? | Across the published opening rates of 1.5%–10%, real ending wealth was Rs. 63,756–69,095. |
| How widely did shares beat deposits from February 2025 to February 2026? | 78.8% of 255 securities traded at both endpoints exceeded the 12% upper FD hurdle on price change alone. All securities share one holding period. |
| Could recent trading data flag near-term losses? | A shallow decision tree reduced test Brier score from 0.0737 to 0.0699. At the validation-set alert threshold it detected 30% of subsequent loss events. |

## Data and methods

The public equity panel contains 71,044 security-day records, 289 security symbols and 257 market dates. The notebook checks the CSV checksum, distinguishes untraded quotes from missing security-days, and builds features on the common market calendar. The 2022 comparison uses the All Share Total Return Index. The later comparison uses individual closing prices and excludes dividends, trading fees and taxes. Deposit scenarios assume the quoted rate is available and the deposit is held to maturity.

The classifier estimates the probability that a share closes at least 5% lower five market sessions later. Logistic regression and a decision tree are evaluated on later dates against a constant training-frequency forecast. Five-session gaps prevent forward outcomes from crossing evaluation boundaries.

## Run locally

Install Python 3 and run these commands from the directory containing the notebook.

```bash
python -m pip install jupyterlab pandas numpy matplotlib scikit-learn pymupdf
python -m jupyterlab Sri_Lanka_Savings_Expanded.ipynb
```

Select **Restart Kernel and Run All Cells**. The first run downloads the pinned stock CSV to the working directory. For offline execution, place the exact file there as `stock_prices.csv` beforehand. The notebook extracts ASTRI and inflation from SEC/DCS PDFs and caches them under `benchmark_sources/`. It reads deposit rates by period and tenor from the two machine-extracted CBSL files included in that directory. CBSL blocked direct PDF downloads during verification. Keep both files beside the notebook in their supplied folder. Amounts, portfolio weights and event thresholds are scenario or modelling choices.

## Run in Colab

Open the notebook and upload `Sri_Lanka_Savings_Updated.zip` via the left Files panel. Run the following in a temporary setup cell, then execute the notebook in order in the same runtime.

```python
!unzip -oq Sri_Lanka_Savings_Updated.zip -d /content/savings_project
%cd /content/savings_project
%pip install pymupdf
```

After a runtime reset, repeat setup. If you clone this repository into Colab, change into the repository directory instead. If a publisher blocks the download, place its original PDF in `benchmark_sources/` under the filename from the source URL. Python 3.10 or newer is required. GitHub can display the saved results without executing Python.

## Sources and attribution

- Jayawardana and colleagues, *CSE Market Data — Cyclone Ditwah Event Study*. The publisher licenses the dataset under CC BY 4.0. [Pinned stock dataset and attribution](https://huggingface.co/datasets/tharu-jwd/cse-market-data/tree/669594502b85772c7b7c853067afde91402e3c2a)
- Securities and Exchange Commission of Sri Lanka, year-end total-return index levels. [Annual Report 2024, p. 75](https://www.sec.gov.lk/wp-content/uploads/2025/07/SEC-AR-2024.pdf)
- Colombo Stock Exchange, definition and dividend treatment of the index. [Index methodology](https://cdn.cse.lk/pdf/Index-Methodologies-of-Colombo-Stock-Exchanhge.pdf)
- Central Bank of Sri Lanka, commercial-bank one-year deposit ranges. [December 2021 rates in the 2022 statistical appendix](https://www.cbsl.gov.lk/sites/default/files/cbslweb_documents/publications/annual_report/2022/en/15_Appendix.pdf) · [February 2025 rates in Weekly Economic Indicators](https://www.cbsl.gov.lk/sites/default/files/cbslweb_documents/statistics/wei/WEI_20250417_e.pdf)
- National purchasing-power measures. [CBSL December 2022 inflation release](https://www.cbsl.gov.lk/en/inflation-in-december-2022-ncpi) · [Department of Census and Statistics February 2026 release](https://www.statistics.gov.lk/Resource/en/InflationAndPrices/NCPI/NCPI_20260323E.pdf)

The two investment periods use different equity measures. They support historical scenarios and a discussion of early-sale risk. Estimating the probability of annual market outperformance across economic cycles would require a longer matched history.
