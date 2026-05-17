# regime-aware-core-satellite

## Overview

`regime-aware-core-satellite` studies investable PCA “principal portfolios” on US technology equities and develops a regime-aware allocation framework around the dominant principal portfolio (PC1).

The project combines:

- ML-based stress-regime identification from implied-volatility state variables,
- a continuous IV-based stress-intensity signal,
- portfolio-aware screening of multi-asset satellite candidates,
- tangency-satellite construction,
- and validation-selected non-linear de-risking from the PCA core into the satellite.

The objective is not pure return maximization. The goal is to improve the risk-adjusted profile of an investable PCA equity core under stressed market states while keeping the allocation logic explicit, systematic, and testable out of sample.

## Research question

The central question is:

> when does the investable PCA equity core remain worth holding, and when does it make sense to de-risk into a multi-asset satellite?

Rather than treating PCA portfolios as static objects, this repository studies how their behaviour changes across stress regimes and how those state changes can be turned into systematic allocation rules.

## Current framework

The current architecture is:

1. **Investable PCA equity core**
   - Build principal portfolios from US technology equities.
   - Focus on the dominant principal portfolio (**PC1**) as the equity core.

2. **Stress regimes**
   - Use ML-based regime identification built on implied-volatility state variables.
   - Construct a continuous **stress-intensity** signal from IV information.

3. **Satellite design**
   - Screen a multi-asset universe in a portfolio-aware way rather than asset-by-asset.
   - Build a **tangency satellite** from the retained subset.

4. **Allocation rule**
   - Allocate between the static PC1 core and the tangency satellite.
   - Use validation-selected **non-linear de-risking** so the satellite can enter faster or slower than a linear mapping would imply.

## Why this repository exists

This repository is the broader home for the regime-aware allocation side of the project:

- principal portfolios under stress regimes,
- regime-conditioned behaviour of PC portfolios,
- satellite construction,
- and non-linear core-satellite allocation.

It is meant to answer a broader portfolio-construction question than a pure overlay notebook or a pure signal notebook.

## Methodological highlights

- Investable PCA / principal portfolios
- ML-based stress-regime identification
- Continuous IV-based stress intensity
- Portfolio-aware multi-asset screening
- Tangency-satellite construction
- Non-linear core-satellite allocation
- Validation-window model selection
- Frozen out-of-sample testing
- Paired stationary-bootstrap inference
- Net performance after Corwin–Schultz transaction-cost adjustments

## Current headline result

In the current retained frozen-OOS specification, the selected regime-aware core-satellite rule improves the risk-adjusted profile of the static PC1 core:

- **Sharpe:** +0.07
- **Martin:** +0.16
- **Max drawdown:** +7.4 percentage points improvement
- **CAGR drag:** about 1.4 percentage points

This is not an “alpha miracle” result. The project’s value is that it appears to deliver a cleaner **risk-adjusted** and **drawdown-controlled** allocation profile than the static core, at the cost of some CAGR.

## Repository status

This is an active research repository.

The main open questions are not whether the framework “works at all”, but:

- how robust the selected non-linearity is,
- whether stress regimes can be refined further without over-complicating the signal,
- whether the satellite universe should evolve further,
- and how much complexity is actually rewarded out of sample.

## What is inside

Typical components in this repository include:

- data download / preparation notebooks,
- PCA / investable principal-portfolio construction,
- regime-definition and stress-signal notebooks,
- multi-asset satellite screening,
- core-satellite allocation tests,
- validation / frozen-OOS evaluation,
- paired stationary-bootstrap inference.

## Interpretation

The project should be read as a **systematic portfolio-construction and de-risking framework**, not as a pure alpha strategy.

The main contribution is the combination of:

- an investable PCA equity core,
- explicit stress-state conditioning,
- and a portfolio-aware satellite / allocation design.

## Caveats

- Results are sensitive to regime specification, satellite universe design, and non-linearity calibration.
- The project trades some CAGR for improved risk-adjusted behaviour.
- This repository is research code, not production investment advice.

## Roadmap

Likely next steps include:

- robustness checks around the selected non-linear allocation region,
- further refinement of the stress signal,
- better separation between “stress but compensated” and “stress and toxic” states,
- and cleaner packaging of final retained specifications.
