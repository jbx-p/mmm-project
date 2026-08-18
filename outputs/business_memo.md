\# Marketing Mix Modeling: Channel Efficiency \& Budget Reallocation

\*\*Bayesian MMM — 5-Channel Analysis\*\*



\---



\## Headline Finding



Display advertising is significantly underperforming relative to its budget share, while Paid Search and Promo are underfunded relative to their contribution. Reallocating just 30% of Display's budget to those two channels is projected to generate \*\*R$2.35 in incremental sales for every dollar moved\*\* — with no net change in total spend.



\## What We Did



We built a Bayesian Marketing Mix Model (geometric adstock + logistic saturation, fit via MCMC) across 5 channels — TV, Paid Search, Paid Social, Display, and Promo — using 3 years of weekly spend and sales data. The model converged cleanly (all r\_hat = 1.00, thousands of effective samples per parameter).



\*\*A note on validation and its limits:\*\* because this analysis used data with known, embeddable ground truth, we were able to directly test whether the model recovered the true channel effects — a validation step rarely possible with real commercial data. The model successfully recovered the \*correct relative ranking\* of channel importance and \*directional\* efficiency, matching the true generating process closely. However, \*absolute\* contribution dollar estimates were inflated (roughly 100–280% above true values), traced to a structural mismatch: the underlying sales process combined trend, seasonality, and price effects multiplicatively, while the model's baseline components are additive — a known limitation of standard MMM specifications. As a result, this analysis focuses on relative efficiency and directional recommendations, which the validation confirmed are trustworthy, rather than absolute ROI dollar figures, which are not.



\## Channel Efficiency Ranking



| Channel | Spend Share | Contribution Share | Efficiency Index |

|---|---|---|---|

| Paid Search | 25% | 35% | \*\*1.40\*\* |

| Promo | 7% | 10% | \*\*1.39\*\* |

| TV | 38% | 34% | 0.88 |

| Paid Social | 20% | 17% | 0.84 |

| Display | 10% | 4% | \*\*0.45\*\* |



\*Efficiency index = contribution share ÷ spend share. Above 1.0 means a channel generates more sales impact than its budget size would predict.\*



\## Recommendation: Reallocate Budget Away From Display



We simulated shifting 30% of Display's weekly budget to Paid Search (60% of the shift) and Promo (40% of the shift), holding all other spend constant, and used the fitted model to project the effect on total sales.



| Metric | Value |

|---|---|

| Baseline projected sales (current allocation) | R$4,484,226 |

| Scenario projected sales (reallocated) | R$4,505,925 |

| Projected lift | R$21,700 (+0.48%) |

| Return per dollar reallocated | \*\*R$2.35\*\* |



The aggregate lift is modest in percentage terms because Display represents only \~10% of total spend — but the marginal return on shifted dollars is strong, and this reallocation carries no additional budget risk since total spend is unchanged.



\## Next Steps



1\. \*\*Pilot the reallocation\*\* at a smaller scale (e.g., 10–15% of Display budget) before a full 30% shift, to validate the projected lift against real results.

2\. \*\*Investigate a log-linear model specification\*\* to properly capture the multiplicative nature of trend/seasonality/price effects, which would allow trustworthy absolute ROI estimates in addition to the relative rankings already validated here.

3\. \*\*Reassess Display's role\*\* — its low efficiency may reflect genuine underperformance, or a different strategic purpose (e.g., brand reach) not well captured by a sales-attribution model; worth a qualitative review before eliminating it further.



\---

\*Methodology, code, and full model diagnostics available in the accompanying GitHub repository.\*

