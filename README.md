# regime-aware-core-satellite

Regime-aware core-satellite allocation on an investable PCA equity core, combining ML-based stress regimes, portfolio-aware multi-asset satellite screening, and non-linear de-risking.

---

## 1. What this repository is about

`regime-aware-core-satellite` studies **investable PCA “principal portfolios”** on a US technology equity universe and develops a **regime-aware allocation framework** around the dominant principal portfolio (**PC1**).

The project starts from a simple empirical observation:

- the dominant investable principal portfolio (**PC1**) can be very strong in benign states,
- but its behaviour deteriorates materially in stressed states,
- so the relevant problem is not only **how to build PCA portfolios**, but also **when to keep the PCA core on** and **when to de-risk it**.

The current framework addresses that problem by combining:

1. **ML-based stress-regime identification** from implied-volatility state variables,
2. a **continuous stress-intensity signal**,
3. **portfolio-aware screening** of multi-asset satellite candidates,
4. construction of a **tangency satellite**,
5. and **non-linear de-risking rules** from the PCA core into the satellite.

The objective is not pure return maximization. The goal is to improve the **risk-adjusted profile** and **drawdown behaviour** of an investable PCA equity core under stress while keeping the allocation logic explicit, systematic, and testable out of sample.

---

## 2. Research question

The central question is:

> **When does an investable PCA equity core remain worth holding, and when does it make sense to de-risk into a multi-asset satellite?**

That question is broken into four sub-questions:

1. **Principal portfolios under regimes**  
   How do investable PCA portfolios behave across different stress states?

2. **State identification**  
   Which stress variables are informative enough to define regimes and a usable continuous stress score?

3. **Satellite design**  
   Which non-equity / multi-asset candidates actually help the portfolio in a **portfolio-aware** sense?

4. **Allocation rule**  
   Is a linear mapping from stress to allocation enough, or does the data favour **non-linear de-risking**?

---

## 3. Current high-level answer

The current evidence points to the following story:

- **PC1 works well in benign states**, but should be partially de-risked under stress.
- A **multi-asset tangency satellite** selected in a portfolio-aware manner helps improve the **risk-adjusted profile**.
- The useful non-linearity appears to be mainly in the **outer allocation** between the **PC1 core** and the **satellite**, rather than in an inner tangency-to-risk-free rotation.
- The project does **not** claim dominance in raw CAGR; instead, it appears to improve **Sharpe**, **Martin**, and **drawdown control** at the cost of some CAGR drag.

---

## 4. Repository structure

A README that does not explain the structure is useless, so here is the practical map.

> Actual filenames may evolve, but the logical structure of the repository is:

```text
regime-aware-core-satellite/
│
├── notebooks/
│   ├── data_download_and_cleaning.ipynb
│   ├── pca_portfolios_construction.ipynb
│   ├── regime_definition_and_stress_signal.ipynb
│   ├── satellite_screening.ipynb
│   ├── core_satellite_allocation.ipynb
│   ├── validation_and_frozen_oos.ipynb
│   └── bootstrap_inference.ipynb
│
├── src/
│   ├── data/
│   │   ├── loaders.py
│   │   └── cleaning.py
│   ├── pca/
│   │   ├── principal_portfolios.py
│   │   └── investable_weights.py
│   ├── regimes/
│   │   ├── iv_state.py
│   │   ├── regime_selection.py
│   │   └── stress_signal.py
│   ├── satellite/
│   │   ├── screening.py
│   │   ├── tangency.py
│   │   └── allocation.py
│   ├── costs/
│   │   └── corwin_schultz.py
│   ├── metrics/
│   │   ├── performance.py
│   │   └── bootstrap.py
│   └── utils/
│       └── plotting.py
│
├── figures/
├── results/
├── README.md
└── requirements.txt
```

Even if the code is still notebook-heavy, this is the conceptual layout the project follows.

---

## 5. Data and investable universe

### 5.1 Equity core universe
The PCA core is built on a **US technology equity universe**.  
The exact list may evolve, but the intent is stable:

- large, liquid US technology names,
- sufficiently long history,
- investable universe suitable for PCA-based portfolio construction.

### 5.2 Multi-asset satellite universe
The satellite is built from a broader multi-asset candidate set, designed to contain:

- nominal duration,
- inflation protection,
- credit,
- real assets,
- defensive FX / diversifying sleeves,
- selective non-core risk sleeves.

Typical categories explored include:

- Treasuries across the curve,
- TIPS,
- IG / HY credit,
- gold,
- broad commodities,
- USD defensive sleeves,
- REITs,
- developed ex-US and EM equity sleeves,
- defensive equity sleeves.

### 5.3 Risk-free / cash-like sleeve
A risk-free or cash-like sleeve is retained for benchmarking and for some allocation-architecture tests, but it is **not always treated as part of the risky satellite screening universe**.

---

## 6. Methodology

### 6.1 Investable PCA equity core
The project constructs **investable principal portfolios** from the equity universe rather than using PCA purely as a statistical diagnostic.

The dominant portfolio, **PC1**, is treated as the **core risk sleeve**.

The important point is that PCA is not the endpoint here.  
It is the starting object around which the allocation framework is built.

### 6.2 ML-based stress regimes
Stress states are inferred from **implied-volatility-based state variables**.

The current logic uses:

- IV-derived state scores,
- regime selection procedures,
- and a **continuous stress-intensity signal** built from IV information.

This stress signal is then used in allocation rules, rather than relying only on hard state labels.

The intent is to move from:

- “regime classification”  
to
- “regime-aware continuous allocation”.

### 6.3 Portfolio-aware satellite screening
Satellite selection is **not** treated as an asset-by-asset beauty contest.

Instead, the screening is done in a **portfolio-aware** way:

