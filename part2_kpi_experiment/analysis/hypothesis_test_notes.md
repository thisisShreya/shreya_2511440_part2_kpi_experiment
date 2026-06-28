# Hypothesis Test Notes
## Campaign Experiment: New Onboarding & Activation Experience

---

## 1. Business Context

The company ran an A/B experiment to evaluate a new onboarding and activation campaign. Users were split into a **Control group** (existing experience, n=693) and a **Treatment group** (new campaign experience, n=715). Leadership must decide whether to roll out the treatment to all users.

---

## 2. Metric Selected for Primary Test

**Metric:** Paid Conversion Rate (proportion of users who converted to a paid subscription)

**Reason for selection:**
- This is the North Star metric — it directly measures whether the campaign achieves its core business goal: converting trial or onboarding users to paying subscribers.
- Revenue is only generated when users convert; all other funnel metrics (landing page visit, trial start, onboarding completion) are leading indicators, not outcomes.
- Conversion rate is a binary, measurable outcome suitable for proportion-based statistical testing.

---

## 3. Hypotheses

### Null Hypothesis (H₀)
> The paid conversion rate in the Treatment group is **less than or equal to** the paid conversion rate in the Control group.

H₀: p_treatment ≤ p_control

### Alternate Hypothesis (H₁)
> The paid conversion rate in the Treatment group is **greater than** the paid conversion rate in the Control group.

H₁: p_treatment > p_control

---

## 4. Test Design

| Parameter              | Value                                      |
|------------------------|--------------------------------------------|
| Test Type              | Two-Proportion Z-Test (one-tailed)         |
| Direction              | One-tailed (right-tail — testing improvement) |
| Significance Level (α) | 0.05                                       |
| Confidence Level       | 95%                                        |
| Metric Tested          | Paid Conversion Rate                       |

**Rationale for one-tailed test:**  
The business question is directional — leadership wants to know whether the treatment **improves** conversion, not merely whether it is different. A one-tailed test is appropriate and provides greater statistical power for detecting positive effects.

---

## 5. Test Inputs

| Input                      | Control     | Treatment   |
|----------------------------|-------------|-------------|
| Users (N)                  | 693         | 715         |
| Converted Users            | 22          | 50          |
| Conversion Rate            | 3.17%       | 6.99%       |
| Pooled Proportion          | —           | 5.11%       |
| Standard Error             | —           | 0.0117      |

---

## 6. Test Output

| Statistic          | Value      |
|--------------------|------------|
| Z-Statistic        | 3.2519     |
| P-Value (one-tail) | 0.000573   |
| Alpha              | 0.05       |
| Critical Z         | 1.645      |

---

## 7. Decision Rule

- **If p-value < α (0.05):** Reject H₀ — the improvement is statistically significant.
- **If p-value ≥ α (0.05):** Fail to reject H₀ — insufficient evidence of improvement.

**Outcome:** p = 0.0006 < 0.05 → **Reject H₀**

---

## 8. Interpretation

The treatment group achieved a paid conversion rate of **6.99%** versus **3.17%** in the control group — a **120% relative lift** and **+3.82 percentage point** absolute improvement. The z-statistic of 3.25 places the result far into the rejection region (critical z = 1.645). The p-value of 0.0006 means there is less than a 0.06% probability of observing this result if the null hypothesis were true.

**We conclude with 99.9% confidence that the new campaign experience meaningfully improves paid conversion rate.**

---

## 9. Guardrail Considerations

The primary test result is positive, but the following guardrail signals require attention before a full launch decision:

| Guardrail Metric      | Control | Treatment | Direction | Risk Level |
|-----------------------|---------|-----------|-----------|------------|
| Refund Rate           | 0.00%   | 0.42%     | ↑ (worse) | Low        |
| Support Ticket Rate   | 14.7%   | 24.8%     | ↑ (worse) | **HIGH**   |
| Avg Revenue/Converted | $1,630  | $770      | ↓ (worse) | Medium     |

The **support ticket rate increase of +10.1pp** is a material guardrail flag. While the conversion rate improved significantly, the treatment also generated substantially more support contacts — which may indicate user confusion, onboarding friction, or unmet expectations in the new experience.

---

## 10. Limitations

- The experiment ran with ~1,400 users. While sufficient for conversion testing, segment-level results (especially Premium plan) have smaller sample sizes and should be interpreted with caution.
- The `days_to_convert` metric is only available for 72 converted users (5.1% of total), limiting the statistical power of time-to-convert comparisons.
- No explicit randomization seed or SRM (Sample Ratio Mismatch) check is available in the dataset; group sizes are close (693 vs 715) suggesting reasonable balance.
- External factors (seasonality, marketing spend) during the experiment period are unknown.
