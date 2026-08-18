# Marketing Mix Modeling (MMM)

Bayesian Marketing Mix Model (geometric adstock + logistic saturation, via
pymc-marketing) quantifying channel-level efficiency and projecting the
impact of a budget reallocation, built on simulated weekly sales and spend
data across 5 marketing channels with known ground-truth channel effects.

## Key results
- Model converged cleanly (all r_hat = 1.00) and correctly recovered the
  *relative* ranking and efficiency of all 5 channels versus ground truth
- Found Paid Search and Promo are the most efficient channels (1.40x and
  1.39x contribution-to-spend ratio); Display is the least efficient (0.45x)
- Projected reallocating 30% of Display's budget to Paid Search and Promo
  generates **R$2.35 in incremental sales per dollar shifted**, with total
  spend unchanged

## A genuine modeling challenge (and how it was resolved)
An initial model fit produced channel contribution estimates 100-280% above
the true embedded values, despite excellent convergence diagnostics. Root
cause: the synthetic sales process combines trend, seasonality, and price
effects *multiplicatively*, while a standard MMM's baseline components are
*additive* — a real, known limitation of adstock/saturation MMM
specifications. Adding an explicit trend control column substantially
reduced (but did not eliminate) the bias.

**Consequence for interpretation:** absolute contribution dollar estimates
carry a caveat and should be treated as directionally indicative. Relative
channel ranking and efficiency — which the ground-truth validation confirmed
as accurate — are the trustworthy output of this model, and are what the
budget reallocation recommendation is based on. See `outputs/business_memo.md`
for the full writeup.

## Dataset
Synthetic — no license restrictions. Generated in `01_data_generation.ipynb`
with known ground-truth channel effects embedded for validation. Regenerate
via that notebook, or use the version already committed at
`data/raw/mmm_weekly_data.csv`.

## Setup

**Environment notes (pymc-marketing 1.0.0):**
- Saving/loading the fitted model requires `h5netcdf` and `h5py` (included
  in `requirements.txt`)
- `adstock`/`saturation` are passed as transformer objects
  (`GeometricAdstock`, `LogisticSaturation`), not integer shorthand
- Use `mmm.save()` / `MMM.load()` for persistence — more reliable than
  calling `.idata.to_netcdf()` directly, which was prone to file-lock
  issues on Windows during development

## Structure
- `notebooks/01_data_generation` — synthetic weekly sales/spend data with
  embedded ground-truth channel effects
- `notebooks/02_mmm_model` — Bayesian MMM fit, convergence diagnostics,
  ground-truth validation, channel efficiency ranking
- `notebooks/03_budget_reallocation` — budget shift scenario using the
  fitted model's posterior predictive, loaded independently via `MMM.load()`
- `outputs/business_memo.md` — full findings and recommendation
- `outputs/charts/` — baseline-vs-actual validation chart

## Status
✅ Complete