- candidate sleeves are assessed in the context of the total allocation problem,
- portfolio interactions matter,
- the goal is not simply “lowest beta” or “lowest volatility”,
- but rather **which sleeves help the total portfolio under stress**.

This matters because many “defensive” assets look similar in isolation but contribute differently once embedded in a core-satellite structure.

### 6.4 Tangency satellite
From the retained candidate set, a **tangency satellite** is built.

The tangency satellite is currently the most useful retained satellite architecture:

- it captures the best risk-adjusted mix from the screened candidate set,
- and it has repeatedly outperformed more complicated internal tangency-to-risk-free branches in the current experimental setup.

### 6.5 Non-linear de-risking
One of the main findings of the project is that a **linear mapping from stress to allocation is not obviously optimal**.

The current retained result is based on **non-linear outer de-risking** between:

- the **PC1 core**
- and the **tangency satellite**

That means the satellite can enter **earlier** or **more slowly** than a linear rule would imply, depending on the calibrated curvature.

This is one of the most important takeaways of the project so far.

### 6.6 Costs and inference
The project does not stop at gross backtests.

It includes:

- **Corwin–Schultz transaction-cost adjustments**
- **frozen out-of-sample tests**
- **paired stationary-bootstrap inference**

So the goal is not just “nice-looking curves”, but a more defensible statement about whether the allocation architecture improves the risk-adjusted profile of the static core.

---

## 7. Experimental pipeline

The current workflow is:

1. **Build the investable PCA equity core**
2. **Define stress regimes / stress signal**
3. **Study regime-dependent behaviour of principal portfolios**
4. **Screen multi-asset candidates**
5. **Build the tangency satellite**
6. **Test alternative allocation architectures**
   - linear core-satellite
   - non-linear outer core-satellite
   - tangency-to-risk-free variants
   - fully combined variants
7. **Select candidate architectures on validation**
8. **Freeze and evaluate in final OOS**
9. **Assess differences with bootstrap**

That is the real logic of the repository.

---

## 8. Current retained specification

The current retained specification is:

- **PC1 as static core**
- **Tangency satellite**
- **Continuous IV-based stress-intensity signal**
- **Non-linear outer allocation** from core to satellite
- No evidence so far that a more complicated inner tangency-to-risk-free branch dominates this simpler structure

The current best retained specification indicates:

- improved **Sharpe**
- improved **Martin**
- improved **drawdown control**
- but with **moderate CAGR drag**

That is an allocator / risk-aware result, not a pure return-maximization result.

---

## 9. Current headline result

In the current retained frozen-OOS specification, the regime-aware allocation improves the static core’s risk-adjusted profile net of costs:

- **Sharpe:** +0.07
- **Martin:** +0.16
- **MaxDD:** +7.4 percentage points improvement
- **CAGR drag:** about 1.4 percentage points

Interpretation:

- the framework appears useful for **risk-adjusted de-risking**
- not for claiming raw-return dominance

This is exactly the kind of trade-off many portfolio-construction frameworks are supposed to solve.

---

## 10. What this project is — and is not

### It is:
- a **regime-aware portfolio-construction framework**
- a **systematic allocation architecture**
- a **risk-aware de-risking engine** around an investable PCA core
- a research project about **when and how to reduce PCA core exposure**

### It is not:
- a pure alpha-signal repository
- a claim of unconditional outperformance on CAGR
- a production system
- investment advice

---

## 11. Relationship to other repositories

This project is related to, but distinct from, the dynamic overlays work.

The **dynamic overlays** repository focuses on a narrower and more specific branch:

- probabilistic overlay design,
- dynamic risk overlays,
- and tactical de-risking signals layered onto principal portfolios.

By contrast, **regime-aware-core-satellite** is the broader portfolio-construction repository for:

- stress regimes,
- regime-dependent principal-portfolio behaviour,
- satellite screening,
- core-satellite architecture,
- and non-linear de-risking.

In short:

- **dynamic overlays** = specialised overlay branch
- **regime-aware-core-satellite** = broader regime-aware allocation framework

---

## 12. Why the project matters

This project matters because it sits at the intersection of:

- PCA / statistical portfolio construction,
- regime analysis,
- multi-asset allocation,
- and practical de-risking.

It shows that a PCA equity core does not need to be treated as a static object.  
Its exposure can be managed systematically as market stress evolves.

That makes the project relevant not only for “quant research” audiences, but also for:

- portfolio construction,
- systematic investing,
- risk-aware allocation,
- and AM / portfolio-risk contexts.

---

## 13. Limitations

This repository is still an active research project, so the limitations matter.

Main limitations include:

- sensitivity to regime specification,
- sensitivity to satellite universe design,
- trade-off between CAGR and risk control,
- potential instability if too much architectural complexity is added,
- the need to balance validation gains against frozen-OOS robustness.

Those limitations are part of the project, not something to hide.

---

## 14. Next steps

The main next steps are:

- refine the retained non-linear outer allocation region,
- test whether the selected curvature is locally robust,
- keep stress-regime design as simple as possible unless extra complexity clearly earns its place,
- improve repo packaging and code modularity,
- and freeze a final research version suitable for cleaner reporting.

---

## 15. If you want to reproduce the work

The expected workflow is:

1. download and clean market data,
2. build the PCA / principal portfolios,
3. construct stress-state variables,
4. generate regimes and continuous stress scores,
5. screen satellite candidates,
6. build the tangency satellite,
7. run validation and frozen-OOS allocation tests,
8. evaluate with bootstrap-based paired inference.

As the repository is cleaned further, a more explicit execution guide will be added.

---

## 16. Short description (for GitHub “About”)

**Regime-aware core-satellite allocation on an investable PCA equity core, combining ML-based stress regimes, portfolio-aware multi-asset satellite screening, and non-linear de-risking.**
