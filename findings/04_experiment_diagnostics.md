# Experiment Diagnostics Report — Milestone 4

**Project:** A/B Testing & Product Experimentation Analysis  
**Author:** Akshobya698  
**Experiment:** Homepage A/B Test · 50,000 users · May 1–14 2024  
**Notebook:** `notebooks/04_ab_test_evaluation.ipynb`  
**Verdict: ⛔ DO NOT SHIP**

---

## 0 · Claim Being Audited

> *"Conversion lifted from 2.8% to 3.2% (p = 0.015), we must ship immediately!"*
> — Growth Team

The p-value is real. The conclusion is premature. Four diagnostic tests reveal the lift is narrower than required, the revenue guardrail is breached, and the result is driven entirely by desktop users.

---

## 1 · Sample Ratio Mismatch (SRM) — ✅ PASS

```
Observed : Control = 25,053   Variant = 24,947
Expected : Control = 25,000   Variant = 25,000
Chi-square: 0.2247   p = 0.6355
```

No SRM detected. The 50/50 randomisation mechanism worked correctly. The experiment data is trustworthy at the assignment level, and all downstream tests are valid.

---

## 2 · Primary Metric — Signup Conversion Z-Test

```
Control  : 712 / 25,053  =  2.84%
Variant  : 802 / 24,947  =  3.21%
Abs lift : +0.37 pp
Z-stat   : 2.433   p = 0.0150
95% CI   : [+0.07 pp, +0.67 pp]
```

**Statistically significant:** Yes — p = 0.015 replicates the Growth Team's claim exactly.

**⚠️ Does not clear the declared MDE:** The pre-registered Minimum Detectable Effect was **+0.5 pp**. The 95% Wald confidence interval lower bound is **+0.07 pp** — well below that threshold. The observed lift of +0.37 pp is real but too small to clear the commercial bar. Shipping based on significance alone, without meeting the MDE, exposes the company to a lift that may not justify engineering and operational costs.

---

## 3 · Guardrail Metric — 30-Day ARPU Welch's T-Test

```
Control ARPU : $3.3134  (std = $22.68)
Variant ARPU : $2.9163  (std = $18.60)
ARPU diff    : −$0.3971
Welch t-stat : −2.141   p = 0.0323
```

**🚨 ARPU GUARDRAIL BREACH.** The variant generates statistically significantly *less* revenue than control (p = 0.032, below α = 0.05). The new homepage design appears to attract lighter-spending users or disrupt the post-signup monetisation path. A ship that gains +0.37 pp in signups while losing $0.40 in average revenue per user could be net-negative for the business depending on LTV assumptions.

> **Note:** Revenue is heavily zero-inflated (>96% of users have $0 revenue). Welch's T-test with `equal_var=False` is the correct test given the dramatically different standard deviations (Control σ=$22.68 vs Variant σ=$18.60).

---

## 4 · Temporal Stability — Novelty / Primacy Decay Check

```
Week 1 lift (May 1–7)  : +0.47 pp
Week 2 lift (May 8–14) : +0.27 pp
Novelty fade           : +0.20 pp
```

**✅ Within tolerance** — a 0.20 pp week-over-week fade does not trigger a formal novelty concern (threshold: > 0.3 pp). However, the directional decay is noteworthy: lift dropped ~43% from Week 1 to Week 2. The two-week experiment window is short; extending the run to 4 weeks would disambiguate novelty from a genuine durable effect.

---

## 5 · Segment Breakdown — Desktop vs Mobile

| Device | Control CR | Variant CR | Abs Lift | p-value | Verdict |
|---|---:|---:|---:|---:|---|
| Desktop | 3.39% | 4.31% | **+0.92 pp** | 0.0002 | ✅ Significant |
| Mobile | 2.29% | 2.13% | **−0.16 pp** | 0.398 | ❌ Not sig (slight harm) |

**Critical finding: the aggregate lift masks a Simpson's Paradox-like heterogeneity.**

- The variant is **strongly positive for Desktop** users (+0.92 pp, p < 0.001) — this is the signal driving the headline result.
- The variant is **flat-to-slightly-negative for Mobile** users (−0.16 pp, p = 0.40). Mobile represents 50% of the experiment population.

Shipping the variant globally would improve desktop conversion while leaving mobile users with a marginally worse (or unchanged) experience, and mobile already converts at only 2.13–2.29%. This design should be shipped **desktop-only** while a mobile-optimised variant is built and tested separately.

---

## 6 · Consolidated Audit Verdict

| Test | Result | Decision |
|---|---|---|
| SRM Check | ✅ PASS (p = 0.636) | Data integrity confirmed |
| Conversion Z-Test | ✅ Sig (p = 0.015) | Significant but... |
| Clears MDE (+0.5 pp) | ❌ FAIL — CI lower = +0.07 pp | Lift too small to meet bar |
| ARPU Guardrail | 🚨 BREACH (p = 0.032) | Revenue degradation |
| Novelty Decay | ✅ Stable (fade = 0.20 pp) | Acceptable |
| Mobile Segment | ❌ Negative (−0.16 pp) | Do not ship globally |

**Recommended Actions:**

1. **Do not ship globally.** ARPU breach + failure to clear MDE + mobile harm together constitute a clear hold signal.
2. **Ship desktop-only** as a fast-follow: the desktop lift (+0.92 pp) is large, significant, and clears the MDE for that segment.
3. **Extend to 4 weeks** if a global ship is still being evaluated, to resolve the novelty ambiguity.
4. **Instrument revenue attribution**: investigate whether the ARPU decline is driven by user quality (cheaper acquisition) or by the homepage design suppressing upsell paths.

---

*Chart saved:* `findings/ab_test_dashboard.png`
