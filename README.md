# Virexo – Lead Conversion Prediction

**Task 02 — AI & Machine Learning**

## Files

| File | What it is |
|---|---|
| `Virexo_Lead_Conversion_Prediction.ipynb` | Main notebook, already run with all outputs and charts saved |
| `virexo_leads.csv` | Dataset (1,435 rows, 11 columns) |

## Dataset source

Synthetic / dummy data, generated specifically for this task. **No public dataset was
used.** It was built to imitate a CRM export from a digital agency, deliberately including
missing values, duplicate rows, inconsistent text casing and a few impossible values so
there is real cleaning work to show.

## How to run it

Upload both files to Google Colab (or Jupyter), keeping them in the same folder, and run
all cells top to bottom. Only pandas, numpy, matplotlib, seaborn and scikit-learn are
needed — all pre-installed in Colab.

## Model evaluation results

Test set = 280 leads the models never saw. 80/20 stratified split.

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| **Logistic Regression** | **0.700** | **0.656** | **0.540** | **0.592** | **0.772** |
| Random Forest | 0.693 | 0.652 | 0.513 | 0.574 | 0.762 |

Confusion matrix (Logistic Regression): 61 true positives, 135 true negatives,
32 false positives, 52 false negatives.

The Random Forest did **not** beat the simpler model. Since Logistic Regression matches it
and is far easier to explain, it's the better pick here.

## Business interpretation (short version)

The model gets the outcome right about 7 times out of 10, against a ~60% baseline from
always guessing "no conversion". The number that matters more is **ROC-AUC of 0.77** — given
one converter and one non-converter, it ranks them correctly 77% of the time. That's what
makes it useful, because the real job is *ranking* the day's leads, not labelling them.

What the analysis shows:

- **Referrals convert far above average; social media well below.** An argument for a
  referral incentive and a harder look at social ad spend.
- **Slow response time strongly predicts non-conversion** — and it's one of the few factors
  Virexo directly controls.
- **Past enquiries are undervalued.** Prior interaction is a strong positive signal, and
  re-engaging them is cheaper than buying new leads.

**The honest weak spot:** at the default 0.5 threshold, recall is only 0.54 — the model
misses nearly half of real converters. The notebook shows this is a threshold problem, not
a model problem. Dropping to 0.30 lifts recall to 0.82 (precision slips 0.66 → 0.59, and
leads flagged rise from 93 to 158). Whether that trade is worth it depends on the sales
team's capacity — a business decision, not a technical one.

## Limitations

**This is a proof of concept, not a production-ready model.**

- Data is synthetic and generated from a known relationship, so the model is partly
  recovering a pattern that was planted. Real CRM data will be noisier and the signal weaker.
- 1,435 rows is small, and there's no time dimension — no seasonality or drift.
- Missing values were assumed missing at random and imputed with median/mode.
- `engagement_score` overlaps with `pages_visited` and `email_opens`, so individual
  coefficients aren't clean independent effects.
- **Correlation isn't causation** — fast response is associated with conversion, but the
  team may already be replying faster to leads that look promising. Only an A/B test settles it.
- `age_group` is used as a feature. Using age to decide who gets a sales call needs an
  ethical and legal review before any real deployment.

Full detail is in section 12 of the notebook.
