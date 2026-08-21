# MoneyTrail Analytics — Fintech Transaction Analytics

A Power BI dashboard analyzing transaction, customer, product, and account
data for a simulated digital payments platform. Built as an end-to-end BI
portfolio project: data modeling, DAX measures, and interactive reporting
across 5 report pages.

![Dashboard Overview](./screenshots/01_overview.png)
*(replace with your own screenshot)*

---

## 1. Project Objective

Digital payment platforms generate huge volumes of transaction data across
channels like UPI, mobile banking, and net banking. This dashboard answers
the core business questions a fintech analytics/BI team would track:

- What is the platform's net financial position (inflows vs outflows) and
  how is it trending?
- Which channels and transaction types drive the most volume, and where
  do failures concentrate?
- Who are the most valuable customers, and how are they distributed
  geographically?
- Which products generate the most inflow, and which carry the most risk
  (outflow-heavy)?
- How healthy is the account base — growth, closures, and balance
  distribution by region?

---

## 2. Tools & Tech Stack

| Tool | Purpose |
|---|---|
| Power BI Desktop | Data modeling, DAX, report design |
| Power Query (M) | Data cleaning and shaping |
| DAX | Custom measures and KPIs |
| Python (pandas, Faker) | Synthetic dataset generation |

---

## 3. Data Model (Star Schema)

```
                DimCalendar
                     |
DimAccount ---- FactTransaction ---- DimProduct ---- DimProductSubCategory ---- DimProductCategory
     |
DimCustomer
```

**Fact table:**
- `FactTransaction` — one row per transaction (5,000 rows)

**Dimension tables:**
- `DimCustomer` — customer demographics and location (800 rows)
- `DimAccount` — account details, linked to customers (1,000 rows)
- `DimProduct` — financial products (16 rows)
- `DimProductSubCategory` / `DimProductCategory` — product hierarchy
- `DimCalendar` — full date table, 2023–2025, marked as the model's Date Table

Full column-level detail is in [`DATA_DICTIONARY.md`](./DATA_DICTIONARY.md).

**Relationship notes:**
- All Dim → Fact relationships are 1-to-many, single direction.
- `DimAccount` has two additional relationships to `DimCalendar` (via
  `OpenDate` and `CloseDate`), both created as **inactive** — only one
  relationship to a date table can be active at a time. These are
  invoked on demand using `USERELATIONSHIP()` inside the
  `Accounts Opened` / `Accounts Closed` measures. See
  [`DAX_MEASURES.md`](./DAX_MEASURES.md).
- `DimProductCategory ↔ DimProductSubCategory` is 1:1 in this dataset, so
  Power BI requires "Both" cross-filter direction — not a modeling error,
  it's the only valid option for 1:1 cardinality.

---

## 4. Report Pages

| Page | Focus |
|---|---|
| **Overview** | Overall financial health — total balance, inflows/outflows trend, average transaction size over time |
| **Transaction Analysis** | Volume, success/failure rate, breakdown by channel and transaction type |
| **Customer Analysis** | Top customers by activity, regional distribution, top customer highlight |
| **Product Analysis** | Product-level inflow/outflow performance, category hierarchy |
| **Account Analysis** | Open vs closed accounts, balance by region, account lifecycle trend |

All pages use button-based page navigation and a synced Year slicer.

---

## 5. Key Design Decisions

- **Success-only filtering for financial measures:** `Total Inflows`,
  `Total Outflows`, and `Total Balance` only count transactions with
  `Status = "Success"` — failed transactions didn't move real money, so
  including them would overstate financial activity. A separate
  `Total Amount (All Status)` measure exists specifically for visuals
  that need to slice by Status itself (see DAX doc for why this was
  necessary).
- **Transaction type weighted by product realism:** deposit/investment
  products skew Credit, loan/credit products skew Debit, rather than a
  random 50/50 split — this makes category-level inflow/outflow patterns
  behave the way they would on a real platform.
- **Transaction dates are clamped to the Calendar table's range**
  (2023–2025) at generation time, so every transaction has a valid match
  in the date dimension.
- **Channel mix reflects real digital-payments usage patterns** — UPI
  weighted as the dominant channel, consistent with real-world digital
  payment adoption trends.

---

## 6. Key Insights

*(Full detail with supporting tables in [`INSIGHTS.md`](./INSIGHTS.md))*

- **Total Balance is +₹2.93M** (Inflows ₹176.44M vs Outflows ₹173.50M) —
  a modest net-positive position, with a 92.96% transaction success rate.
- **UPI accounts for 40.3% of all transaction volume** — more than
  double the next channel (Mobile App, 26.0%) — while Net Banking (7.97%)
  and ATM (7.80%) carry the highest failure rates of any channel.
- **Deposits is the only product category with a positive net balance**
  (+₹4.84M) — it's effectively carrying the platform's entire positive
  Total Balance; Credit is nearly break-even and Payments runs net
  negative.
- **Account closure rate tripled in two years**: 8.5% of opened accounts
  closed in 2023, rising to 30.2% in 2025 — while account-opening growth
  is flattening (+57% in 2024, only +11% in 2025). This is the most
  significant trend in the account data.
- **East (+₹5.91M) and North (+₹3.25M) regions carry the platform's
  positive balance**, while West (-₹4.35M) runs the most negative despite
  having the 2nd-highest customer count.

---

## 7. How to Reproduce

1. Clone this repo
2. Open `Money_Flow_Dashboard.pbix` in Power BI Desktop
3. Data source files are in `/data` — if paths break, go to
   **Transform Data → Data Source Settings** and repoint to the local
   `/data` folder
4. Refresh to load

---

## 8. Project Structure

```
├── data/                          # Source CSVs (star schema)
├── docs/
│   ├── README.md
│   ├── DATA_DICTIONARY.md
│   ├── DAX_MEASURES.md
│   ├── INSIGHTS.md
│   └── RECOMMENDATIONS.md          
├── screenshots/                    # Dashboard page screenshots
└── Dashboard.pbix       # Power BI file
```

---

## 9. Author

Built by Karan Kukadiya as a Power BI / data analytics portfolio project.
[LinkedIn](https://www.linkedin.com/in/karankukadiya/) · [GitHub](https://github.com/Karankukadiya) · [Portfolio site](https://karankukadiya.github.io/Karan-Kukadiya-Portfolio/)
