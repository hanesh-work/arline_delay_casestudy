# Understanding Bayesian Model Effects: An Airline Delay Case Study

<p align="center">
  <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img alt="pandas" src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" />
  <img alt="NumPy" src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" />
  <img alt="PyMC" src="https://img.shields.io/badge/PyMC-5A4FCF?style=for-the-badge&logo=python&logoColor=white" />
  <img alt="Jupyter" src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" />
</p>

<p align="center">
  <b>A Bayesian hierarchical analysis of U.S. flight delays using BTS data to separate airline operational effects from airport infrastructure effects.</b>
</p>

---

## Introduction

This project studies U.S. flight delay patterns using a Bayesian hierarchical model. The goal is to separate airline operational effects from airport infrastructure effects while controlling for weather, NAS delay, security delay, and late aircraft delay.

Using 13,295 flight records from January to June 2025, the analysis shows that airline choice has a larger impact on delay variation than airport choice.

---

## Project Overview

**Understanding Bayesian Model Effects: An Airline Delay Case Study** examines whether delays are driven more by carriers or airports after controlling for known confounders.

The project focuses on three questions:

- Do airlines contribute more to delay variation than airports?
- Which airlines consistently save or add delay time?
- Which airports perform best and worst after adjustment?

To answer these questions, the project uses a Bayesian hierarchical model with partial pooling and variational inference.

---

## Dataset Overview

The analysis is built on BTS On-Time Performance data aggregated by carrier-airport pair.

### Data Scope

- 13,295 flight records.
- January to June 2025.
- Aggregated to the carrier-airport level.
- Restricted to the top 59 airports, which account for about 80% of traffic.

### Outcome and Controls

- Outcome: average delay minutes per arriving flight.
- Controls: weather delay, NAS delay, late aircraft delay, and security delay.
- Groups: airline and airport.

This setup avoids unfairly penalizing high-volume carriers and helps isolate residual performance differences.

---

## Methodology

The model uses a hierarchical structure to estimate airline and airport effects while accounting for nested data.

### Model Structure

- Outcome modeled as a normal distribution around a linear predictor.
- Random effects included for airline and airport.
- Fixed controls included for weather, NAS, late aircraft, and security delay.

### Why Hierarchical Modeling

- Handles grouped data properly.
- Shrinks noisy estimates toward the overall mean.
- Works well for small carriers and small airports.
- Separates true variation from sample-size noise.

### Inference Method

- Variational inference was used instead of MCMC.
- This made estimation faster for a dataset with over 13,000 observations and 100+ groups.
- The method still produced stable variance estimates.

---

## Results Summary

The main finding is that airline choice matters more than airport choice when explaining residual delay variation.

### Variance Components

| Component | Sigma | Meaning |
|---|---:|---|
| Airlines | 10.67 | Bigger differences between carriers |
| Airports | 9.40 | Smaller differences between hubs |

This means airline choice explains more of the remaining delay variation than airport choice.

### Best Airlines

| Airline | Effect (min) | 95% CI |
|---|---:|---|
| Horizon Air | -6.00 | [-12.46, -1.68] |
| Envoy Air | -4.96 | [-10.80, -1.18] |
| Alaska Airlines | -4.51 | [-10.07, -0.60] |
| Spirit | -3.43 | [-8.80, +0.36] |
| Southwest | -3.18 | [-9.02, +0.68] |

### Worst Airlines

| Airline | Effect (min) | 95% CI |
|---|---:|---|
| Air Wisconsin | +7.87 | [+2.54, +16.10] |
| GoJet Airlines | +6.05 | [+1.69, +12.02] |
| American Airlines | +5.20 | [+1.36, +11.17] |
| PSA Airlines | +4.85 | [+1.00, +10.99] |
| CommuteAir | +3.99 | [-0.04, +9.82] |

Horizon Air and Envoy Air reduce delays by about 5 to 6 minutes per flight, while Air Wisconsin adds nearly 8 minutes.

---

## Airport Rankings

The model also produced airport-level estimates after controlling for airline effects.

### Best Airports

- Lihue.
- Kona.
- Kahului.

These Hawaiian airports had the strongest adjusted delay performance.

### Worst Airports

- Chattanooga.
- Midland-Odessa.
- Several Texas regional airports.

The airport results show meaningful variation, but the spread is smaller than the airline spread.

---

## Discussion

The findings suggest that operational differences across airlines are more important than airport differences for explaining residual delay variation.

- Airlines show stronger dispersion than airports.
- Airport quality matters, but less than carrier operations.
- Partial pooling prevents small samples from being over-interpreted.
- Controlling for late aircraft delay helps reduce cascade bias.
- The model produces credible intervals, making uncertainty visible rather than hidden.

---

## Practical Takeaways

- Travelers should prioritize airline choice before airport choice.
- Corporate travel policies can save meaningful time by favoring efficient carriers.
- Airports still matter, but operational carrier management is the bigger lever.
- The modeling framework can be extended to trucking, rail, or delivery systems.

---

## Limitations

- The sample covers only the top 59 airports.
- The data spans January to June 2025 only.
- Some unmeasured factors such as aircraft age and crew training are not included.
- Delay reasons are reported by airlines and may contain reporting bias.
- Interaction effects between airline and airport were not modeled.

---

## Tech Stack

| Category | Tools |
|---|---|
| Language | Python |
| Data Handling | pandas, NumPy |
| Bayesian Modeling | PyMC / Bayesian hierarchical modeling |
| Visualization | Matplotlib, Seaborn |
| Environment | Jupyter Notebook, Google Colab |
| Output | Notebook, PDF report, CSV |

---

## Repository Structure

```bash
.
├── arline_delay_code.ipynb
├── Report.pdf
├── airline_delay_data.csv
└── README.md
```

---

## Conclusion

This project shows that airline choice has a larger impact on delay risk than airport choice. Using Bayesian hierarchical modeling with partial pooling, the analysis estimated carrier and airport effects in a way that is both interpretable and uncertainty-aware.

The strongest takeaway is simple: pick the right airline first.

---

## References

1. Bureau of Transportation Statistics. On-Time Performance Data.
2. Gelman, A., & Hill, J. Data Analysis Using Regression and Hierarchical Models.
3. Gelman, A., et al. Bayesian Data Analysis.
4. Betancourt, M., & Girolami, M. Hamiltonian Variational Inference.
5. FAA. Report on National Airspace System Efficiency.
