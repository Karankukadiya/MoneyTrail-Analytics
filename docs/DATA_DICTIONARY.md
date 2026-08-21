# Data Dictionary

## FactTransaction (Fact table — ~5,000 rows)

| Column | Type | Description |
|---|---|---|
| TransactionID | Text | Unique transaction identifier (TXN000001...) |
| Date | Date | Date the transaction occurred |
| AccountID | Text | Foreign key → DimAccount |
| CustomerID | Text | Foreign key → DimCustomer |
| ProductID | Text | Foreign key → DimProduct |
| TransactionType | Text | `Credit` (money in) or `Debit` (money out) |
| TransactionChannel | Text | UPI, Mobile App, Net Banking, ATM, Branch, POS |
| Amount | Decimal | Transaction value in INR |
| Status | Text | `Success` or `Failed` |

## DimCustomer (800 rows)

| Column | Type | Description |
|---|---|---|
| CustomerID | Text | Primary key |
| CustomerName | Text | Customer full name |
| Gender | Text | Male / Female |
| Age | Integer | Customer age |
| City | Text | Indian city |
| State | Text | Indian state |
| Region | Text | North / South / East / West / Central |
| JoinDate | Date | Date customer joined the platform |
| Segment | Text | Retail / Premium / Corporate |

## DimAccount (1,000 rows)

| Column | Type | Description |
|---|---|---|
| AccountID | Text | Primary key |
| CustomerID | Text | Foreign key → DimCustomer |
| AccountType | Text | Savings / Current / Salary / NRI |
| Branch | Text | Branch name (by city) |
| Status | Text | Open / Closed |
| OpenDate | Date | Account opening date |
| CloseDate | Date | Account closure date (blank if still open) |

## DimProduct (16 rows)

| Column | Type | Description |
|---|---|---|
| ProductID | Text | Primary key |
| ProductName | Text | e.g. Savings Account, UPI Wallet, Personal Loan |
| SubCategoryID | Text | Foreign key → DimProductSubCategory |

## DimProductSubCategory (5 rows) / DimProductCategory (5 rows)

Product hierarchy grouping (Deposits, Payments, Credit, Investments,
Insurance) used for drill-down analysis.

## DimCalendar (1,096 rows — daily, 2023–2025)

| Column | Type | Description |
|---|---|---|
| DateID | Integer | YYYYMMDD surrogate key |
| Date | Date | Calendar date (marked as the model's Date Table) |
| Year / Month / MonthName / Quarter / Day / Weekday | — | Standard calendar attributes |
| IsWeekend | Boolean | True for Sat/Sun |

**Note:** `MonthName` should be sorted by the numeric `Month` column
(Column tools → Sort by Column) so it displays Jan→Dec instead of
alphabetically.
