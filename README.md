# Olist Marketplace Performance & Customer Focused Analysis

*Prepared for Olist Operations & Customer Experience · Data window: Sep 2016 – Oct 2018 · 99,441 orders*

## Overview

An evidence-driven analysis of Olist's two-year order history, joining all 9 source tables (orders, items, payments, reviews, customers, products, sellers, geolocation, category translation) to answer three questions:

1. How has the marketplace actually performed over the available period?
2. Which factors are associated with customer satisfaction — and which are primary drivers vs. secondary contributors?
3. What should Olist prioritize as it scales to more sellers and regions?

**Scope:** 99,441 orders (96,478 delivered), 2016-09-04 to 2018-10-17.

## Approach

- One order-level "master" table built by aggregating line items, payments, and reviews, joined to customer/seller/product attributes.
- Geolocation aggregated to one mean lat/lng per zip-code prefix; seller–customer distance computed via a NumPy haversine formula.
- Methods: descriptive KPIs and monthly trends, segmentation by delivery timing/state/seller/category, and a standardized OLS regression of review score on five candidate drivers (delivery delay, freight share, distance, installments, item price) — **associative, not causal**.
- All charts in Matplotlib, all numeric work in NumPy; full code in a companion Jupyter notebook.

## Key Findings

**Delivery reliability is the dominant driver of customer focused.** It's the strongest signal across every cut of the data:
- 91.9% of orders arrive on time or early; average review score is **4.28 for on-time/early orders vs. 2.54 for late orders**.
- 1-star rate jumps from 6.8% (on-time) to 46.7% (late) — nearly a 7x increase.
- In a standardized multi-factor regression (n=95,993, R²=0.08), delivery delay's effect (-0.359) dwarfs distance (-0.081), freight share (-0.065), and price (-0.058).

**Growth is outpacing operational buffer at peak demand.** Order volume grew ~6.5x over the period; review scores dipped visibly during the Nov 2017–Mar 2018 demand spike (Black Friday/holidays), then recovered.

**Geography compounds the delivery problem but isn't the root cause.** Sellers are concentrated in SP/PR/MG (79% of sellers) while demand is national. Distance predicts freight cost (r=0.31) far more than it predicts delay (r=-0.07) — pointing to logistics *execution*, not raw geography, as the real lever.

**Category risk clusters in bulky/complex goods.** Furniture, audio, telephony, and construction categories underperform and are also the most delay-sensitive — the two problems reinforce each other.

**Seller quality varies meaningfully even after controlling for category/region.** Among sellers with ≥30 orders, average scores range from 2.3 to 5.0 (SD 0.32) — real, addressable seller-level variation.

**Payment behavior is a value signal, not a satisfaction signal.** Installments track order size (r=0.32 with value) but barely correlate with review score (r=-0.03); average scores are flat (~4.07) across all major payment types.

**Repeat customers are a small (3.1% of unique customers, 6.4% of orders) but valuable segment** worth protecting specifically, since they've already chosen to come back once.

## Recommendations (by expected impact)

1. **Tighten delivery-estimate accuracy and carrier SLAs**, starting with the highest delay-sensitivity states (CE, AL, PA, RJ, PE, SE, RN) rather than a uniform rollout.
2. **Build peak-season fulfillment capacity ahead of demand**, not reactively — pre-position carrier capacity and staffing before known high-volume windows.
3. **Recruit sellers in underserved regions** to shorten the logistics chain, cutting both cost and transit time for currently underserved, lower-scoring regions.
4. **Apply category-specific handling/listing standards** to bulky, high-risk categories (furniture, audio, home comfort, construction).
5. **Introduce a seller performance tier/scorecard** using the ≥30-order cohort as a baseline to identify and coach or demote underperformers.
6. **Protect the delivery experience of repeat customers** specifically, e.g. priority handling or proactive delay notifications.
7. **Do not prioritize payment-method/installment changes** as a satisfaction lever — that evidence points to conversion/AOV work instead, not CX.

## Methodology Caveat

All relationships are associative, drawn from observational data — not a controlled experiment. The regression controls for five tested factors against each other but not for unmeasured confounders (e.g. underlying product quality). Recommendations should be validated with small, monitored pilots before full rollout.