# PCA Portfolios in Risk Regimes

### Can regime awareness make principal portfolios investable and profitable?

## Overview

This project studies whether principal components extracted from a panel of US tech equities can be treated as investable equity factors, and whether their risk-adjusted performance changes across volatility regimes.

The framework combines:

* **Linear factor extraction:** correlation-PCA on daily equity returns
* **Factor portfolio construction:** factor-mimicking principal portfolios
* **Regime conditioning:** LOW / MID / HIGH states defined from an implied-volatility proxy
* **Robust estimation:** flexible probabilities, shrinkage covariance, and walk-forward re-estimation
* **Validation:** Politis–Romano stationary block bootstrap
* **Implementability checks:** turnover, loading stability, and spread-based transaction-cost proxies

## Research Question

Do higher-order principal portfolios become attractive in specific risk regimes, or does a simple buy-and-hold exposure to **PC1** remain the most robust investable choice once robustness and implementation are taken seriously?

\---

## Main Findings

### 1\) PC1 is the strongest baseline exposure

Across bootstrap validation and walk-forward testing, **PC1** emerges as the most robust buy-and-hold principal portfolio.

In the current bootstrap validation (**N = 1000**), PC1 delivers approximately:

* **42.6% median gross CAGR**
* **1.59 median Sharpe ratio**
* **7.22 median Martin ratio**

### 2\) Higher-order PCs are more regime-dependent

Higher PCs can look attractive in selected regimes, especially on conditional risk-adjusted metrics, but they are materially less stable through time.

### 3\) Stability and implementability matter

Walk-forward PCA shows a clear trade-off:

* **PC1:** more stable loadings and lower turnover
* **Higher PCs:** noisier rebalances and larger turnover spikes

This means that apparent performance improvements in higher PCs are harder to monetize in practice.

### 4\) The core result survives more realistic evaluation choices

The project now evaluates performance:

* against a **historical risk-free proxy** (used for Sharpe, Sortino, Martin, and excess-return metrics)
* net of **Corwin–Schultz spread-based implementation costs**

The main conclusion remains unchanged: **PC1 is still the most robust investable exposure**, while higher PCs remain more fragile and regime-sensitive.

\---

## Methodology

### Universe and frequency

* **Universe:** US tech equities
* **Frequency:** daily returns

### Factor construction

* Correlation-based PCA on the equity return panel
* Conversion of eigenvectors into **factor-mimicking principal portfolios**
* Walk-forward re-estimation to avoid static in-sample weights

### Regime definition

* Regimes classified as **LOW / MID / HIGH** using an implied-volatility state variable

### Risk and performance metrics

* CAGR
* Excess CAGR
* Sharpe ratio
* Sortino ratio
* Calmar ratio
* Martin ratio
* Max drawdown
* Turnover
* Loading stability / cosine similarity

### Validation

* **Politis–Romano stationary block bootstrap**
* **N = 1000** simulations
* Dependency-aware resampling to assess robustness beyond the realized path

### Implementation-cost layer

* Asset-level **Corwin–Schultz** bid–ask spread estimation from daily high/low data
* Conversion from full spread to **one-way spread-based cost proxy**
* Aggregation of costs through the turnover of the underlying stock weights of each PC portfolio

\---

## Selected Results

### Bootstrap validation

For **PC1 (overall, gross)**, the stationary block bootstrap indicates strong and persistent performance, with median values around:

|Metric|Median|
|-|-:|
|CAGR|42.6%|
|Sharpe|1.59|
|Martin|7.22|

This supports the view that the strongest principal portfolio is not just an in-sample artifact.

### Walk-forward results

In walk-forward testing, **PC1** remains the cleanest investable baseline:

* about **42.0% gross CAGR** overall
* about **35.7% excess CAGR** overall
* about **1.55 Sharpe** overall

Spread-based costs modestly reduce performance, but do **not** overturn the ranking of PC1 as the most robust principal portfolio.

\---

## Interpretation

This project is **not** a claim that higher PCs are useless.

Rather, it shows that:

* higher PCs can carry useful regime information,
* but their attractiveness is conditional,
* and the jump from “interesting backtest” to “investable factor” depends heavily on stability and turnover.

In that sense, the project sits at the intersection of:

* quantitative risk modelling,
* factor investing,
* and implementation-aware portfolio construction.

\---

## Strategy Implications

### Natural benchmark

* **Buy \& Hold PC1**

### Extensions under study

* Regime-based rotation across principal portfolios
* Cross-country or cross-universe PCA rotation
* Risk overlays and portfolio constraints
* Richer execution-cost modelling beyond spread-only proxies

\---

## Limitations

* Corwin–Schultz costs should be interpreted as **spread-based implementation proxies**, not a full execution-cost model
* Regime-conditional sub-sample metrics are informative diagnostically, but not all of them correspond directly to continuous investable strategies
* Results remain sample-dependent and should be interpreted jointly with walk-forward and bootstrap evidence

\---

## Repository Structure

* `PCA_Portfolios_in_Risk_Regimes.ipynb` — main notebook
* `requirements.txt`
* `README.md`

\---

## Author

**Saverio Lauriola**

