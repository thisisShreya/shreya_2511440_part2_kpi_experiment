# Recommendation Memo
## Campaign Experiment: New Onboarding & Activation Experience

**To:** Product & Growth Leadership  
**From:** Business Analytics Team  
**Date:** June 2026  
**Subject:** A/B Experiment Results — Launch Recommendation

---

## Executive Summary

The new onboarding and activation campaign produced a **statistically significant 120% lift in paid conversion rate** (3.17% → 6.99%, p = 0.0006). The result is robust across all four regions and all device types. However, a material increase in support ticket rate (+10.1pp) and a decline in average revenue per converted user ($1,630 → $770) introduce guardrail risks that must be addressed before full launch.

**Recommendation: Conditional Launch — Address Support Ticket Risk First**

---

## Business Problem Statement

The company must decide whether to replace the existing user onboarding experience with a new campaign experience for all new users. The decision impacts the entire new-user acquisition funnel and downstream revenue. The primary metric that must improve is paid conversion rate. Risks that must be monitored are support burden, refund rate, and revenue quality. Evidence required: statistically significant improvement in paid conversion rate with acceptable guardrail metric performance.

---

## North Star Metric

**Paid Conversion Rate** — the proportion of new users who convert to a paid subscription within 30 days of signup.

**Why this is the North Star:**
- It is the direct output that connects user acquisition to revenue generation.
- All upstream funnel metrics (landing page visit, trial start, onboarding completion) are drivers of this outcome — not outcomes themselves.
- It connects to business growth: each additional converted user generates direct subscription revenue and improves LTV.

**What could go wrong if optimized blindly:** If conversion is maximized through high-pressure or misleading onboarding flows, refund rates and support tickets rise, net revenue quality falls, and long-term retention suffers — as partially evidenced by the lower revenue-per-converted-user in the treatment group.

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
│   ├── Plan Mix (Free → Paid upgrade rate)
│   └── Days to Convert (Speed)
│
├── Engagement & Retention Signals
│   ├── Engagement Score (0–100)
│   ├── Feature Adoption Rate
│   └── Day-7 / Day-30 Retention Rate
│
└── Guardrail Metrics
    ├── Refund Rate
    ├── Support Ticket Rate
    └── Segment-level Conversion Decline
```

---

## Experiment Result Summary

| Metric                        | Control | Treatment | Δ Absolute | Δ Relative |
|-------------------------------|---------|-----------|------------|------------|
| Users (N)                     | 693     | 715       | +22        | +3.2%      |
| Landing Page Visit Rate       | 63.6%   | 72.6%     | +9.0pp     | +14.1%     |
| Trial Start Rate              | 25.1%   | 29.1%     | +4.0pp     | +15.9%     |
| Onboarding Completion Rate    | 15.6%   | 21.3%     | +5.7pp     | +36.5%     |
| **Paid Conversion Rate ★**   | **3.17%** | **6.99%** | **+3.82pp** | **+120.3%** |
| Avg Revenue per User          | $51.75  | $53.88    | +$2.13     | +4.1%      |
| Avg Revenue per Converted User| $1,630  | $770      | −$860      | −52.7%     |
| Refund Rate                   | 0.00%   | 0.42%     | +0.42pp    | —          |
| Support Ticket Rate           | 14.7%   | 24.8%     | +10.1pp    | +68.4%     |
| Avg Engagement Score          | 57.0    | 62.9      | +5.9       | +10.4%     |
| Avg Days to Convert           | 8.86    | 6.40      | −2.46 days | −27.8%     |

★ = North Star Metric

---

## Hypothesis Test Interpretation

- **Test:** Two-proportion Z-test (one-tailed, α = 0.05)
- **H₀:** p_treatment ≤ p_control
- **H₁:** p_treatment > p_control
- **Z-Statistic:** 3.25 | **P-Value:** 0.0006 | **Decision: Reject H₀**

The treatment group's improvement in paid conversion rate is statistically significant at the 99.9% confidence level. The new campaign experience meaningfully moves users through the funnel at every stage — from landing page through to paid conversion.

---

## Guardrail Analysis

### 1. Support Ticket Rate — ⚠ HIGH RISK
Treatment users raised support tickets at a rate of 24.8% versus 14.7% in control — a 68% relative increase. This is the most significant guardrail flag. High support rates suggest the new onboarding creates confusion or unmet expectations for a meaningful subset of users. At scale, this will substantially increase customer support costs and may degrade user satisfaction scores. **This must be investigated and resolved before full rollout.**

### 2. Avg Revenue per Converted User — ⚠ MEDIUM RISK
While more users convert in treatment, the average revenue per converted user dropped from $1,630 to $770. This may indicate the treatment is converting lower-intent or lower-value users (e.g., more Free plan conversions vs. Basic/Premium). The increase in absolute converted users (50 vs. 22) partially offsets this, but revenue quality should be tracked in production.

### 3. Refund Rate — LOW RISK
Treatment has a minor refund rate of 0.42% vs 0.00% in control. With only 3 refunds from 50 converted treatment users, this is statistically marginal and likely not material — but should be monitored as volume scales.

---

## Segment-Level Insights

**By Region:** Treatment outperforms control in all four regions. The North region shows the strongest lift (+5.5pp, from 3.4% to 8.9%), while the West region shows the smallest (+1.7pp). No region shows a decline.

**By Device:** Treatment outperforms control on all devices. Mobile shows particular strength (+4.8pp), suggesting the new onboarding experience is well-optimized for mobile — the dominant device type in the dataset (60%+ of users).

**By Plan Type:** The Free plan segment shows the most dramatic lift (+6.2pp, from 3.0% to 9.2%), suggesting the treatment is most effective at converting free-tier users. Basic and Premium plan segments show more modest improvements (+0.3pp and +3.5pp respectively).

---

## Final Recommendation

**Conditional Launch — Phase-gated rollout with support investigation gate**

The statistical evidence for the treatment's effectiveness is strong and consistent across segments. However, the support ticket rate increase is a material operational risk that must be addressed.

**Recommended Next Steps:**

1. **Immediate:** Root-cause the support ticket spike. Analyze ticket categories in the treatment group to identify which onboarding step(s) generate confusion. This may be fixable with copy changes, UI improvements, or a clarification flow.

2. **Short-term (2–4 weeks):** Launch treatment to 30–50% of new users across all regions while monitoring support ticket rate in production. Set a hard guardrail: if support ticket rate exceeds 22% in production, pause the rollout.

3. **Revenue quality monitoring:** Track 90-day revenue per converted user (not just 30-day) to assess whether the lower per-conversion revenue in treatment recovers over time. Free-plan converters may upgrade later.

4. **Segment consideration:** Given the North region's outsized lift and the Free plan's strong response, prioritize monitoring these segments for sustainability.

5. **Full launch:** Proceed to 100% rollout once support ticket rate is under control and revenue-per-converted-user trends are understood.

---

## Risks and Limitations

- The experiment window and duration are not specified; seasonal effects may influence results.
- `days_to_convert` data is available for only 72 users (5.1%), limiting time-to-convert analysis reliability.
- Revenue figures are 30-day snapshots; LTV impact is unknown.
- Sample sizes in segment subgroups (e.g., Premium plan, Tablet device) are small and should be interpreted cautiously.
- No SRM (Sample Ratio Mismatch) test was formally conducted; group sizes differ by 22 users, which appears balanced.

---

*This memo was prepared based on analysis of 1,408 experiment users using a two-proportion z-test at α = 0.05.*
