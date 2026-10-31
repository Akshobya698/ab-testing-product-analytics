# Funnel Friction Report — Milestone 2

**Project:** A/B Testing & Product Experimentation Analysis  
**Author:** Akshobya698  
**Analysis window:** 30-day clickstream sample · 10,000 unique Homepage visitors  
**Notebook:** `notebooks/02_funnel_analysis.ipynb`

---

## Executive Summary

A 5-stage clickstream funnel was constructed from raw multi-session event logs. After deduplicating repeated pings (each `user_id` counted once per stage), the overall end-to-end conversion from Homepage to Payment stands at **0.74%** — only 74 users out of 10,000 complete a purchase. The analysis identifies two primary friction zones and a substantial mobile penalty concentrated at the Cart-to-Checkout transition.

---

## Funnel Metrics — All Users

| Stage | Unique Users | Cumulative CR | Step-over-Step CR | Step Drop-off |
|---|---:|---:|---:|---:|
| Homepage | 10,000 | 100.00 % | — | — |
| Product Page | 6,388 | 63.88 % | 63.88 % | **36.12 %** |
| Cart | 2,447 | 24.47 % | 38.31 % | **61.69 %** |
| Checkout | 519 | 5.19 % | 21.21 % | **78.79 %** |
| Payment | 74 | 0.74 % | 14.26 % | **85.74 %** |

> User count is strictly non-increasing at every step — sanity check ✅

---

## Top Friction Stages

### 1 · Product Page → Cart (−3,941 users, 61.7 % drop-off)
This is the **single largest absolute volume loss** in the funnel: 3,941 users who explored a product page did not add an item to cart. This points to weak purchase intent signals — likely insufficient social proof, unclear pricing, or a CTA that fails to convey urgency. This stage should be the **primary optimisation target**.

### 2 · Cart → Checkout (−1,928 users, 78.8 % drop-off)
Nearly four in five Cart visitors abandon before entering Checkout. Classic causes include unexpected shipping costs, mandatory account creation, or form friction. A checkout friction audit and a guest-checkout experiment are warranted here.

### 3 · Checkout → Payment (−445 users, 85.7 % drop-off)
The highest *percentage* drop-off, though with a smaller absolute user base. Payment failures, trust signals (SSL badges, accepted card logos), and limited payment method choice are likely culprits.

---

## Device Segmentation — Desktop vs Mobile

| Stage | Desktop CR | Mobile CR | Gap (pp) |
|---|---:|---:|---:|
| Product Page | 67.9 % | 59.0 % | **8.9** |
| Cart | 42.4 % | 32.6 % | **9.8** |
| Checkout | 23.9 % | 16.3 % | **7.6** |
| Payment | 17.5 % | 5.7 % | **11.8** ← sharpest |

Mobile's step-over-step conversion is consistently lower than Desktop's, but the **gap widens most sharply at Payment** (11.8 pp). End-to-end, Desktop users convert at **1.20 %** versus only **0.18 %** on Mobile — a **6.7× difference**. This suggests the mobile Checkout and Payment forms carry specific UX friction (small tap targets, difficult card-number entry, lack of Apple/Google Pay) rather than a uniform engagement deficit.

### Key device finding
The Cart → Checkout drop-off is 8 percentage points worse on Mobile (67.4 % vs 57.7 % on Desktop). If mobile UX at the Cart stage were brought to parity with Desktop, approximately **400 additional users** would enter Checkout per 10,000 Homepage visitors.

---

## Recommendations

1. **Immediate — A/B test Cart CTA copy and social proof placement** to reduce the largest absolute loss (Product Page → Cart).
2. **Short-term — Launch guest checkout on mobile** and remove the account-creation gate to address Cart → Checkout friction.
3. **Medium-term — Add mobile payment wallets** (Apple Pay / Google Pay) on the Payment screen; the 11.8 pp gap at Payment strongly suggests form-based payment is the bottleneck.
4. **Instrument sub-steps** within Checkout (address → shipping → payment details) to pinpoint the exact field where mobile users abandon.

---

*Charts saved:* `findings/funnel_overall.png` · `findings/funnel_device_split.png` · `findings/funnel_dropoff_heatmap.png`  
*Data export:* `findings/funnel_summary.csv`
