# Macroeconomic Determinants of Startup Mortality in India 

## Overview
This repository contains the R scripts and quantitative framework used to model the survival probability of 218 venture-backed startups in India[cite: 2]. Using a multivariate **Cox Proportional Hazards model**, this analysis tests how macroeconomic shocks—specifically changes in Real GDP Growth, CPI Inflation, and the RBI Repo Rate—affect startup mortality rates while controlling for right-censored active firms[cite: 2].

## Key Quantitative Findings
* **The Dominance of Capital Costs:** The model confirmed that the RBI Repo Rate is the primary hazard driver for venture-backed startups[cite: 2]. While domestic GDP growth offers a marginal protective effect, monetary tightening overwhelmingly dictates survival outcomes[cite: 2]. CPI inflation was found to be statistically irrelevant[cite: 2].
* **Proof of Sector Agnosticism:** Contrary to traditional assumptions regarding industry-specific resilience, a Likelihood Ratio Test (p = 0.4601) failed to reject the null hypothesis that sector coefficients are jointly zero[cite: 2]. Macroeconomic capital shocks act as an indiscriminate systematic threat across all mega-sectors (Consumer Tech, FinTech, EdTech, Enterprise/SaaS)[cite: 2].
* **The 2022 Funding Winter Structural Break:** By engineering a time-based dummy interaction term, the model empirically proved that the 2022 funding contraction was not merely cyclical, but a structural break[cite: 2]. Post-2022, the ecosystem's sensitivity to interest rate fluctuations increased dramatically (coefficient increased by 168.966, p < 0.001)[cite: 2].

## Methodology & Diagnostics
To ensure the statistical integrity of the survival model, several robust methodological adaptations were implemented:
* **Multicollinearity Checks:** Variance Inflation Factors (VIF) were extracted via OLS projection to bypass standard error conflicts inherent in survival objects[cite: 2]. All continuous variables scored well below the 5.0 threshold[cite: 2].
* **Proportional Hazards Testing:** Schoenfeld residual plots confirmed the core assumption that hazard ratios remained constant over time (Global p-value = 0.627)[cite: 2].
* **Residual Analysis:** Martingale and Deviance residuals were plotted to verify linear functional form specifications and rule out model-breaking outliers[cite: 2].

## Tech Stack
* **Language:** R
* **Core Libraries:** `survival`, `survminer`, `tidyverse`, `car`, `readxl`
* **Statistical Methods:** Cox Proportional Hazards Regression, Kaplan-Meier Estimators, Likelihood Ratio Tests, OLS Projections, Right-Censoring Treatment
