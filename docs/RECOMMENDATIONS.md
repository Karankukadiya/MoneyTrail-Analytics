# MoneyTrail Analytics — Recommendations

Each recommendation below is tied directly to a specific finding in
[`INSIGHTS.md`](./INSIGHTS.md), with a suggested next step and the
metric that would confirm whether it worked.

---

## 1. Investigate the accelerating account closure rate

**Finding:** Closure rate (accounts closed ÷ accounts opened, same year)
rose from 8.5% (2023) to 30.2% (2025) — a 3.5x increase — while account
opening growth is flattening (+57% in 2024, only +11% in 2025).

**Recommendation:** Treat this as the platform's top retention risk, not
just an account-count statistic. Segment closed accounts by:
- Account type (are NRI or Salary accounts closing faster than Savings?)
- Tenure (are accounts closing shortly after opening — an onboarding
  problem — or after years of use — a satisfaction problem?)
- Region (does the West region's negative balance correlate with higher
  closures there specifically?)

**Success metric:** Reduce closure rate back toward the 2023 baseline
(~8-10%) within two quarters of identifying and addressing the cause.

---

## 2. Reduce Net Banking and ATM failure rates

**Finding:** Net Banking (7.97%) and ATM (7.80%) have the highest
transaction failure rates of all channels — meaningfully above Branch
(4.66%), the most reliable channel.

**Recommendation:** Since UPI carries the highest volume, a 1-point
improvement there recovers more transactions in absolute terms — but
Net Banking and ATM have the highest failure *rate*, meaning something
channel-specific (timeout thresholds, downtime, connectivity) is likely
at play. Prioritize:
1. Root-cause the Net Banking failure pattern first (highest rate)
2. Then UPI (highest volume — biggest absolute recovery opportunity)

**Success metric:** Bring overall Failure Rate from 7.04% down to
under 5%, matching Branch-channel reliability as the benchmark.

---

## 3. Protect and grow the Deposits category

**Finding:** Deposits is the *only* product category with a meaningfully
positive net balance (+₹4.84M) — it is effectively carrying the entire
platform's positive Total Balance on its own, while Credit is roughly
break-even and Payments is net-negative.

**Recommendation:** Since the platform's overall financial health
currently depends disproportionately on one category, this is a
concentration risk worth flagging to leadership. Two directions:
- **Defensive:** protect Deposits products (Fixed Deposit, Recurring
  Deposit, Savings) with retention-focused offers, since losing this
  category would flip the platform's total balance negative
- **Offensive:** look for ways to make Investments (currently slightly
  negative at -₹0.38M net) into a second positive-contributing category,
  since it already has healthy transaction volume

**Success metric:** Total Balance no longer swings negative if Deposits
volume dips 10% — i.e., diversify so no single category is load-bearing.

---

## 4. Address the West region's negative balance

**Finding:** West region has the most negative net balance (-₹4.35M)
despite having the second-highest customer count (168) — the opposite
pattern from East, which has fewer customers (162) but the most
positive balance (+₹5.91M).

**Recommendation:** Compare product mix by region — if West customers
are disproportionately using loan/credit products relative to
deposit/investment products, that explains the gap without necessarily
indicating a "problem," but it does mean West-region customers carry
more credit risk exposure. Build a region × product-category matrix
visual to confirm this before acting.

**Success metric:** A clear product-mix explanation for the East/West
gap, turning an unexplained anomaly into an understood (and if needed,
addressed) pattern.

---

## 5. Build a high-value customer retention program

**Finding:** The top 10 customers each transact 26-32 times — roughly
4-5x the platform average of 6.25 transactions/customer — but there's
currently no dedicated view or program tracking this segment.

**Recommendation:** Formalize a "high-engagement" customer segment
(e.g. top 10% by transaction frequency) and track their retention
separately from the general customer base. Since customer growth is
strong (40 → 180+ transactions/month across 2023-2025), protecting the
most active customers within that growth matters more as the base
scales.

**Success metric:** A defined high-value segment with its own churn/
retention rate tracked quarter over quarter.

---

## 6. Add channel-level and category-level views to the dashboard

**Finding:** Several of the insights above (failure rate by channel,
net balance by product category) required querying the raw data
directly — they aren't currently visualized anywhere in the 5-page
dashboard.

**Recommendation:** Add two visuals to strengthen the dashboard itself:
- A **Failure Rate by Channel** bar chart on the Transaction Analysis
  page (currently only shows overall failure rate and channel volume
  separately, not combined)
- A **Net Balance by Product Category** chart on the Product Analysis
  page (currently only breaks down to individual product level, not
  the category rollup)

**Success metric:** Both findings above become self-evident from the
dashboard itself, without needing to query the underlying CSVs.
