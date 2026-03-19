# Figure: Predicted Subjective Health Score — Interaction Models 5–8

**Paper:** *For Better or for Health? Medical Knowledge Spillovers and Health Inequality in European Partnerships*  
**Author:** Michaela Bunakova, Department of Sociology, McGill University  
**Data:** European Social Survey (ESS), pooled rounds 2002–2023  

---

## What this figure shows

Four stacked panels displaying **predicted subjective health scores** (on a 1–5 scale) for respondents *with* and *without* an MP partner, at each level of a moderating variable. Each panel corresponds to one interaction model:

| Panel | Model | Moderator |
|-------|-------|-----------|
| Top | Model 5 | Welfare regime |
| Second | Model 6 | Age group |
| Third | Model 7 | Household income quintile |
| Bottom | Model 8 | GDP per capita tertile |

Each panel shows:
- **Side-by-side horizontal bars** at each moderator level — grey (no MP partner) and blue (with MP partner)
- **95% CI whiskers** around each bar
- A **dashed vertical baseline** at the model intercept (predicted score for the reference person with no MP partner and all covariates at reference)
- A **summary table** to the right showing predicted scores for each group, the MP partner gap at that level, and a star where the interaction term is significant (p < 0.05)

The key visual feature is the **changing gap** between grey and blue bars across moderator levels — a narrowing or reversing gap indicates that the MP partner health advantage is moderated by that variable.

---

## How predicted scores are computed

For each moderator category, two predicted scores are computed:

```
no_mp   = intercept + β_moderator_level
with_mp = intercept + β_treat + β_moderator_level + β_treat#moderator_level
```

For the **reference category** (first row in each panel):

```
no_mp   = intercept
with_mp = intercept + β_treat
```

The **MP partner gap** at each level is:

```
gap = β_treat + β_treat#moderator_level
```

For the reference category, the gap equals the main effect of `treat` directly.

### Intercepts and main treat effects by model

| Model | Intercept (_cons) | β_treat (ref. category) | SE_treat | Reference category |
|-------|-------------------|------------------------|----------|--------------------|
| Model 5 | 3.962 | +0.168 | 0.060 | Post-communist regime |
| Model 6 | 4.310 | +0.046 | 0.048 | Age 40–49 |
| Model 7 | 3.991 | +0.127 | 0.027 | Highest income quintile |
| Model 8 | 3.963 | +0.164 | 0.052 | Q1 (lowest GDP tertile) |

### 95% Confidence intervals

For **no-MP bars** (non-reference categories):
```
CI = no_mp ± 1.96 × SE_moderator
```

For **with-MP bars**:
- Reference category: `CI = with_mp ± 1.96 × SE_treat`
- Non-reference categories: `CI = with_mp ± 1.96 × √(SE_treat² + SE_interaction²)`

This approximation treats the treat and interaction SEs as independent. Standard errors are robust SEs adjusted for clustering at the country level (37 clusters), taken directly from Stata output.

### Significance marker

The **star (*)** in the table denotes that the *interaction term* is significantly different from zero (p < 0.05) — i.e., the MP partner gap at that level is significantly different from the gap at the reference category. This is distinct from whether the *net gap itself* differs from zero.

---

## Statistical models

All models are **mixed-effects linear regressions** with random intercepts at the country level (N = 34,011, 37 countries), estimated in Stata:

```stata
mixed subjective_health i.treat##i.moderator [covariates] || cntry: , reml vce(cluster cntry)
```

