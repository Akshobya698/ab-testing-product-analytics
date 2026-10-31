# Retention Dynamics Report — Milestone 3

**Project:** A/B Testing & Product Experimentation Analysis  
**Author:** Akshobya698  
**Analysis window:** Jan 2024 – Sep 2024 · 9 acquisition cohorts · up to 8 months of follow-up  
**Notebook:** `notebooks/03_cohort_retention.ipynb`

---

## Retention Matrix Summary

| Cohort Age | Avg Retention | Step Change |
|:---:|---:|---:|
| M0 | 100.0 % | — (baseline) |
| M1 | 44.0 % | −56.0 pp |
| M2 | 33.4 % | −10.6 pp |
| M3 | 28.7 % | −4.7 pp |
| M4 | 26.4 % | −2.3 pp |
| M5 | 24.6 % | −1.8 pp |
| M6 | 23.1 % | −1.5 pp |
| M7 | 21.9 % | −1.2 pp |
| M8 | 20.9 % | −1.0 pp |

> All Cohort Index 0 values equal exactly 100.0 % — sanity check ✅  
> Pivot table forms a proper upper-left triangle — sanity check ✅

---

## Curve Shape Classification: **Flattening Bucket**

The retention curve is neither a classic *Leaky Bucket* (steady, unrelenting drain) nor a *Smiling Curve* (retention recovers after an initial dip). It is a **Flattening Bucket**: a sharp, front-loaded drop followed by a decelerating but persistent decline that has not yet reached a true asymptote within the 8-month window.

### Phase 1 — Steep M1 Drop-off (−56.0 pp)

More than half the user base — 56 out of every 100 acquired users — does not return after their first transaction month. This is the single largest attrition event in the lifecycle and the most actionable opportunity. Common causes include:

- No post-purchase re-engagement flow (email, push, in-app)
- Product failed to deliver on its first-session promise
- No perceived reason to return within 30 days

### Phase 2 — M1–M3 Deceleration (−10.6 pp, then −4.7 pp)

Between M1 and M3, the drop-off rate slows rapidly — from 10.6 pp in M1→M2 to 4.7 pp in M2→M3. This deceleration pattern shows that users who survive the first month are substantially "stickier", but the product has not yet found a retention floor. The cohorts are converging toward a shared decay trajectory, with very little cross-cohort variance (all nine cohorts track within ±0.2 pp of each other at every age).

### Phase 3 — M3+ Asymptotic Approach (−2.3 pp → −1.0 pp per month)

From M3 onward the per-month loss drops below 2.5 pp and continues to tighten. By M8 the curve is losing only ~1 pp per month. If this deceleration continues, the long-run retention floor is estimated at approximately **18–20 %** — not catastrophic, but still a significant ongoing leak.

The near-identical trajectories across all nine cohorts are noteworthy: cohort quality (the mix of intent, channel, and acquisition offer) is not materially changing month to month. Both the magnitude of M1 drop-off and the rate of stabilisation are essentially constant, suggesting the friction is structural (product/onboarding) rather than seasonal or campaign-driven.

---

## Strategic Recommendations

1. **Priority 1 — Redesign the M0→M1 re-engagement loop.** A 56 pp first-month drop is the dominant lever. A triggered Day-7 and Day-21 email/push sequence with a personalised "return offer" is the highest-ROI intervention. Target: bring M1 retention from 44 % to 55 %+.

2. **Priority 2 — Introduce a habit-forming trigger at M2–M3.** The curve's steepest deceleration window (M1–M3) is where retained users form or abandon habitual behaviour. A loyalty milestone, streak mechanic, or curated "what's new" nudge at the 6-week mark can accelerate flattening.

3. **Monitor for a Smiling Curve signal.** If M1 retention improves via re-engagement, watch whether M3+ rates also lift — a correlated improvement would reclassify the curve toward *Smiling* and signal that the product's core value is being discovered faster.

---

*Charts saved:* `findings/cohort_retention_heatmap.png` · `findings/retention_curve.png`  
*Data export:* `findings/cohort_retention_matrix.csv`
