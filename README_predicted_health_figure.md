# Figure: Predicted Subjective Health Score Across Models 2–4

**Paper:** *For Better or for Health? Medical Knowledge Spillovers and Health Inequality in European Partnerships*  
**Author:** Michaela Bunakova, Department of Sociology, McGill University  
**Data:** European Social Survey (ESS), pooled rounds 2002–2023  

---

## What this figure shows

Three stacked panels displaying **predicted subjective health scores** (on a 1–5 scale) for each covariate category across three model specifications. Rather than showing raw β coefficients, each bar represents the **absolute predicted health score** for a respondent in that category, holding all other covariates at their reference values.

| Panel | Model | Content |
|-------|-------|---------|
| Top | Model 2 | Full individual-level demographic adjustment — all covariates shown |
| Middle | Model 3 | Additions to Model 2 only: welfare regime fixed effects |
| Bottom | Model 4 | Additions to Model 2 only: GDP per capita tertile |

Each panel shows:
- A **horizontal bar** at the predicted health score for each category
- **95% CI whiskers** around each bar
- A **dashed vertical baseline** at the intercept (the predicted score for the reference person with all covariates at their reference category)
- A **summary table** to the right with predicted score, change from baseline (Δ base), and significance stars

---

## How predicted scores are computed

Each bar is computed as:

```
predicted_score = intercept + β_covariate
```

where the **intercept** represents a reference person with all covariates at their reference category, and **β_covariate** is the coefficient for that category from the mixed-effects regression.

The **baseline** (dashed vertical line) is the intercept itself — the predicted score when all covariates are at their reference.

### Intercepts by model

| Model | Intercept (_cons) | Reference person |
|-------|-------------------|-----------------|
| Model 2 | 4.135 | Male, age 40–49, <post-secondary education, manager/professional, highest income quintile, 2-person household, working 36–55 hrs/week, partner working 36–55 hrs/week |
| Model 3 | 4.015 | Same as Model 2, + post-communist welfare regime |
| Model 4 | 4.078 | Same as Model 2, + Q1 (lowest) GDP tertile |

### 95% Confidence intervals

```
CI = (intercept + β) ± 1.96 × SE
```

Standard errors are robust SEs adjusted for clustering at the country level (37 clusters), taken directly from Stata output.

---

## Statistical models

All models are **mixed-effects linear regressions** with random intercepts at the country level (N = 34,011 observations, 37 countries). Robust standard errors are adjusted for clustering within countries. Models were estimated in Stata using:

```stata
mixed subjective_health [covariates] || cntry: , reml vce(cluster cntry)
```

### Model 2 — Full individual-level adjustment

All individual-level covariates included. Stata output:

```
Wald chi2(28) = 5466.07     Log pseudolikelihood = -1798.8243
```

| Variable | β | SE | p |
|----------|---|----|---|
| **MP partner** | **+0.069** | **0.025** | **0.006** |
| Age: 20–29 | +0.358 | 0.042 | <0.001 |
| Age: 30–39 | +0.180 | 0.034 | <0.001 |
| Age: 50–59 | −0.144 | 0.040 | <0.001 |
| Age: 60–69 | −0.224 | 0.053 | <0.001 |
| Female | +0.019 | 0.039 | 0.623 |
| Post-sec. education | +0.101 | 0.034 | 0.003 |
| Mother post-sec. | +0.000 | 0.026 | 0.990 |
| Mother educ. missing | −0.014 | 0.041 | 0.721 |
| Technicians/Clerical | −0.055 | 0.035 | 0.118 |
| Service/Sales | −0.108 | 0.064 | 0.090 |
| Agri./Crafts/Operators | −0.165 | 0.060 | 0.006 |
| Elementary occupations | −0.004 | 0.189 | 0.984 |
| Lowest income | −0.330 | 0.172 | 0.055 |
| Low-middle income | −0.131 | 0.060 | 0.029 |
| Middle income | −0.203 | 0.058 | 0.001 |
| Upper-middle income | −0.144 | 0.035 | <0.001 |
| Income missing | −0.151 | 0.046 | 0.001 |
| Household 3–4 members | −0.006 | 0.022 | 0.803 |
| Household 5+ members | +0.081 | 0.052 | 0.119 |
| Resp. hrs <15 | +0.077 | 0.061 | 0.212 |
| Resp. hrs 15–35 | −0.020 | 0.031 | 0.513 |
| Resp. hrs >55 | +0.027 | 0.048 | 0.577 |
| Resp. hrs missing | +0.032 | 0.044 | 0.459 |
| Partner hrs <15 | −0.009 | 0.070 | 0.896 |
| Partner hrs 15–35 | −0.039 | 0.047 | 0.414 |
| Partner hrs >55 | +0.031 | 0.054 | 0.563 |
| Partner hrs missing | +0.026 | 0.058 | 0.656 |
| _cons | 4.135 | 0.069 | <0.001 |

### Model 3 — + Welfare regime

Model 2 covariates plus welfare regime fixed effects. Only the additions are shown in the figure.