All models include the full set of individual-level covariates from Model 2 (age, gender, education, mother's education, occupation, income, household size, respondent and partner working hours) plus welfare regime fixed effects.

### Model 5 — Welfare regime interaction

```
Log pseudolikelihood = -1794.3953
```

| Variable | β | SE | p |
|----------|---|----|---|
| **treat (ref: Post-Communist)** | **+0.168** | **0.060** | **0.005** |
| Social Democratic | +0.293 | 0.083 | <0.001 |
| Conservative | +0.189 | 0.101 | 0.062 |
| Liberal | +0.364 | 0.086 | <0.001 |
| Mediterranean | +0.265 | 0.141 | 0.060 |
| treat × Social Democratic | −0.140 | 0.085 | 0.098 |
| treat × Conservative | −0.106 | 0.067 | 0.113 |
| treat × Liberal | −0.111 | 0.085 | 0.192 |
| treat × Mediterranean | **−0.203** | **0.097** | **0.037** |
| _cons | 3.962 | 0.092 | <0.001 |

### Model 6 — Age group interaction

```
Log pseudolikelihood = -1796.7447
```

| Variable | β | SE | p |
|----------|---|----|---|
| **treat (ref: 40–49)** | **+0.046** | **0.048** | **0.338** |
| Age: 20–29 | +0.281 | 0.032 | <0.001 |
| Age: 30–39 | +0.171 | 0.015 | <0.001 |
| Age: 50–59 | −0.154 | 0.022 | <0.001 |
| Age: 60–69 | −0.235 | 0.029 | <0.001 |
| treat × 20–29 | +0.161 | 0.088 | 0.068 |
| treat × 30–39 | +0.021 | 0.061 | 0.734 |
| treat × 50–59 | +0.022 | 0.080 | 0.785 |
| treat × 60–69 | +0.025 | 0.096 | 0.792 |
| _cons | 4.310 | 0.101 | <0.001 |

### Model 7 — Household income interaction

```
Log pseudolikelihood = -1793.536
```

| Variable | β | SE | p |
|----------|---|----|---|
| **treat (ref: Highest)** | **+0.127** | **0.027** | **<0.001** |
| Upper-middle income | −0.062 | 0.015 | <0.001 |
| Middle income | −0.144 | 0.023 | <0.001 |
| Low-middle income | −0.167 | 0.040 | <0.001 |
| Lowest income | −0.265 | 0.040 | <0.001 |
| treat × Upper-middle | **−0.171** | **0.069** | **0.013** |
| treat × Middle | −0.127 | 0.139 | 0.360 |
| treat × Low-middle | +0.131 | 0.156 | 0.400 |
| treat × Lowest | −0.129 | 0.334 | 0.699 |
| _cons | 3.991 | 0.098 | <0.001 |

> **Note:** The very wide CI for the Lowest income category (SE = 0.334) reflects sparse representation of MP-partner households at the bottom of the income distribution in this elite-restricted analytical sample. This estimate should be interpreted with caution.

### Model 8 — GDP per capita tertile interaction

```
Log pseudolikelihood = -1794.1487
```

| Variable | β | SE | p |
|----------|---|----|---|
| **treat (ref: Q1 lowest)** | **+0.164** | **0.052** | **0.002** |
| GDP Q2 (middle) | +0.096 | 0.062 | 0.123 |
| GDP Q3 (highest) | +0.078 | 0.067 | 0.241 |
| treat × Q2 | **−0.138** | **0.070** | **0.047** |
| treat × Q3 | **−0.138** | **0.064** | **0.030** |
| _cons | 3.963 | 0.094 | <0.001 |

> **GDP source:** World Bank National Accounts Data, indicator `NY.GDP.MKTP.CD` (https://data.worldbank.org/indicator/NY.GDP.MKTP.CD). GDP per capita in current USD was retrieved for each country-year combination matching ESS survey rounds, log-transformed, then categorised into three approximately equal tertiles using the individual-level distribution.

---

## Full data values used in figure

### Model 5 — Welfare regime

| Category | No MP predicted | With MP predicted | Gap | Interaction sig. |
|----------|----------------|-------------------|-----|-----------------|
| Post-Communist (ref) | 3.962 | 4.130 | +0.168 | ref |
| Social Democratic | 4.256 | 4.283 | +0.028 | No |
| Conservative | 4.151 | 4.213 | +0.062 | No |
| Liberal | 4.326 | 4.383 | +0.057 | No |
| Mediterranean | 4.227 | 4.192 | −0.034 | Yes * |

### Model 6 — Age group

| Category | No MP predicted | With MP predicted | Gap | Interaction sig. |
|----------|----------------|-------------------|-----|-----------------|
| 40–49 (ref) | 4.310 | 4.356 | +0.046 | ref |
| 20–29 | 4.592 | 4.799 | +0.207 | No |
| 30–39 | 4.481 | 4.548 | +0.067 | No |
| 50–59 | 4.157 | 4.225 | +0.068 | No |
| 60–69 | 4.075 | 4.147 | +0.071 | No |

### Model 7 — Household income

| Category | No MP predicted | With MP predicted | Gap | Interaction sig. |
|----------|----------------|-------------------|-----|-----------------|
| Highest (ref) | 3.991 | 4.118 | +0.127 | ref |
| Upper-middle | 3.929 | 3.884 | −0.044 | Yes * |
| Middle | 3.846 | 3.847 | +0.000 | No |
| Low-middle | 3.824 | 4.082 | +0.258 | No |
| Lowest | 3.725 | 3.723 | −0.002 | No |

### Model 8 — GDP tertile

| Category | No MP predicted | With MP predicted | Gap | Interaction sig. |
|----------|----------------|-------------------|-----|-----------------|
| Q1 — low (ref) | 3.963 | 4.127 | +0.164 | ref |
| Q2 — middle | 4.059 | 4.085 | +0.026 | Yes * |
| Q3 — high | 4.041 | 4.066 | +0.026 | Yes * |

---

## Colour coding

| Colour | Meaning |
|--------|---------|
| Blue bars | Predicted health score with MP partner |
| Grey bars | Predicted health score without MP partner |
| Dashed grey line | Baseline (intercept — reference person, no MP partner) |
| * in table | Interaction term significantly different from reference (p < 0.05) |
| "ref" in table | Reference category |

---

## Axis and scale

- **X-axis:** Predicted subjective health score (1–5 scale), ranging from 3.30 to 4.75
- **Shared scale:** All four panels use the same x-axis range as Models 2–4, enabling direct visual comparison across the entire set of figures

---

## Key interpretation notes

**Model 5 (welfare regime):** The MP partner health advantage is largest in Post-Communist countries (+0.168) and is the only regime where it turns negative in Mediterranean countries (gap = −0.034), the only significant interaction. This suggests the health spillover from an MP partner may be less relevant in contexts where public health infrastructure already provides broader access to medical knowledge.

**Model 6 (age):** The MP partner gap is remarkably stable across age groups — none of the interaction terms are significant. The omnibus pattern suggests the relational health capital mechanism operates independently of life-course stage.

**Model 7 (income):** The gap is largest at the top of the income distribution (+0.127) and is significantly attenuated among upper-middle income households (−0.044 net gap). The "Lowest" income cell has an extremely wide CI (SE = 0.334) due to sparse representation of MP-partner households in that stratum and should not be interpreted.

**Model 8 (GDP):** The most striking pattern — the MP partner advantage of +0.164 in low-GDP countries shrinks to +0.026 in both Q2 and Q3, with both interactions significant. This is consistent with a macro-level substitution mechanism: in wealthier countries, population-level access to health information may substitute for the individualised health capital transmitted through an MP partner.

---

## How to reproduce the figure

The figure is a standalone HTML file using [Chart.js 4.4.1](https://www.chartjs.org/). Open in any modern browser — no build step or server required.

### Dependencies

| Library | Version | CDN |
|---------|---------|-----|
| Chart.js | 4.4.1 | `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js` |

### Key implementation details

**Predicted score calculation:**

```javascript
// No MP partner bar
const no_mp = intercept + cat.mod_b;

// With MP partner bar
const with_mp = intercept + treat_b + cat.mod_b + cat.int_b;

// Gap
const gap = treat_b + cat.int_b;
```

**CI approximation for with-MP bars (non-reference categories):**

```javascript
const mp_se = cat.ref
  ? treat_se
  : Math.sqrt(treat_se ** 2 + cat.int_se ** 2);
```

**Interleaved bar layout** — bars for each category are interleaved (no-MP, then with-MP) so they appear as a pair:

```javascript
m.categories.forEach(cat => {
  interleavedData.push(no_mp_pred);   // grey bar
  interleavedData.push(with_mp_pred); // blue bar
});
```

**CI whiskers** — drawn as a custom `afterDatasetsDraw` plugin, with separate colour and position for each bar in the pair:

```javascript
const yBase = y.getPixelForValue(i * 2);      // no-MP bar
const yMp   = y.getPixelForValue(i * 2 + 1); // with-MP bar
```

**Baseline dashed line** — positioned at `x.getPixelForValue(intercept)`, representing the predicted score for the reference person with no MP partner.

---

## Citation

If you use or adapt this figure, please cite:

> Bunakova, M. (*forthcoming*). For Better or for Health? Medical Knowledge Spillovers and Health Inequality in European Partnerships. McGill University.
