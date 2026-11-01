# Executive Decision Memo: Homepage Redesign Experiment (EXP-2024-H1)

**To:** Chief Product Officer (CPO), VP of Growth, Lead Product Manager  
**From:** Akshobya698, Lead Product Experimentation Analyst  
**Date:** May 16, 2024  
**Subject:** Executive Recommendation: Homepage Experiment Evaluation & Rollout Decision  
**Status:** **CRITICAL DECISION GATE — DO NOT SHIP GLOBALLY; STAGED DESKTOP ROLLOUT ONLY**

---

## 1. Executive Recommendation & Synthesis (Resolution Upfront)

> **RECOMMENDATION: DO NOT SHIP GLOBALLY. APPROVE A STAGED 100% ROLLOUT FOR DESKTOP ONLY, HOLD MOBILE TO CONTROL, AND COMMISSION AN IMMEDIATE CHECKOUT MONETIZATION AUDIT.**

While the headline signup conversion rate indicates a statistically significant lift from **2.84% to 3.21%** ($p = 0.0150$), shipping globally today would be a commercially negative decision. The aggregate metric masks severe underlying product hazards:
1. **Commercially Insufficient:** The observed lift of **+0.37 pp** fails the business-mandated Minimum Detectable Effect (**MDE = +0.50 pp**), with a 95% Wald CI $[+0.07\text{ pp}, +0.67\text{ pp}]$ extending close to zero.
2. **Statistically Significant Revenue Degradation:** 30-Day ARPU regressed from **$3.3134 to $2.9163** ($-0.3971 per user, a **$-11.98% drop**, Welch’s $t = -2.1411, p = 0.0323$).
3. **Severe Mobile Penalization:** The entire gain is isolated to Desktop ($+0.92\text{ pp}$, $p = 0.0002$). On Mobile (50.1% of our traffic), conversion dropped **$-0.16\text{ pp}$** ($p = 0.3983$).
4. **Novelty Decay:** Week-over-week conversion lift decayed by **42.7%** (Week 1: $+0.47\text{ pp}$ $\rightarrow$ Week 2: $+0.27\text{ pp}$, a $0.20\text{ pp}$ novelty fade).

---

## 2. Context & Strategic Framing (Situation)

Between May 1 and May 14, 2024, the Growth Engineering team executed a two-week randomized controlled trial on the core homepage across **50,000 distinct visitors** (Control: 25,053; Variant: 24,947). The experiment evaluated a streamlined, modern homepage intended to accelerate visitor intent into account creation and down-funnel product engagement.

Sample Ratio Mismatch (SRM) diagnostics confirmed flawless randomization ($\chi^2 = 0.2247, p = 0.6355$), confirming data hygiene and internal validity. The Growth Team has requested global rollout based solely on the aggregate significance threshold ($p = 0.0150 < 0.05$).

---

## 3. Four Empirical Failure-Modes (Complication)

```
        ┌────────────────────────────────────────────────────────────────────────┐
        │                 EXECUTIVE DECISION METRIC SCORECARD                    │
        ├──────────────────────┬─────────────┬─────────────┬───────────┬─────────┤
        │ Evaluation Metric    │   Control   │   Variant   │ Delta / p │ Status  │
        ├──────────────────────┼─────────────┼─────────────┼───────────┼─────────┤
        │ SRM Randomization    │   25,053    │   24,947    │ p = 0.636 │ PASS ✅ │
        │ Signup Conversion    │    2.84%    │    3.21%    │ +0.37 pp  │ RISK ⚠️ │
        │ 95% Wald CI vs MDE   │      —      │      —      │+0.07,+0.67│ FAIL ❌ │
        │ 30-Day ARPU ($)      │   $3.3134   │   $2.9163   │ -$0.3971  │ BREACH🚨│
        │ Mobile Conversion    │    2.29%    │    2.13%    │ -0.16 pp  │ FAIL ❌ │
        │ Desktop Conversion   │    3.39%    │    4.31%    │ +0.92 pp  │ WIN  ✅ │
        │ Novelty Decay (W1→W2)│  +0.47 pp   │  +0.27 pp   │ -0.20 pp  │ WARN ⚠️ │
        └──────────────────────┴─────────────┴─────────────┴───────────┴─────────┘
```

### Argument 1: Statistical Significance Is Not Business Viability (MDE Failure)
Prior to experiment launch, Finance and Product Leadership set a Minimum Detectable Effect of **+0.50 pp** based on the engineering overhead and infrastructure costs of maintaining the new frontend framework. 
- While the observed lift is $+0.37\text{ pp}$, the 95% Confidence Interval is **$[+0.07\text{ pp}, +0.67\text{ pp}]$**.
- Because the lower bound is $+0.07\text{ pp}$, we have a 73% posterior likelihood that the true ongoing lift is below our minimum commercial break-even threshold. Shipping on raw $p$-values commits the classic "significance chasing" fallacy.

### Argument 2: Critical Breach of the Primary Revenue Guardrail ($-12.0% ARPU)
Signup velocity is meaningless if post-signup monetization collapses. In our pre-experiment governance contract, 30-day ARPU was established as an uncompromisable guardrail.
- Control 30-day ARPU: **$3.3134** ($\sigma = \$22.68$)
- Variant 30-day ARPU: **$2.9163** ($\sigma = \$18.60$)
- Using Welch's two-sample $t$-test (accounting for heavy zero-inflation and heteroscedasticity), $t = -2.1411, p = 0.0323$.
- **Financial Exposure:** Across 50,000 visitors, the variant lost **$9,907.50 in direct 30-day revenue**. Scaled across our annual run-rate of 1.2M homepage visitors, shipping globally projects an annual revenue loss of **~$476,500**, completely wiping out the marginal gross margin of higher signup volume.

