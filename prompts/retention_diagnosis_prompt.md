# 🤖 AI Prompt: QSR Retention Diagnosis & Action Plan

**Role:** Senior Product + Data Analyst
**Context:** A QSR brand "PizzaHub" (45 outlets) saw a 28% drop in repeat customer orders in the last 2 months despite regular campaigns.
**Objective:** Diagnose drivers and recommend actions.

---

## 📂 DATA I WILL PROVIDE (all CSV)

**1) customers.csv** (exported from Reelo → Customer Insights → Customer List)
* **Columns:** `customer_id` (derived if absent), `name`, `email`, `phone`, `gender`, `birthday`, `anniversary`, `address`, `last_visited_at` (UTC), `rfm_segment`, `total_spent`, `avg_spend`, `total_visits`, `points_balance`, `redemptions`, `customer_tags`, `average_tbo_in_days`.

**2) sales_kpis.csv** (from Dashboard or POS export)
* **Columns:** `date`, `store_id`, `store_name`, `total_orders`, `repeat_orders`, `total_sales`, `rewards_redeemed`.

**3) campaign_perf.csv** (from Campaigns → Campaign Performance)
* **Columns:** `date`, `campaign_id`, `campaign_name`, `channel` (sms/whatsapp/email), `audience_segment`, `messages_sent`, `delivered`, `failed`, `visits`, `revenue`, `avg_visit_rate`.

**4) loyalty_config.csv** (from Loyalty settings)
* **Columns:** `program_type` (cashback/amount_spent/visit_made), `earn_rule`, `redeem_rule`, `min_points_to_redeem`, `points_expiry_days`, `issuance_multiplier`.

---

## 🔍 WHAT TO ANALYZE

### A. Trend & Composition
* **Trend Analysis:** specific trend of `repeat_orders` and repeat rate by week and by store. Compare last 8 weeks vs prior 8 weeks.
* **Customer Mix:** Shift in customer mix (new vs repeat) and **RFM segment transitions** (e.g., Loyal → At Risk → Lost).
* **Time Between Orders (TBO):** Calculate median/95th percentile by segment; identify where it stretched.

### B. Campaign Effectiveness
* For each campaign: delivery rate, visits lift (vs baseline), revenue per message, repeat conversion rate.
* **Targeting Check:** Did "at_risk" / "about_to_sleep" audiences receive targeted campaigns? Measure lift vs control if available; otherwise pre/post.

### C. Loyalty Program
* **Breakage Risk:** Analyze points accrual vs redemption rate by segment; identify proportion with high `points_balance` but no recent visits.
* **Rule Impact:** Any changes to earn/redeem rules correlating with the drop (e.g., higher threshold, expiry).

### D. Store/Cluster Effects
* Which stores contributed most to the decline? Control for footfall by new customers and local factors.

### E. Hypotheses to Validate
* **Hypothesis 1:** Campaigns under-delivered (e.g., WhatsApp undelivered) → low visit rate.
* **Hypothesis 2:** Points redemption friction or expiry → disengagement.
* **Hypothesis 3:** Price/menu changes (proxy via `avg_spend` shift) → reduced repeat.
* **Hypothesis 4:** Data capture issues (drop in valid numbers) → campaign reach fell.

---

## 📝 OUTPUT REQUIRED

1.  **Executive Summary:** (≤10 bullets) Key drivers of the 28% drop.
2.  **Actionable Recommendations:** 3–5 most actionable recommendations with estimated impact and the specific data signals that support them.
3.  **Tables:**
    * Weekly repeat rate by store cluster (top 10 deltas).
    * Segment transition matrix (Loyal → At Risk → Lost, etc.).
    * Campaign ROI table (`messages_sent`, `delivery%`, `visits`, `revenue/message`).
4.  **Visuals Descriptions:** Titles + what they'd show (e.g., trend lines, bar charts, and a Sankey for segment transitions).
5.  **Data Quality Notes:** Missing columns, date gaps, outliers.
6.  **Next Data Requirements:** What to pull next if needed (e.g., raw transaction log, item-level basket, store hours).

---

## ⚙️ ASSUMPTIONS
* If `repeat_orders` is absent, infer repeat via `total_visits > 1` or by comparing `last_visited_at` and visit counts.
* Treat `phone` as primary identifier if `customer_id` is missing; deduplicate by phone/email.
* Use UTC; convert if timestamps include timezone.