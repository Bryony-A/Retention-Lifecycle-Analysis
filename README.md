# Retention-Lifecycle-Analysis

# Executive Overview

This project analyzes a 10,000-customer base for Nordlane, a fictional direct-to-consumer home goods retailer, using RFM segmentation to identify where customer value is concentrated and which segments show the strongest signs of disengagement. The analysis translates a cross-sectional snapshot of purchase behavior into nine actionable customer segments, then uses those segments to answer a specific, scoped question: within a fixed retention budget, where should spend be concentrated to protect the most revenue?

#### Tools Used

- Excel
- Power BI

# Business Context

**Company:** *Nordlane*, a mid-market e-commerce retailer selling home goods and lifestyle products online, operates across North America, Latin America, Europe, South Africa, and APAC.  The retailer has ~$40–60M in annual revenue.

Nordlane's Growth Marketing team runs a standing retention budget aimed at reducing customer disengagement before it becomes churn. That budget is currently allocated without a clear view of which customers are both at risk and high-value. Decisions have been made on recency alone, without weighing frequency or monetary contribution, and without visibility into where in the lifecycle customers begin to quietly disengage. This analysis gives the team a segmentation layer to target that existing budget more precisely, rather than spreading it evenly or reactively across whoever looks recently inactive.

**My role:** Analyst on the **Retention & Lifecycle team**, reporting into Growth Marketing. 

### **Stakeholders:**

- **Growth Marketing Lead** — owns the retention budget and needs to decide which customer segments to prioritize for win-back and retention campaigns this quarter.
- **CRM / Lifecycle Marketing Manager** — will execute segment-specific campaigns and needs segment definitions precise enough to build targeting rules from.

# Business Question

> Within Nordlane's fixed quarterly retention budget, which 2–3 customer segments should the Growth Marketing Lead prioritize to protect the greatest amount of revenue currently at risk of disengagement?
> 

---

## Preview
![Dashboard Screenshot]()

# Key Findings

**1. Cannot Lose Them offers the best revenue-per-customer-targeted ratio.**
At £1.93K average proxy revenue per customer, *Cannot Lose Them(CLT)* customers are worth more than 2x as much individually as the *At Risk(AT)* segment (£0.84K).

Despite AT holding more aggregate revenue with £1.5M compared to CLT with £0.7M simply by virtue of size, AT being 29% larger. For a budget that has to be spent per-customer (calls, personalised offers, account management), *Cannot Lose Them* is the more efficient allocation; for a budget spent at scale (automated email/CRM), *At Risk* reaches more revenue per pound spent on outreach.

**2. Cart abandonment and site engagement move together with churn, and neither is recency-derived.**
Churned customers show a 63.3% average cart abandonment rate against 48.5% for retained customers, and 12.49 average monthly site visits against 15.96 for retained customers. Because these fields are independent of the recency component that defines churn in this dataset, this is a genuine signal. One that the CRM/Lifecycle Marketing Manager can act on directly as a trigger for win-back automation, ahead of a customer formally lapsing into an At Risk Segment or Lost.

**3. Risk is concentrated in mid-tenure customers, not the newest or longest-tenured.**
When normalized as a share of each tenure band's full customer count. Gold 5+ is the safest cohort (55.4% Safe, 29.0% in the At Risk Segments bucket, 15.6% Lost, n=5,786). Silver 2-4 has the highest At Risk Segments share (50.5%, n=2,239), and Bronze 0-1 has the highest Lost share (28.1%, n=1,975). The pattern is a hump rather than a straight line. 

Customers who've moved past onboarding but haven't yet consolidated into loyalty appear to be the point where the relationship is most likely to erode. The dataset can show that this pattern exists but not why.