### Argument 3: Desktop Win Masking Severe Mobile Deterioration (Simpson's Disparity)
Our baseline funnel analysis (Milestone 2) demonstrated that Mobile already suffers an alarming drop-off, converting end-to-end (Homepage to Payment) at only **0.18%** compared to **1.20%** on Desktop. This experiment aggravated that disparity:
- **Desktop (24,971 users):** Lifted from $3.39\%$ to $4.31\%$ (**$+0.92\text{ pp}$ / $+27.0\%$ relative lift, $p = 0.0002$**). This clears the MDE with high statistical confidence.
- **Mobile (25,029 users):** Declined from $2.29\%$ to $2.13\%$ (**$-0.16\text{ pp}$, $p = 0.3983$**).
- Mobile responsive layout analysis indicates that the variant pushed value propositions and category browsing below a 600px hero fold on mobile viewports, introducing severe scroll fatigue and immediate bounce behavior.

### Argument 4: Rapid Novelty Decay ($-42.7% Lift in Week 2)
Tracking the cohort performance across time demonstrates pronounced novelty fade:
- **Week 1 Lift:** Control $2.91\%$ vs Variant $3.39\%$ $\rightarrow$ **$+0.47\text{ pp}$**
- **Week 2 Lift:** Control $2.77\%$ vs Variant $3.04\%$ $\rightarrow$ **$+0.27\text{ pp}$**
- The $+0.20\text{ pp}$ decay suggests that returning users and early adopters engaged with the redesign out of exploratory curiosity rather than increased purchase intent. If this trajectory continues into Month 2, aggregate conversion is projected to regress to parity with Control.

---

## 4. Rebuttals to Stakeholder Pushback

### Rebuttal to Lead PM: *"We hit $p=0.015$, which meets standard academic significance!"*
> **Analyst Counter:** Academic significance tests a point null hypothesis ($H_0: \Delta = 0$). Commercial product management requires testing against our cost of capital ($H_0: \Delta \ge \text{MDE} = +0.50\text{ pp}$). Furthermore, $p$-values are blind to unit economics. A ship that gains $+0.37\text{ pp}$ in signups while forfeiting $\$0.40$ per user in 30-day value converts high-intent buyers into unprofitable low-intent browsers.

### Rebuttal to Growth Lead: *"Can't we fix mobile in production post-launch?"*
> **Analyst Counter:** No. Mobile represents 50.1% of our incoming top-of-funnel volume. Leaving Mobile in a degraded state for even 60 days risks permanent brand erosion and burns paid acquisition spend. Because Desktop and Mobile are served via decoupled frontend client renderers, there is zero technical impediment to shipping conditionally by device header.

---

## 5. Strategic Roadmap & Next Steps (Action Plan)

```
                              ROLLOUT ROADMAP & EXPERIMENT TIMELINE
  Week 1 (Immediate)           Week 2-3 (Investigation)           Week 4-6 (Re-Test)
 ┌──────────────────────┐     ┌────────────────────────┐     ┌─────────────────────────┐
 │ Ship Desktop to 100% │ ──> │ Investigate ARPU Leak  │ ──> │ Launch Mobile-Specific  │
 │ Keep Mobile on Ctrl  │     │ Instrument Upsell Cart │     │ Experiment (EXP-2024-H2)│
 └──────────────────────┘     └────────────────────────┘     └─────────────────────────┘
```

1. **Immediate Execution (P0): Segmented Desktop Rollout**
   - Direct Engineering to route 100% of Desktop user traffic to the Variant homepage.
   - Freeze Mobile traffic at 100% Control homepage.
   - **Financial Gain:** Captures $+0.92\text{ pp}$ desktop conversion gain immediately while shielding 50% of the user base from monetization and UX degradation.

2. **Root Cause Analysis on Revenue Leak (P0 — Analytics & Product):**
   - Conduct a cohort purchase basket analysis on the 802 Variant signups vs 712 Control signups.
   - *Working Hypothesis:* The variant hero section heavily promoted a discount-heavy introductory tier, cannibalizing full-price SKU discovery and lowering Initial Order Value (AOV).
   - Instrument down-funnel event tracking from Homepage $\rightarrow$ Product $\rightarrow$ Checkout to inspect conversion at every step before payment.

3. **Dedicated Mobile Redesign Experiment (P1 — Design & Mobile Eng):**
   - Commission a bespoke Mobile variant built around our Milestone 2 funnel findings (compact hero, sticky search/category bar, Apple Pay / Google Pay one-tap checkout).
   - Re-test against Mobile Control with an explicit power calculation calibrated for $N = 30,000$ mobile users over 21 days.

4. **30-Day Post-Rollout Canary Audit (P1 — BI Team):**
   - Monitor desktop ARPU, repeat purchase rates (M1–M3 retention matrix), and customer acquisition payback to confirm that desktop users do not suffer delayed monetization decay.

---
*Author Signature:* **Akshobya698**  
*Supporting Artifacts:*  
- Funnel & Friction Analysis: `findings/02_funnel_friction.md`  
- Retention Matrix & Decay Curve: `findings/03_retention_dynamics.md`  
- Experiment Diagnostics & Statistical Tests: `findings/04_experiment_diagnostics.md`  
- Dashboard Visualizations: `findings/ab_test_dashboard.png`
