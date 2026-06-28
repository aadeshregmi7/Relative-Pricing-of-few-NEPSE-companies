# Relative Pricing & Valuation Engine

A quantitative research and relative valuation engine engineered to analyze equity asset classes in the Nepalese Capital Market (NEPSE). This module automates the end-to-end workflow of financial data ingestion, programmatic normalization, and peer-group multiple benchmarking across three foundational market sectors: Commercial Banking, Hydropower, and Life Insurance.

---

## 1. Module Architecture & Components


Relative Pricing/
├── bank_data.csv             # Structured cache for Commercial Banking data
├── scrap.ipynb               # Sandbox environment for DOM layout prototyping
└── analysis/                 # Quantitative execution layer
    ├── bank_analysis.ipynb   # Banking sector multiples & Graham boundary models
    ├── hydro_analysis.ipynb  # Capital structure & asset-heavy operational analytics
    └── life_insurance.ipynb  # Underwriting performance & premium-to-valuation proxies

## 2. Core Valuation Theory & Methodology

This submodule implements a defensive financial analytics framework tailored for emerging equity landscapes (such as NEPSE). It cross-references **Cross-Sectional Relative Pricing Multiples** with **Absolute Intrinsic Value Limits** to identify market mispricings while preserving a true margin of safety.

### a. Cross-Sectional Relative Pricing
Relative valuation relies on the *Law of One Price*, standardizing company market valuations against a specific financial denominator (Earnings, Book Value, or Operating Cash Flow).

* **Dynamic Sector Medians ($M_{\text{sector}}$):** Rather than evaluating companies against a rigid, historical benchmark (such as a static P/E of 15), this engine derives rolling benchmarks directly from live peer listings.
* **Outlier Resilience:** Standard averages (means) are easily distorted by extreme corporate events or speculative bubbles. The baseline uses the **statistical median**, pinpointing the true structural midpoint of the peer group.

### b. Domain-Specific Multiples Selection
Value drivers vary significantly by business model, requiring customized scaling metrics per sector:
* **Commercial Banking (`P/E`, `P/B`):** Evaluates equity value relative to high-liquidity financial statements, asset quality, and statutory regulatory capital requirements.
* **Hydropower (`EV/EBITDA`, `Debt-to-Equity`):** Targets capital-heavy infrastructure assets. It bypasses massive non-cash depreciation distortions to isolate operational cash generation relative to debt leverage.
* **Life Insurance (`P/E`, `Price-to-Premium`):** Measures market premium against underwriting momentum and the scale of the company's premium investment float.

### c. Absolute Intrinsic Limit: The Graham Blended Model
To protect the portfolio if an entire sector becomes irrationally overvalued (inflating the sector median), the engine uses Benjamin Graham’s classic defensive investment metric as an absolute circuit breaker:

$$\text{Graham Intrinsic Value} = \sqrt{22.5 \times \text{EPS} \times \text{BVPS}}$$

To compute calculations smoothly using standardized ratio variables, the mathematical engine evaluates the threshold in **multiples-space**:

$$\text{Graham Intrinsic Boundary} = \sqrt{22.5 \times \left( \frac{\text{Market Price}}{\text{P/E Ratio}} \right) \times \left( \frac{\text{Market Price}}{\text{P/B Ratio}} \right)}$$

* **The 22.5 Multiplier:** Derived from classical criteria setting the absolute maximum acceptable risk product at a P/E of 15.0 multiplied by a P/B of 1.5 ($15 \times 1.5 = 22.5$).

### d. Valuation Status Matrix
Assets are classified using a two-tier verification filter to enforce strict risk control:

* **`UNDERVALUED`:** Assigned **only** when an asset trades below its dynamic peer group median multiple ($P < M_{\text{sector}}$) **and** sits beneath its absolute calculated Graham Intrinsic Boundary. Both conditions must be met to verify a true margin of safety.
* **`OVERVALUED`:** Assigned when an equity systematically outpaces peer relative norms or breaches its absolute intrinsic valuation limit, signaling premium market pricing or speculative extension.
