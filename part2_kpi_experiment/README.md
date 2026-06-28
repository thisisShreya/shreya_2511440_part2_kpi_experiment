# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

## Repository Structure

```
part2_kpi_experiment/
├── data/
│   └── campaign_experiment_data.xlsx
├── analysis/
│   ├── experiment_analysis.xlsx
│   └── hypothesis_test_notes.md
├── outputs/
│   ├── kpi_tree.png
│   ├── experiment_summary.xlsx
│   └── recommendation_memo.md
├── screenshots/
│   ├── summary_metrics.png
│   ├── hypothesis_test_output.png
│   └── kpi_tree_preview.png
└── README.md
```

---

## Business Context

A subscription-based digital product company launched a new **onboarding and activation campaign** to improve user conversion and early engagement. New users were randomly assigned to:

- **Control group (n=693):** Existing onboarding experience
- **Treatment group (n=715):** New campaign experience

Leadership must decide whether to launch the treatment to all users. This analysis frames the business problem, defines success metrics, analyzes experiment results, evaluates guardrail metrics, and delivers a data-backed recommendation.

---

## Dataset Description

**File:** `data/campaign_experiment_data.xlsx`  
**Total rows:** 1,408 users  
**Columns (16):**

| Column | Description |
|---|---|
| user_id | Unique user identifier |
| signup_date | Date the user signed up |
| experiment_group | Control or Treatment |
| region | Geographic region (North, South, East, West) |
| device_type | Desktop, Mobile, Tablet |
| traffic_source | Organic, Paid Search, Email, Social, Referral |
| plan_type | Free, Basic, Premium |
| visited_landing_page | Binary (1/0) |
| started_trial | Binary (1/0) |
| completed_onboarding | Binary (1/0) |
| converted_to_paid | Binary (1/0) — **North Star Metric** |
| revenue_30d | Revenue in first 30 days (USD) |
| support_tickets_30d | Number of support tickets raised |
| refund_requested | Binary (1/0) |
| days_to_convert | Days from signup to conversion (null if not converted) |
| engagement_score | Continuous score 0–100 |

---

## North Star Metric

**Paid Conversion Rate** — the proportion of new users who convert to a paid subscription within 30 days.

**Why this metric:**
- It directly measures the campaign's business objective: turning new users into paying subscribers.
- All other funnel metrics (landing page visit, trial start, onboarding completion) are **drivers** of conversion, not outcomes in themselves.
- It ties directly to revenue and LTV growth.
- It is objectively measurable and statistically testable.

**Risk of blind optimization:** Over-optimizing for conversion rate without guardrails could attract low-intent users, inflate support costs, generate refunds, and reduce average revenue per user — ultimately harming business health even as the top-line conversion number improves.

---

## KPI Tree Summary

```
Paid Conversion Rate (North Star)
│
├── Funnel Depth (Activation)
│   ├── Landing Page Visit Rate
│   ├── Trial Start Rate
│   └── Onboarding Completion Rate
│
├── Revenue Quality
│   ├── Avg Revenue per Converted User
│   ├── Plan Mix (upgrade distribution)
│   └── Days to Convert
│
├── Engagement & Retention
│   ├── Engagement Score
│   ├── Feature Adoption Rate
│   └── Day-7 / Day-30 Retention
│
└── Guardrail Metrics
    ├── Refund Rate
    ├── Support Ticket Rate
    └── Segment-level Conversion Decline
```

See `outputs/kpi_tree.png` for the full visual.

---

## Experiment Analysis Approach

All analysis was conducted in `analysis/experiment_analysis.xlsx` and `outputs/experiment_summary.xlsx`.

### Data Quality Checks Performed

| Check | Finding | Handling |
|---|---|---|
| Duplicate user_id | 8 duplicates found | First occurrence retained; duplicates flagged |
| Missing: device_type | 18 rows | Retained; excluded from device segment |
| Missing: traffic_source | 24 rows | Retained; excluded from traffic segment |
| Missing: engagement_score | 14 rows | Excluded from engagement average only |
| Missing: days_to_convert | 1,336 rows | Expected — only converted users have values |
| Invalid binary values | 0 invalid | All binary columns contain only 0 or 1 |
| Revenue outliers (>3 std) | 18 users | Retained — high-value users are valid |
| Group balance | 693 vs 715 | Balanced; difference of 22 users is acceptable |

---

## Hypothesis Test Summary

- **Metric tested:** Paid Conversion Rate
- **Test type:** Two-Proportion Z-Test (one-tailed)
- **H₀:** p_treatment ≤ p_control
- **H₁:** p_treatment > p_control
- **Significance level (α):** 0.05
- **Control rate:** 3.17% (22/693)
- **Treatment rate:** 6.99% (50/715)
- **Z-Statistic:** 3.2519
- **P-Value:** 0.0006
- **Decision:** Reject H₀ — result is statistically significant at the 99.9% confidence level

The treatment group showed a **120% relative lift** in paid conversion rate over control.

See `analysis/hypothesis_test_notes.md` and `screenshots/hypothesis_test_output.png` for full details.

---

## Guardrail Metrics Considered

| Guardrail Metric | Control | Treatment | Δ | Risk |
|---|---|---|---|---|
| Refund Rate | 0.00% | 0.42% | +0.42pp | Low |
| Support Ticket Rate | 14.7% | 24.8% | +10.1pp | **HIGH** |
| Avg Revenue / Converted User | $1,630 | $770 | −$860 | Medium |

The **support ticket rate increase (+68% relative)** is the primary guardrail concern. It suggests the new campaign may cause user confusion at scale, increasing operational cost and potentially degrading NPS.

---

## Final Recommendation

**Conditional Launch — Phase-gated rollout pending support ticket investigation**

The experiment shows a statistically significant, consistent, and practically meaningful improvement in paid conversion across all regions and device types. However, the support ticket rate increase must be root-caused and resolved before full rollout.

**Recommended actions:**
1. Investigate support ticket drivers in the treatment group
2. Fix identified UX/copy issues in the new onboarding flow
3. Soft-launch at 30–50% traffic with a hard guardrail on support ticket rate
4. Monitor 90-day revenue per converted user to assess revenue quality over time
5. Proceed to full launch once guardrails are cleared

---

## Assumptions and Limitations

- Revenue is measured at 30 days only; long-term LTV impact is unknown
- No formal SRM (Sample Ratio Mismatch) test was conducted
- Days-to-convert data available for only 72 users (converted only), limiting that analysis
- Experiment window and duration are not specified in the dataset; seasonal effects are uncontrolled
- Segment-level results for smaller subgroups (Premium, Tablet) should be treated as directional only

---

## Screenshots Included

| File | Description |
|---|---|
| `screenshots/summary_metrics.png` | Control vs Treatment summary metrics table |
| `screenshots/hypothesis_test_output.png` | Z-test calculation and result |
| `screenshots/kpi_tree_preview.png` | KPI tree visual |

---

## Files Reference

| File | Location | Description |
|---|---|---|
| Raw Dataset | `data/campaign_experiment_data.xlsx` | Source experiment data |
| Data Quality + Hypothesis Test | `analysis/experiment_analysis.xlsx` | Cleaned data, quality checks, test output |
| Hypothesis Test Notes | `analysis/hypothesis_test_notes.md` | Full hypothesis framing and interpretation |
| KPI Tree | `outputs/kpi_tree.png` | Visual KPI breakdown |
| Experiment Summary | `outputs/experiment_summary.xlsx` | Metric comparisons by group and segment |
| Recommendation Memo | `outputs/recommendation_memo.md` | Executive recommendation with full analysis |
