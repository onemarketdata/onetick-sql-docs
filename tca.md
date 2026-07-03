# Trade Cost Analysis (TCA)

A series of examples are provided showing how to perform Trade Cost Analysis (TCA) by joining proprietary trade and order data with market data benchmarks.
TCA compares executed trade prices against market conditions to measure trading performance and execution quality.

## TCA Datasets

TCA analysis uses proprietary order and trade data stored in secure sample databases:


* `S_ORDERS_LSE_SAMPLE` - Sample LSE order flow dataset (encrypted and secured)


* `S_ORDERS_US_COMP_SAMPLE` - Sample US trades dataset (encrypted and secured)

These datasets are joined with market data benchmarks to calculate performance metrics.

## Query Loaded Order Messages

Proprietary order flow data can be queried to retrieve all order messages for a specific symbol.
This provides complete order lifecycle information.

```sql
select * from S_ORDERS_LSE_SAMPLE.ORDER
where SYMBOL_NAME = 'VOD'
and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
and TIMESTAMP < '2024-04-04 00:00:00 Europe/London'
limit 1000
```

## Query Loaded Trades

Proprietary trade data can be queried to retrieve all trades for a specific symbol.
This shows execution details and trade characteristics.

```sql
select * from S_ORDERS_US_COMP_SAMPLE.TRD
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 America/New_York'
and TIMESTAMP < '2024-04-04 00:00:00 America/New_York'
limit 1000
```

## Prevailing Prices for Supplied Trades

Prevailing market prices (NBBO) at the time of execution can be retrieved by joining trade data with quote data.
This associates each trade with the market conditions that existed when the trade was executed.

The `sametime_as_existing` function performs an as-of join to find the prevailing quote at each trade timestamp.

```sql
select t.ID as ID, t.PRICE as PRICE, t.SIZE as SIZE, t.SIDE as SIDE,
q.TIMESTAMP NBBO_TIME, q.BID_PRICE as BID_PRICE, q.ASK_PRICE as ASK_PRICE
from S_ORDERS_US_COMP_SAMPLE.TRD t, US_COMP_SAMPLE.NBBO q
where t.SYMBOL_NAME='CSCO' and q.SYMBOL_NAME='CSCO'
and sametime_as_existing(t.timestamp, q.timestamp, 0) = TRUE
and TIMESTAMP >= '2024-01-03 00:00:00 America/New_York'
and TIMESTAMP < '2024-01-04 00:00:00 America/New_York'
limit 1000
```

## Effective Spread Calculation

Effective spread measures the performance of executed trades against prevailing market conditions.
It captures the cost of execution by comparing the volume-weighted average price (VWAP) of trade fills against the mid-price at order arrival.

The effective spread is calculated as:

`2 × Direction × Quantity × (VWAP_Price - Mid_Price_at_Arrival)`

Where:


* `Direction` = 1 for buy orders, -1 for sell orders


* `Quantity` = Total filled quantity


* `VWAP_Price` = Volume-weighted average price of fills


* `Mid_Price_at_Arrival` = Mid-price when the order arrived in the market

```sql
select ID, VWAP_PRICE, EXEC_COUNT, EXEC_QTY, MID_AT_ARRIVAL, DIRECTION,
2 * DIRECTION * EXEC_QTY * (VWAP_PRICE - MID_AT_ARRIVAL) as EFFECTIVE_SPREAD
from
(
 select ID,
 VWAP(PRICE_FIELD_NAME=PRICE_FILLED,SIZE_FIELD_NAME=QTY_FILLED) as VWAP_PRICE,
 COUNT(QTY_FILLED) as EXEC_COUNT,
 SUM(QTY_FILLED) as EXEC_QTY,
 FIRST(MID_PRICE) as MID_AT_ARRIVAL,
 FIRST(DIRECTION) as DIRECTION
 from
  (
   select o.ID as ID, o.PRICE as PRICE, o.PRICE_FILLED as PRICE_FILLED,
   o.QTY as QTY, o.QTY_FILLED as QTY_FILLED,
   case
    when o.SIDE ='BUY' then 1
    else -1
   end as DIRECTION,
   q.TIMESTAMP QTE_TIME, (q.BID_PRICE +  q.ASK_PRICE)/2 as MID_PRICE
   from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
   where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
   and sametime_as_existing(o.timestamp, q.timestamp, 0) = TRUE
   and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
   and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
  ) t
 group by ID
) t
where EXEC_QTY > 0
```


    <script type="text/x-thebe-config">
    {
        requestKernel: true,
        binderOptions: {
            repo: "binder-examples/jupyter-stacks-datascience",
            ref: "master",
        },
        codeMirrorConfig: {
            theme: "abcdef",
            mode: "python"
        },
        kernelOptions: {
            name: "python3",
            path: "./."
        },
        predefinedOutput: true
    }
    </script>
    <script>kernelName = 'python3'</script>