> **At-risk Segments:** Defined as *Needs Attention*, *At Risk*, *Cannot Lose Them,* and *Hibernating.* (Showing declining engagement where retention intervention is still plausible. *Recent, Lw Value* is excluded as these customers haven't yet established a behavioral pattern; *Lost* is excluded as recovery is unlikely to be cost-effective.
>

**4. Needs Attention is the segment most worth catching before it becomes At Risk.**
At £1.2M in revenue and £1.00K average per customer, *Needs Attention* sits just below *At Risk* in per-customer value but is, by definition, not yet churned. 

**5. Hibernating and Lost are not good candidates for paid retention spend.**
At £0.52K and £0.33K average revenue per customer respectively, these segments do not clear a reasonable per-customer acquisition-equivalent cost. 

# Recommendations

| Priority | Segment | Owner | Action |
| --- | --- | --- | --- |
| 1 | Cannot Lose Them | Growth Marketing Lead | High-touch retention (personal outreach, tailored offers) — best £-protected per customer targeted |
| 2 | At Risk | Growth Marketing Lead | Scaled CRM campaign — lower per-customer value but largest addressable revenue pool |
| 3 | Needs Attention | CRM/Lifecycle Marketing Manager | Proactive lifecycle nudges before lapse, triggered ahead of the churn point |
| 4 | Silver 2-4 tenure cohort | CRM/Lifecycle Marketing Manager | Qualitative research into mid-lifecycle drop-off before designing a campaign; Bronze 0-1 also worth watching for its high Lost share |
| 5 | Hibernating / Lost | CRM/Lifecycle Marketing Manager | Low-cost automated reactivation only; deprioritise for budgeted spend |

**Cross-cutting recommendation:** Build cart abandonment rate and monthly site visit frequency into an early-warning trigger, rather than waiting for a customer's RFM recency score to formally shift them into At Risk. The gap between retained and churned customers on both metrics is wide enough to act as a leading indicator, and using it moves the CRM team's intervention earlier in the lifecycle than the segment labels alone allow.

## Note on Scope

These recommendations are directional, based on relative revenue-per-customer and observed correlations. They are not a formal ROI or campaign-cost model — the dataset has no CAC or campaign-cost field, which is why the analysis is framed as a targeting decision within a fixed budget rather than a budget-sizing exercise. See the Limitations section for the full set of data constraints this analysis operates under.

---

# Methodology & Data
[Kaggle Dataset Link](https://www.kaggle.com/datasets/fares279/customers-transactions)

### Data Model

|  |  |
| --- | --- |
| Grain |   • 1 row per `customer_id`  10,000 rows <br/> (IDs 1–10,000 • All unique, no duplicates) |
| One Row |   **• One customer, as of a single snapshot in time.** <br/> • This is not transaction-level data. There is no order table, no line items, no repeat observations of the same customer over time. <br/> • Every metric (`num_purchases`, `avg_purchase_value`, `website_visits_per_month`, `cart_abandon_rate`) is a pre aggregated lifetime or monthly-average figure, not raw events you can re-cut. |
| Date range | `last_purchase_date`  <br/> • spans 2023-10-31 to 2025-10-27 (~2 years) <br/> • It's the *only* date field — there's no signup date, no order date, no churn date. |

### Key entities/fields:

|  |  |
| --- | --- |
| Demographics | `age` (18–70) <br/> `gender` (M/F only) <br/> `country` (10 countries, US-heavy at 24%) |
| Value/engagement | `annual_income` <br/> `spending_score` (1–100, unclear derivation) <br/> `num_purchases` (1–49, lifetime) <br/> `avg_purchase_value` ($16.75–$83.27) <br/> `membership_years` (0–15) <br/> `website_visits_per_month` <br/> `cart_abandon_rate` (0–1) |
| Outcome | `churned` (binary, 10.9% positive) |
| Feedback text |  • Only 12 distinct canned phrases across 10,000 rows (e.g., "Excellent customer service.") |

### Calculated Fields

| Field  | Formula |
| --- | --- |
| Proxy_Revenue | `=[@[Avg_Purchase_Value]]*[@[Num_Purchases]]` |
| Loyalty_Tier | `=IF([@[Membership_Years]] >= 5, "Gold 5+", IF([@[Membership_Years]] >= 2, "Silver 2-4", "Bronze 0-1"))` |
| Recency(Days) | `=DATE(2025,10,27)-[@[Last_Purchase_Date]]` |

---

## Recency-Churn Relationship

![Pivot Table Screenshot](https://github.com/Bryony-A/Retention-Lifecycle-Analysis/blob/main/Screenshots/Pivot%20Chart.png)

A pivot table cross-tabulating `Last_Purchase_Date` (recency) against `churned` shows a clean threshold effect: every customer with `churned = 0` falls within 0–178 days since last purchase, while every customer with `churned = 1` falls at 179+ days. 

![Correlation Matrix Screenshot]()

The correlation between the two variables is 0.84. This pattern indicates that `churned` was likely operationalised as a recency-based proxy (no purchase within ~180 days) rather than derived from an independent behavioural or transactional signal. As a result, any segment-level analysis of churn is expected to closely mirror recency, since the two are effectively measuring the same underlying construct.

## RFM(+E) Methodology

| Dimension | Field | Why |
| --- | --- | --- |
| Recency | `Last_Purchase_Date` → days since 27 Oct 2025 | Core lifecycle signal |
| Frequency | `Num_Purchases` | Lifetime purchase count (cumulative) |
| Monetary | `Avg_Purchase_Value` x `membership_years` | Spend behaviour (Proxy) |
| Engagement | `Monthly_Website_Visits`, `Cart_Abandon_Rate` | Indicator layer |

#### RFM Score (quintile-based, using `RANK.EQ`)

```
Recency_Score  = 6 - CEILING(RANK.EQ(E2,$E$2:$E$1000,1)/(COUNT($E$2:$E$1000)/5),1)
Frequency_Score = CEILING(RANK.EQ(F2,$F$2:$F$1000,1)/(COUNT($F$2:$F$1000)/5),1)
Monetary_Score  = CEILING(RANK.EQ(G2,$G$2:$G$1000,1)/(COUNT($G$2:$G$1000)/5),1)
```

#### Computing an average of F and M scores

```
FM_Score = ROUND(AVERAGE(Frequency_Score, Monetary_Score), 0)
```

#### Reasoning

<aside>
👉🏾

Standard RFM segmentation assumes Frequency and Monetary reflect *recent* behavior, not lifetime totals. Without windowing, the model conflates two very different customers:

- A customer who made 20 purchases two years ago and has bought nothing since (functionally churned)
- A customer who made 20 purchases steadily and is still active today

Both would score identically on Frequency and Monetary (F=5, M=5), differing only on Recency. In effect, this dataset's RFM implementation collapses toward a **Recency-driven segmentation with F/M as static tiebreakers**, rather than the three genuinely independent dimensions RFM is designed to capture. A "Champion" segment (555) built on this data may actually contain a meaningful share of dormant high-lifetime-value customers who look engaged only because their historical volume was large, not because it's ongoing.

</aside>

#### RFM segment

```
=R_Score&FM_Score
```

Gives strings like `"5-1"` (best customers) or `"1-1"` (at risk / churned), which I used to then map to named segments with a lookup table (Champions, At Risk, Lost, etc.) via `XLOOKUP`.

| R / FM | 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- | --- |
| 5 | Recent, Low Value | Recent, Low Value | Potential Loyalist | Loyal Customer | Champion |
| 4 | Recent, Low Value | Potential Loyalist | Potential Loyalist | Loyal Customer | Champion |
| 3 | At Risk | Needs Attention | Needs Attention | Loyal Customer | Loyal Customer |
| 2 | Hibernating | At Risk | At Risk | Needs Attention | Loyal Customer |
| 1 | Lost | Hibernating | At Risk | At Risk | Cannot Lose Them |

| RFM_Key | Segment |
| --- | --- |
| 5-5 | Champion |
| 5-4 | Loyal Customer |
| 5-3 | Potential Loyalist |
| 5-2 | Recent, Low Value |
| 5-1 | Recent, Low Value |
| 4-5 | Champion |
| 4-4 | Loyal Customer |
| 4-3 | Potential Loyalist |
| 4-2 | Potential Loyalist |
| 4-1 | Recent, Low Value |
| 3-5 | Loyal Customer |
| 3-4 | Loyal Customer |
| 3-3 | Needs Attention |
| 3-2 | Needs Attention |
| 3-1 | At Risk |
| 2-5 | Loyal Customer |
| 2-4 | Needs Attention |
| 2-3 | At Risk |
| 2-2 | At Risk |
| 2-1 | Hibernating |
| 1-5 | Cannot Lose Them |
| 1-4 | At Risk |
| 1-3 | At Risk |
| 1-2 | Hibernating |
| 1-1 | Lost |

# Segmentation Names and Definitions

| Name | Definition | Score |
| --- | --- | --- |
|  1. Champion  | Best customers - bought recently, buy often, and spend the most | High R, High F, High M |
| 2. Loyal Customer | Consistent repeat buyers who respond well to engagement, just shy of Champion-level spend or recency | High F, High M, mid-high R |
| 3. Potential Loyalist  | Recent customers with above-average frequency — early signs of becoming Loyal, not yet proven. | High R, mid F, mid M |
| 4. Recent, Low value  | Newer / low-engagement customers who have bought recently but only once or twice, and spent little — too new to judge intent. | High R, Low F, Low M |
| 5. Needs Attention  | Above-average across the board, but recency has started slipping — early drift, not yet crisis | Mid R, mid-high F, mid-high M |
| 6. At Risk  | Used to purchase frequently, but recency has dropped meaningfully. | Low-mid R, High F, High M |
| 7. Cannot Lose Them  | Highest spenders and most frequent buyers, but recency has dropped further than At Risk — high value, going quiet. | Low R, Highest F, Highest M |
| 8. Hibernating  | Faded out with little value to recover. | Low R, Low F, Low M |
| 9. Lost  | Longest gap since last purchase, effectively churned. | Lowest R, Lowest F, Lowest M |

> Segment labels follow standard RFM naming conventions but should be interpreted with caution given lifetime (non-windowed) F/M inputs — see Limitations. A customer labeled "Champion" may reflect strong historical spend that has since gone dormant, distinguishable only via the Recency score component.
> 

---

# Limitations

### Structural limitations

Due to the synthetic nature of the dataset there are a few limitations that need to be stated

- Cross-sectional nature: no repeat purchase timestamps, no session-level history → true cohort retention curves and time-to-churn survival analysis are off the table.
- `feedback_text` is 12 canned phrases. No real NLP signal, this has been completely dropped from data
- **`churned` is functionally a recency threshold, not an independent outcome.** Customers with `churned = 1` never have a `last_purchase_date` after 2025-05-01, while `churned = 0` customers run through 2025-10-27.
- No signup date — `membership_years` is a rounded integer, so cohort assignment will be coarse (year-level at best, and only a proxy, not observed).
- No transaction ID, product ID, category, price, or channel — no market basket, no product-level, no marketing-attribution analysis is possible.
- `spending_score` is a vendor provided score, it has no stated formula.
- RFM segmentation is compromised by the absence of time-windowed data.

### Analytical exclusions

- **Causal drivers of churn.** Cross-sectional snapshot data can show correlation but can't establish that fixing cart abandonment *causes* retention.
- **Time series seasonality: T**he data doesn’t make it possible with one row per customer and no periodic re-observation.
- **A/B testing for any campaign:** With no campaign history any claim is a labelled assumption or a proposed test design. Not a measured result.
- **Product-level or channel-level insight** ("which products drive repeat purchase," "which acquisition channel retains best")

# Declared Assumptions

- No explicit meta data extract date is provided. Snapshot/reporting date is assumed to be `max(Last_Purchase_Date) = 2025-10-27`
- `churned = 1` is assumed to mean "no purchase in the ~6 months before snapshot" (inferred from the data pattern - See Descriptive Stats)