```
Wald chi2(32) = 7561.57     Log pseudolikelihood = -1796.0742
```

| Variable | β | SE | p |
|----------|---|----|---|
| **MP partner** | **+0.074** | **0.026** | **0.005** |
| Social Democratic | +0.223 | 0.108 | 0.039 |
| Conservative | +0.132 | 0.108 | 0.223 |
| Liberal | +0.308 | 0.113 | 0.006 |
| Mediterranean | +0.159 | 0.137 | 0.246 |
| _cons | 4.015 | 0.098 | <0.001 |

### Model 4 — + GDP tertile

Model 2 covariates plus log GDP per capita tertile. Only the additions are shown in the figure.

```
Wald chi2(30) = 5773.67     Log pseudolikelihood = -1797.5091
```

| Variable | β | SE | p |
|----------|---|----|---|
| **MP partner** | **+0.070** | **0.025** | **0.006** |
| GDP Q2 (middle) | +0.117 | 0.058 | 0.044 |
| GDP Q3 (highest) | +0.100 | 0.071 | 0.159 |
| _cons | 4.078 | 0.081 | <0.001 |

> **GDP source:** World Bank National Accounts Data, indicator `NY.GDP.MKTP.CD` (https://data.worldbank.org/indicator/NY.GDP.MKTP.CD). GDP per capita in current USD was retrieved for each country-year combination matching ESS survey rounds, log-transformed, then categorised into three approximately equal tertiles using the individual-level distribution.

---

## Colour coding

| Colour | Meaning |
|--------|---------|
| Blue | MP partner (treatment variable of interest) |
| Red | Significant covariate (p < 0.05) |
| Grey | Non-significant covariate |
| Amber | Variable newly added in this model (Models 3–4 only) |
| Dashed grey line | Baseline (intercept — reference person) |

---

## Axis and scale

- **X-axis:** Predicted subjective health score (1–5 scale), ranging from 3.30 to 4.75
- **Shared scale:** All three panels use the same x-axis range, enabling direct visual comparison across models
- **Scale rationale:** Lower bound set at 3.30 to accommodate the widest CI (Lowest income: β = −0.330, SE = 0.172, lower 95% CI bound ≈ 3.468); upper bound at 4.75 to accommodate Age 20–29 upper CI (≈ 4.575)

---

## How to reproduce the figure

The figure is a standalone HTML file using [Chart.js 4.4.1](https://www.chartjs.org/). Open in any modern browser — no build step or server required.

### Dependencies

| Library | Version | CDN |
|---------|---------|-----|
| Chart.js | 4.4.1 | `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js` |

### Key implementation details

**Predicted score calculation** — each bar value is `intercept + β`, computed in JavaScript before passing to Chart.js:

```javascript
const preds = rows.map(r => +(intercept + r.b).toFixed(4));
```

**Baseline dashed line** — drawn as a custom Chart.js plugin using `beforeDraw`, positioned at `x.getPixelForValue(intercept)`:

```javascript
function makeBaselinePlugin(intercept) {
  return { id: 'baseline', beforeDraw(chart) {
    const { ctx, scales: { x } } = chart;
    const px = x.getPixelForValue(intercept);
    ctx.save();
    ctx.strokeStyle = 'rgba(0,0,0,0.22)';
    ctx.lineWidth = 1.2;
    ctx.setLineDash([5, 4]);
    ctx.beginPath();
    ctx.moveTo(px, chart.chartArea.top);
    ctx.lineTo(px, chart.chartArea.bottom);
    ctx.stroke();
    ctx.restore();
  }};
}
```

**95% CI whiskers** — drawn as a custom `afterDatasetsDraw` plugin, clipping the CI at the axis boundaries:

```javascript
const lo = x.getPixelForValue(pred - 1.96 * se);
const hi = x.getPixelForValue(pred + 1.96 * se);
```

**Models 3 and 4** show only the newly added variables (welfare regime / GDP tertile) plus the updated MP partner coefficient — all other Model 2 covariates are omitted to avoid repetition.

---

## Notes on interpretation

- The **baseline** in each model represents a different hypothetical reference person because the intercept changes when new covariates are added (welfare regime in M3, GDP tertile in M4). Baselines are therefore not directly comparable across models.
- **Models 3 and 4** use a different intercept than Model 2, so the baseline dashed line shifts slightly between panels. This reflects the re-anchoring of predictions when country-level covariates are added.
- The **MP partner effect** is modest (≈ +0.07 points) relative to the age gradient (≈ 0.58 points from youngest to oldest) and the income gradient (≈ 0.33 points from highest to lowest), consistent with the paper's argument that relational health capital operates within larger structural constraints.
- The **wide CI on Lowest income** (SE = 0.334) reflects the sparse representation of MP-partner households at the bottom of the income distribution in this elite-restricted analytical sample — this should be interpreted with caution.

---

## Citation

If you use or adapt this figure, please cite:

> Bunakova, M. (*forthcoming*). For Better or for Health? Medical Knowledge Spillovers and Health Inequality in European Partnerships. McGill University.
