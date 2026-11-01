# A/B Testing & Product Experimentation Analysis

[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Topfolio](https://img.shields.io/badge/Topfolio-Product%20Analytics-brightgreen)](https://github.com/Akshobya698/ab-testing-product-analytics)

**Author:** [Akshobya698](https://github.com/Akshobya698)  
**Domain:** Product Experimentation, Growth Analytics & Behavioral Economics  
**Tech Stack:** Python (`pandas`, `numpy`, `scipy.stats`, `statsmodels`), Data Visualization (`seaborn`, `matplotlib`, `plotly`), Jupyter Notebooks, Git & GitHub CLI

---

## Executive Summary & Core Verdict

This repository delivers an end-to-end product experimentation and lifecycle analytics portfolio evaluating user journeys, cohort retention, and a high-stakes 50,000-user homepage redesign A/B experiment.

```
       ┌────────────────────────────────────────────────────────────────────────┐
       │                       EXECUTIVE DECISION GATE                          │
       │                                                                        │
       │   PROPOSED ACTION: Ship 50k Homepage Redesign Globally                 │
       │   ANALYST VERDICT: ⛔ DO NOT SHIP GLOBALLY; DESKTOP ROLLOUT ONLY       │
       │                                                                        │
       │   • Statistical Significance: p = 0.0150 (< 0.05)                      │
       │   • Commercial MDE (+0.50 pp): FAILED (Lift = +0.37 pp, CI: 0.07-0.67) │
       │   • Revenue Guardrail (30d ARPU): BREACHED (p = 0.0323, -$0.40/user)   │
       │   • Device Divergence: Desktop (+0.92 pp) vs Mobile (-0.16 pp)         │
       │   • Novelty Decay: 42.7% lift decay between Week 1 and Week 2          │
       └────────────────────────────────────────────────────────────────────────┘
```

The analysis synthesizes three analytical pillars:
1. **Multi-Step Funnel & Friction Analysis:** Mapping a 5-stage e-commerce conversion path across 10,000 distinct users to pinpoint drop-off friction across Desktop and Mobile viewports.
2. **Monthly Cohort Retention & Lifecycle Dynamics:** Deriving monthly user acquisition cohorts (Jan–Sep 2024), computing triangular retention matrices, and classifying the platform's retention curve as a *Flattening Bucket*.
3. **A/B Test Statistical Evaluation & Diagnostics:** Conducting rigorous statistical testing on 50,000 randomized users—auditing Sample Ratio Mismatch (SRM), two-sample proportion Z-tests, 95% Wald confidence bounds, Welch's $t$-tests on right-skewed ARPU guardrails, novelty fade, and Simpson's Paradox device disparities.

---

## Repository Structure

```
.
├── README.md                                # Portfolio documentation & executive summary
├── homepage_ab_test.csv                     # Raw 50,000-user experiment dataset
├── notebooks/                               # Executable Jupyter notebooks with full outputs
│   ├── 02_funnel_analysis.ipynb             # Multi-step funnel cleaning, dedup & Plotly charts
│   ├── 03_cohort_retention.ipynb            # Cohort derivation, triangular matrix & Seaborn heatmaps
│   └── 04_ab_test_evaluation.ipynb          # Statistical tests (SRM, Z-test, Welch t-test, novelty)
└── findings/                                # Executive reports, visualizations & exports
    ├── 02_funnel_friction.md                # 630-word baseline funnel friction memo
    ├── 03_retention_dynamics.md             # 625-word retention dynamics & curve classification report
    ├── 04_experiment_diagnostics.md         # 840-word detailed statistical diagnostics audit
    ├── 05_executive_decision_memo.md        # CPO-facing 1-page decision memo (Minto Pyramid SCR)
    ├── funnel_overall.png                   # Overall 5-stage conversion funnel
    ├── funnel_device_split.png              # Funnel conversion rate by Desktop vs Mobile
    ├── funnel_dropoff_heatmap.png           # Stage drop-off rate heatmap
    ├── funnel_summary.csv                   # Stage metrics export (Overall, Desktop, Mobile)
    ├── cohort_retention_heatmap.png         # Triangular monthly cohort retention heatmap (%)
    ├── retention_curve.png                  # Cohort retention curves with M1-M3 decay & asymptote
    ├── cohort_retention_matrix.csv          # 9x9 Cohort retention percentage matrix
    └── ab_test_dashboard.png                # 4-panel diagnostic dashboard for the A/B test
```

---

## Milestone 2: Multi-Step Funnel & Friction Analysis

### Objective
Ingest raw multi-session clickstream tracking logs, deduplicate repeated events (`nunique()` vs `count()`), calculate cumulative and step-over-step conversion rates across five core stages (**Homepage $\rightarrow$ Product Page $\rightarrow$ Cart $\rightarrow$ Checkout $\rightarrow$ Payment**), and isolate device-specific bottlenecks.

### Methodology & Data Hygiene
Raw tracking tables log multiple pings if users refresh pages or navigate back and forth. Deduplicating per `(user_id, step)` ensures each user is counted at most once per funnel phase, avoiding inflated populations and distorted conversion rates.

### Funnel Summary Table

| Funnel Stage | Unique Users | Cumulative CR | Step-over-Step CR | Step Drop-off | Desktop CR | Mobile CR | Device Gap |
|---|---:|---:|---:|---:|---:|---:|---:|
| **Homepage** | 10,000 | 100.00% | — | — | 100.00% | 100.00% | 0.0 pp |
| **Product Page** | 6,388 | 63.88% | 63.88% | 36.12% | 67.87% | 59.00% | +8.87 pp |
| **Cart** | 2,447 | 24.47% | 38.31% | 61.69% | 42.35% | 32.62% | +9.73 pp |
| **Checkout** | 519 | 5.19% | 21.21% | 78.79% | 23.91% | 16.28% | +7.63 pp |
| **Payment** | 74 | 0.74% | 14.26% | 85.74% | 17.46% | 5.67% | **+11.79 pp** |

### Key Analytical Takeaways
- **Largest Absolute Volume Loss:** **Product Page $\rightarrow$ Cart** lost **3,941 users** (61.69% drop-off). Users browse items but abandon before cart addition, indicating friction in value proposition, pricing clarity, or weak primary CTAs.
- **Highest Friction Percentage:** **Checkout $\rightarrow$ Payment** suffers an **85.74% drop-off**, driven by payment gateway complexity and lack of native mobile wallet support.
- **The Mobile Penalty:** Mobile conversion severely lags Desktop at every transition, culminating in a **6.7× end-to-end penalty** (Desktop 1.20% vs Mobile 0.18%). Bringing Mobile Cart $\rightarrow$ Checkout parity to Desktop would recover ~400 checkout entries per 10,000 visitors.

### Visualizations
<p align="center">
  <img src="findings/funnel_overall.png" width="48%" alt="Overall Funnel" />
  <img src="findings/funnel_device_split.png" width="48%" alt="Device Split Funnel" />
</p>

*Full report available at [`findings/02_funnel_friction.md`](findings/02_funnel_friction.md).*

---

## Milestone 3: Monthly Cohort Retention & Lifecycle Dynamics

### Objective
Track 9 monthly acquisition cohorts (Jan 2024 to Sep 2024) across up to 8 months of follow-up. Derive acquisition months using windowed minimum transformations (`transform('min')`), calculate cohort age indices, render an annotated Seaborn heatmap, and classify the product's retention curve.

### Retention Matrix (%)

| Cohort Month | M0 | M1 | M2 | M3 | M4 | M5 | M6 | M7 | M8 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **2024-01** | 100.0% | 44.0% | 33.4% | 28.8% | 26.5% | 24.7% | 23.1% | 22.0% | 20.9% |
| **2024-02** | 100.0% | 44.0% | 33.5% | 28.8% | 26.4% | 24.6% | 23.0% | 21.9% | — |
| **2024-03** | 100.0% | 44.0% | 33.4% | 28.8% | 26.4% | 24.6% | 23.1% | — | — |
| **2024-04** | 100.0% | 44.0% | 33.4% | 28.8% | 26.5% | 24.7% | — | — | — |
| **2024-05** | 100.0% | 44.0% | 33.4% | 28.7% | 26.4% | — | — | — | — |
| **2024-06** | 100.0% | 44.0% | 33.4% | 28.8% | — | — | — | — | — |
| **2024-07** | 100.0% | 44.0% | 33.4% | — | — | — | — | — | — |
| **2024-08** | 100.0% | 44.0% | — | — | — | — | — | — | — |
| **2024-09** | 100.0% | — | — | — | — | — | — | — | — |
| **Average** | **100.0%** | **44.0%** | **33.4%** | **28.7%** | **26.4%** | **24.6%** | **23.1%** | **21.9%** | **20.9%** |

### Curve Classification: Flattening Bucket
- **M0 $\rightarrow$ M1 Drop:** $-56.0\text{ pp}$ cliff. More than half of acquired users do not make a return transaction in month 1.
- **M1 $\rightarrow$ M3 Deceleration:** Decay slows dramatically from $-10.6\text{ pp}$ (M1 $\rightarrow$ M2) to $-4.7\text{ pp}$ (M2 $\rightarrow$ M3).
- **M3+ Asymptote:** Decay flattens to $\le 1.5\text{ pp}$ monthly loss, trending toward a long-term retention asymptote of **$\sim 20.9\%$**.
- **Cohort Stability:** Identical retention across all 9 cohorts confirms that acquisition channel quality is consistent and retention friction is driven by core product onboarding rather than seasonal marketing fluctuations.

### Visualizations
<p align="center">
  <img src="findings/cohort_retention_heatmap.png" width="58%" alt="Cohort Retention Heatmap" />
  <img src="findings/retention_curve.png" width="38%" alt="Retention Curve" />
</p>

*Full report available at [`findings/03_retention_dynamics.md`](findings/03_retention_dynamics.md).*

---

## Milestone 4: A/B Test Statistical Evaluation & Diagnostics

### Objective
Audit the Growth Team’s claim that the homepage redesign achieved a statistically significant lift ($p = 0.0150$) and is ready for immediate 100% production rollout across 50,000 users.

### Diagnostic Evaluation Framework

```
                          EXPERIMENT AUDIT FLOWCHART
  50,000 Users
 ┌────────────┐     1. Check SRM (Chi-square)
 │ Raw Data   │ ─────────────────────────────────> ✅ PASS (Chi2=0.225, p=0.6355)
 └────────────┘
       │
       ▼            2. Primary Metric (Z-test)
 ┌────────────┐ ─────────────────────────────────> ⚠️ SIG (p=0.0150) BUT
 │ Conversion │                                    ❌ FAILS MDE (+0.37pp vs +0.50pp)
 └────────────┘
       │
       ▼            3. Guardrail Metric (Welch's t)
 ┌────────────┐ ─────────────────────────────────> 🚨 BREACH (p=0.0323, -$0.40 ARPU)
 │ 30d ARPU   │
 └────────────┘
       │
       ▼            4. Segmentation (Desktop vs Mobile)
 ┌────────────┐ ─────────────────────────────────> ✅ Desktop: +0.92 pp (p=0.0002)
 │ Segments   │                                    ❌ Mobile : -0.16 pp (p=0.3983)
 └────────────┘
```

### Statistical Test Summary

| Diagnostic Test | Statistical Method | Control ($N=25,053$) | Variant ($N=24,947$) | Delta / Effect | Test Statistic | $p$-value | Decision |
|---|---|---:|---:|---:|---:|---:|:---:|
| **Sample Ratio Mismatch** | Goodness-of-fit $\chi^2$ | 25,053 | 24,947 | $-106$ split | $\chi^2 = 0.2247$ | $0.6355$ | **PASS ✅** |
| **Signup Conversion Rate** | Two-Sample Proportion Z-Test | $2.84\%$ ($712$) | $3.21\%$ ($802$) | $+0.37\text{ pp}$ ($+13.1\%$) | $Z = 2.4326$ | $0.0150$ | **SIG ✅** |
| **95% Wald Confidence Interval** | Normal Approximation CI | — | — | $[+0.07\text{ pp}, +0.67\text{ pp}]$ | $Z_{0.975} = 1.96$ | — | **FAILS MDE ❌** |
| **30-Day ARPU (Guardrail)** | Welch’s Two-Sample $T$-Test | $\$3.3134$ | $\$2.9163$ | $-\$0.3971$ ($-12.0\%$) | $t = -2.1411$ | $0.0323$ | **BREACH 🚨** |
| **Novelty Decay** | Temporal Week Split | W1: $+0.47\text{ pp}$ | W2: $+0.27\text{ pp}$ | $-0.20\text{ pp}$ ($42.7\%$ fade) | — | — | **CAUTION ⚠️** |
| **Desktop Segment** | Segment Two-Sample Z-Test | $3.39\%$ ($426/12,553$) | $4.31\%$ ($535/12,418$) | $+0.92\text{ pp}$ ($+27.0\%$) | $Z = 3.766$ | $0.0002$ | **STRONG WIN ✅** |
| **Mobile Segment** | Segment Two-Sample Z-Test | $2.29\%$ ($286/12,500$) | $2.13\%$ ($267/12,529$) | $-0.16\text{ pp}$ ($-6.9\%$) | $Z = -0.845$ | $0.3983$ | **NET HARM ❌** |

### Four Critical Findings
1. **The MDE Trap:** The lower bound of the 95% CI is $+0.07\text{ pp}$. The likelihood that true ongoing lift clears the commercial break-even threshold ($+0.50\text{ pp}$) is under 27%.
2. **Monetization Cannibalization:** The variant attracted users who converted to signups but exhibited significantly lower down-funnel spending, resulting in an annualized projected loss of **~$\$$476,500**.
3. **Simpson's Paradox in Device Mix:** The aggregate $+0.37\text{ pp}$ is an artifact of high-performing Desktop traffic masking negative lift on Mobile.
4. **Novelty Erosion:** More than 40% of the initial lift vanished between Week 1 and Week 2, signaling exploratory novelty rather than fundamental intent improvement.

### Diagnostic Visualizations
<p align="center">
  <img src="findings/ab_test_dashboard.png" width="90%" alt="A/B Test Diagnostic Dashboard" />
</p>

*Full report available at [`findings/04_experiment_diagnostics.md`](findings/04_experiment_diagnostics.md).*

---

## Milestone 5: Executive Decision Memo & Rollout Strategy

### Executive Recommendation
The formal recommendation delivered to the Chief Product Officer, VP of Growth, and Lead PM in [`findings/05_executive_decision_memo.md`](findings/05_executive_decision_memo.md) follows the **Minto Pyramid (Situation-Complication-Resolution)** structure:

> **RECOMMENDATION: DO NOT SHIP GLOBALLY. IMPLEMENT A CONDITIONAL 100% DESKTOP ROLLOUT, FREEZE MOBILE TRAFFIC ON CONTROL, AND COMMISSION AN IMMEDIATE CHECKOUT MONETIZATION AUDIT.**

### Strategic Action Plan & Roadmap

```
  Phase 1: Immediate (Week 1)
  ├── Route 100% Desktop traffic to Variant homepage (capture +0.92 pp lift)
  └── Pin 100% Mobile traffic to Control homepage (protect 50% user base from ARPU leak)

  Phase 2: Investigation (Weeks 2-3)
  ├── Basket analysis on the 802 Variant signups to identify discount SKU cannibalization
  └── Instrument granular sub-step telemetry between Homepage and Checkout

  Phase 3: Remediation & Re-Test (Weeks 4-6)
  ├── Design mobile-first variant with compact hero fold & native Apple/Google Pay integration
  └── Power mobile-specific A/B experiment calibrated for N = 30,000 over 21 days
```

---

## Reproducibility & Environment Setup

All data pipelines, statistical calculations, and visualizations are 100% reproducible:

### Prerequisites
- Python 3.10+
- Virtual environment or Conda

### Installation
```bash
# Clone the repository
git clone https://github.com/Akshobya698/ab-testing-product-analytics.git
cd ab-testing-product-analytics

# Install required dependencies
pip install pandas numpy scipy statsmodels matplotlib seaborn plotly kaleido jupyter nbconvert
```

### Running the Notebooks
```bash
# Execute Funnel Analysis (Milestone 2)
jupyter nbconvert --to notebook --execute --inplace notebooks/02_funnel_analysis.ipynb

# Execute Cohort Retention Matrix (Milestone 3)
jupyter nbconvert --to notebook --execute --inplace notebooks/03_cohort_retention.ipynb

# Execute A/B Test Statistical Evaluation (Milestone 4)
jupyter nbconvert --to notebook --execute --inplace notebooks/04_ab_test_evaluation.ipynb
```

---

## Project Artifacts Directory

| Artifact | Location | Description |
|---|---|---|
| **Executive Decision Memo** | [`findings/05_executive_decision_memo.md`](findings/05_executive_decision_memo.md) | CPO-facing 1-page Minto SCR recommendation memo |
| **Statistical Diagnostics Memo** | [`findings/04_experiment_diagnostics.md`](findings/04_experiment_diagnostics.md) | In-depth audit of SRM, Z-tests, Welch's $t$, and segments |
| **Retention Dynamics Memo** | [`findings/03_retention_dynamics.md`](findings/03_retention_dynamics.md) | Analysis of 9-month retention matrices & decay classification |
| **Funnel Friction Memo** | [`findings/02_funnel_friction.md`](findings/02_funnel_friction.md) | 5-stage e-commerce funnel and mobile penalty breakdown |
| **Diagnostic Dashboard** | [`findings/ab_test_dashboard.png`](findings/ab_test_dashboard.png) | 4-panel visual audit of the homepage experiment |
| **Cohort Heatmap** | [`findings/cohort_retention_heatmap.png`](findings/cohort_retention_heatmap.png) | Annotated Seaborn monthly cohort retention heatmap |
| **Funnel Visualizations** | [`findings/funnel_overall.png`](findings/funnel_overall.png) | Plotly purchase funnel conversion diagrams |

---
*Maintained by [Akshobya698](https://github.com/Akshobya698). Built for Topfolio Product Analytics Specialization.*
