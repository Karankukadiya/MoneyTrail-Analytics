# DAX Measures Reference

All measures live on `FactTransaction` unless noted. Grouped by the page
they were built for.

## Core / Executive Overview

```DAX
Total Transactions = COUNTROWS(FactTransaction)

Total Inflows =
CALCULATE(
    SUM(FactTransaction[Amount]),
    FactTransaction[TransactionType] = "Credit",
    FactTransaction[Status] = "Success"
)

Total Outflows =
CALCULATE(
    SUM(FactTransaction[Amount]),
    FactTransaction[TransactionType] = "Debit",
    FactTransaction[Status] = "Success"
)

Total Balance = [Total Inflows] - [Total Outflows]

Successful Transactions % =
DIVIDE(
    CALCULATE(COUNTROWS(FactTransaction), FactTransaction[Status] = "Success"),
    [Total Transactions], 0
)

Failure Rate = 1 - [Successful Transactions %]

Average Transaction =
AVERAGEX(
    FILTER(FactTransaction, FactTransaction[Status] = "Success"),
    FactTransaction[Amount]
)

Total Amount (All Status) = SUM(FactTransaction[Amount])
```
> `Total Amount (All Status)` exists separately because `Total Balance`
> hard-filters to `Status = "Success"` inside `CALCULATE` — that filter
> overrides any Status filter coming from a visual (e.g. a pie chart
> legend), so slicing `Total Balance` by Status produces identical values
> on every slice. Use this unfiltered measure whenever a visual needs to
> slice by Status itself.

## Time Intelligence

```DAX
Total Inflows LY =
CALCULATE([Total Inflows], SAMEPERIODLASTYEAR(DimCalendar[Date]))

Total Balance YTD =
TOTALYTD([Total Balance], DimCalendar[Date])

Inflows YoY % =
DIVIDE([Total Inflows] - [Total Inflows LY], [Total Inflows LY], 0)
```
> Requires `DimCalendar` to be marked as the model's Date Table
> (Table tools → Mark as Date Table → Date column).

## Transaction Analysis

```DAX
Successful Transactions =
CALCULATE(COUNTROWS(FactTransaction), FactTransaction[Status] = "Success")

Failed Transactions =
CALCULATE(COUNTROWS(FactTransaction), FactTransaction[Status] = "Failed")

Inflows % = DIVIDE([Total Inflows], [Total Inflows] + [Total Outflows], 0)
Outflows % = DIVIDE([Total Outflows], [Total Inflows] + [Total Outflows], 0)
```

## Customer Analysis

```DAX
Total Customers = DISTINCTCOUNT(DimCustomer[CustomerID])

Transactions Last Year =
CALCULATE([Total Transactions], SAMEPERIODLASTYEAR(DimCalendar[Date]))

Average Transaction by Client =
DIVIDE([Total Inflows] + [Total Outflows], [Total Customers], 0)

Top Customer Name =
CALCULATE(
    SELECTEDVALUE(DimCustomer[CustomerName]),
    TOPN(1, ALL(DimCustomer[CustomerName]), [Total Transactions])
)

Top Customer Transactions =
CALCULATE([Total Transactions], TOPN(1, ALL(DimCustomer[CustomerName]), [Total Transactions]))

Top Customer Region =
CALCULATE(
    SELECTEDVALUE(DimCustomer[Region]),
    TOPN(1, ALL(DimCustomer[CustomerName]), [Total Transactions])
)
```

## Product Analysis

```DAX
Number of Products = DISTINCTCOUNT(DimProduct[ProductID])

Top Product Name =
CALCULATE(
    SELECTEDVALUE(DimProduct[ProductName]),
    TOPN(1, ALL(DimProduct[ProductName]), [Total Inflows])
)

Top Product Inflows =
CALCULATE([Total Inflows], TOPN(1, ALL(DimProduct[ProductName]), [Total Inflows]))

Lowest Outflow Product Name =
CALCULATE(
    SELECTEDVALUE(DimProduct[ProductName]),
    TOPN(1, ALL(DimProduct[ProductName]), [Total Outflows], ASC)
)

Lowest Outflow Product Value =
CALCULATE([Total Outflows], TOPN(1, ALL(DimProduct[ProductName]), [Total Outflows], ASC))

Product Rank by Inflows =
RANKX(ALL(DimProduct[ProductName]), [Total Inflows], , DESC)
```
> `Product Rank by Inflows` only produces meaningful values inside a
> table/matrix visual with one row per product — on a standalone Card it
> will always show rank 1.

## Account Analysis

```DAX
Number of Accounts = DISTINCTCOUNT(DimAccount[AccountID])

Open Accounts =
CALCULATE(DISTINCTCOUNT(DimAccount[AccountID]), DimAccount[Status] = "Open")

Closed Accounts =
CALCULATE(DISTINCTCOUNT(DimAccount[AccountID]), DimAccount[Status] = "Closed")

Average Balance per Account = DIVIDE([Total Balance], [Number of Accounts], 0)

Accounts Opened =
CALCULATE(
    DISTINCTCOUNT(DimAccount[AccountID]),
    USERELATIONSHIP(DimAccount[OpenDate], DimCalendar[Date])
)

Accounts Closed =
CALCULATE(
    DISTINCTCOUNT(DimAccount[AccountID]),
    DimAccount[Status] = "Closed",
    USERELATIONSHIP(DimAccount[CloseDate], DimCalendar[Date])
)
```
> `Accounts Opened`/`Accounts Closed` use `USERELATIONSHIP` because
> `DimAccount` has two date columns (`OpenDate`, `CloseDate`) but only one
> relationship to `DimCalendar` can be active at a time. Both alternate
> relationships are created as **inactive** in the model, then activated
> per-measure on demand.
