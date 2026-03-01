# PCA Portfolios in Risk Regimes
### Can regime awareness make principal portfolios investable and profitable?

## Overview

This project investigates whether principal components (PCs) extracted from US Tech equities can be exploited as investable factors, and whether their performance varies across market regimes.

It combines:
- Quantitative risk modelling techniques (state conditioning, shrinkage covariance, dependency-aware bootstrap)
- Portfolio construction and evaluation (factor-mimicking portfolios, turnover, stability, implementability)

Core question:
Can regime-dependent behavior of PCA factors support a dynamic allocation strategy, or is a simple buy-and-hold exposure to PC1 optimal?

---

## Key Findings

### PC1 dominance
- Strongest CAGR and risk-adjusted performance
- Most stable across rebalances
- Natural baseline strategy

### Higher-order PCs
- Improve Sharpe, Sortino, Calmar, Martin in HIGH regimes
- No improvement in CAGR
- Strong regime dependency

### Trade-off
- Lower stability (cosine similarity)
- Higher turnover
→ Transaction costs become critical

---

## Methodology

- Universe: US Tech equities
- Frequency: Daily
- PCA → factor-mimicking portfolios
- Regimes: LOW / MID / HIGH (volatility-based)

Metrics:
- CAGR
- Sharpe
- Sortino
- Calmar
- Martin
- Max Drawdown
- Turnover
- Cosine similarity

---

## Bootstrap Validation

Politis–Romano stationary block bootstrap:
- N = 1000 simulations
- Block length = 39

PC1 (overall):

| Metric | p05 | Median | p95 |
|--------|-----|--------|-----|
| CAGR   | 25.1% | 47.0% | 71.3% |
| Sharpe | 1.11 | 1.95 | 2.83 |
| Martin | 3.10 | 10.22 | 23.40 |
| MaxDD  | -28.2% | -18.9% | -10.8% |

---

## Stability & Turnover

Walk-forward PCA shows:

PC1:
- Stable
- Low turnover

Higher PCs:
- Unstable
- High turnover spikes

Key insight:
Higher performance ≠ implementable

---

## Strategy Implications

Baseline:
Buy & Hold PC1

Extensions:
- Regime-based PC rotation (US)
- Cross-country PCA rotation

---

## Roadmap

- State variable refinement
- Transaction costs
- Improved regime definition
- Cost-aware rotation
- Multi-country extension
- Risk overlays

---

## Structure

- PCA Portfolios in Risk Regimes_v1.ipynb
- requirements.txt
- README.md

---

## Author

Saverio Lauriola
