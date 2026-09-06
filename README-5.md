# Macroeconomic Determinants of Startup Mortality in India

A multivariate Cox Proportional Hazards survival analysis modeling how macroeconomic shocks affect the mortality risk of venture-backed startups in India.

## Overview

This repository contains the R scripts and quantitative framework used to model the survival probability of 218 venture-backed startups in India over a 15-year period. Using a multivariate **Cox Proportional Hazards model**, the analysis tests how macroeconomic shocks — specifically changes in Real GDP Growth, CPI Inflation, and the RBI Repo Rate — affect startup mortality rates, while controlling for right-censored active firms (firms still operating at the end of the observation window, whose "survival time" is only a lower bound).

## Data

- **Sample:** 218 venture-backed startups, spanning a 15-year period
- **Sectors covered:** Consumer Tech, FinTech, EdTech, Enterprise/SaaS
- **Macroeconomic covariates:** Real GDP Growth, CPI Inflation, RBI Repo Rate

## Key Quantitative Findings

| Finding | Result |
|---|---|
| Primary hazard driver | RBI Repo Rate — monetary tightening dominates survival outcomes |
| GDP growth effect | Marginal protective effect only |
| CPI inflation effect | Statistically insignificant |
| Sector-specific resilience | Not supported — Likelihood Ratio Test (p = 0.4601) fails to reject that sector coefficients are jointly zero |
| 2022 funding winter | Structural break, not merely cyclical (see below) |

**The dominance of capital costs.** The model finds that the RBI Repo Rate is the primary hazard driver for venture-backed startups. While domestic GDP growth offers a marginal protective effect, monetary tightening overwhelmingly dictates survival outcomes. CPI inflation was found to be statistically irrelevant once repo rate and GDP growth are controlled for.

**Sector agnosticism.** Contrary to the assumption that some sectors are more resilient to macro shocks than others, a Likelihood Ratio Test (p = 0.4601) fails to reject the null hypothesis that sector coefficients are jointly zero. This suggests macroeconomic capital shocks act as an indiscriminate, systematic threat across all mega-sectors (Consumer Tech, FinTech, EdTech, Enterprise/SaaS) rather than hitting some harder than others.

**The 2022 funding winter as a structural break.** By engineering a time-based dummy interaction term, the model tests whether the 2022 funding contraction was a temporary cyclical dip or a lasting shift in how the ecosystem responds to rates. Post-2022, sensitivity to interest rate fluctuations increased sharply (interaction coefficient: 168.966, p < 0.001)[^1], consistent with a structural break rather than a cyclical one.

[^1]: **Note:** this coefficient value is unusually large for a standard Cox PH log-hazard coefficient and likely reflects either a hazard ratio (exp(coefficient)) or a scaling artifact in one of the interacted variables (e.g. repo rate coded as a raw percentage rather than a decimal). Worth double-checking against the model output directly before citing this figure in an interview — an unusually large coefficient is often the first thing a reviewer with survival-analysis experience will probe.

## Methodology & Diagnostics

- **Multicollinearity checks.** Variance Inflation Factors (VIF) were extracted via OLS projection — a workaround for the fact that standard VIF functions don't work directly on `coxph` survival objects. All continuous variables scored well below the conventional 5.0 threshold.
- **Proportional hazards assumption.** Schoenfeld residual plots confirmed that hazard ratios remained constant over time (Global test p-value = 0.627), supporting the core PH assumption the model depends on.
- **Residual analysis.** Martingale and Deviance residuals were plotted to check the functional form of continuous covariates and rule out model-breaking outliers.
- **Censoring.** Firms still active at the end of the observation window are treated as right-censored rather than dropped, avoiding survivorship bias.

## Tech Stack

- **Language:** R
- **Core libraries:** `survival`, `survminer`, `tidyverse`, `car`, `readxl`
- **Statistical methods:** Cox Proportional Hazards Regression, Kaplan–Meier Estimators, Likelihood Ratio Tests, OLS Projections (for VIF), Right-Censoring Treatment

## How to Run

```r
install.packages(c("survival", "survminer", "tidyverse", "car", "readxl"))
```

Open the analysis script in R or RStudio and run it top to bottom against the underlying dataset.

## Limitations

- **Sample size and period.** 218 firms is a meaningful sample for a Cox model but still limited relative to the full universe of Indian venture-backed startups; results should be read as indicative of directional effects rather than precise population-level hazard ratios.
- **Right-censoring.** Firms still active at the end of the window contribute only partial information: we know they survived at least that long, not their eventual outcome.
- **Macro variables are national-level.** Repo rate, GDP growth, and CPI inflation are economy-wide series, not firm- or sector-specific exposures, so the model can't distinguish firms with different sensitivities to the same macro shock beyond the sector and interaction terms tested.
- **The 2022 structural break test relies on a single dummy interaction.** It confirms a shift in sensitivity around that date but doesn't pin down the exact mechanism (funding supply vs. investor risk appetite vs. valuation resets).

## Possible Extensions

- Time-varying covariates for firm-level metrics (funding stage, burn rate) rather than macro variables alone
- Stratified Cox models by sector, now that sector coefficients are shown to be jointly insignificant, to check whether that holds under a stratified rather than pooled specification
- Extending the structural break test with additional breakpoints (e.g. COVID-19 onset) for comparison
