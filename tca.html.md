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

## Arrival Ask / Bid / Mid Prices for an Order

The arrival price is the prevailing quote captured at the moment an order arrives. Arrival is identified by `STATE = 'N'` (New) in the order lifecycle dataset. The prevailing LSE quote is associated with each arrival order through an as-of join (`sametime_as_existing`), and the mid is the average of the bid and ask.

```sql
select ID,
ARRIVAL_ASK_PRICE,
ARRIVAL_BID_PRICE,
(ARRIVAL_ASK_PRICE + ARRIVAL_BID_PRICE)/2 as ARRIVAL_MID_PRICE
from
(
 select o.ID as ID, q.ASK_PRICE as ARRIVAL_ASK_PRICE, q.BID_PRICE as ARRIVAL_BID_PRICE
 from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
 where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
 and o.STATE='N'
 and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
 and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
 and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
) t
limit 1000
```

## Effective to Quoted Spread (ETQ)

ETQ is an easier-to-interpret Effective Spread that normalises for the usual spread differences between instruments. It is meaningful only for aggressive orders that execute immediately.

`ETQ = (VWAP - Mid_Price) × 2 × Direction / (Ask_Price - Bid_Price)`

The prevailing LSE quote is joined to each order as-of (`sametime_as_existing`). Direction is `+1` for a buy and `-1` for a sell, and VWAP is the fill VWAP across the order.

```sql
select ID,
(VWAP_PRICE - MID_PRICE) * 2 * DIRECTION / (ASK_PRICE - BID_PRICE) as ETQ
from
(
 select ID,
 VWAP(PRICE_FIELD_NAME=PRICE_FILLED, SIZE_FIELD_NAME=QTY_FILLED) as VWAP_PRICE,
 FIRST(ASK_PRICE) as ASK_PRICE,
 FIRST(BID_PRICE) as BID_PRICE,
 FIRST(MID_PRICE) as MID_PRICE,
 FIRST(DIRECTION) as DIRECTION
 from
 (
  select o.ID as ID, o.PRICE_FILLED as PRICE_FILLED, o.QTY_FILLED as QTY_FILLED,
  q.ASK_PRICE as ASK_PRICE, q.BID_PRICE as BID_PRICE, (q.ASK_PRICE + q.BID_PRICE)/2 as MID_PRICE,
  case when o.SIDE='BUY' then 1 else -1 end as DIRECTION
  from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
  where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
  and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
  and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
  and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
 ) t
 group by ID
) t
where ASK_PRICE - BID_PRICE <> 0
limit 1000
```

## Implementation Shortfall (Slippage)

Implementation Shortfall measures execution quality relative to the mid prevailing when the order arrived. Large positive values imply a good execution; large negative values a poor one.

`IS = Direction × Executed_QTY × (Arrival_Mid_Price - VWAP)`

The prevailing LSE quote is joined to each order as-of (`sametime_as_existing`); the arrival mid is the first mid seen for the order. Direction is `+1` for a buy and `-1` for a sell.

```sql
select ID,
DIRECTION * EXEC_QTY * (ARRIVAL_MID_PRICE - VWAP_PRICE) as IS
from
(
 select ID,
 FIRST(MID_PRICE) as ARRIVAL_MID_PRICE,
 VWAP(PRICE_FIELD_NAME=PRICE_FILLED, SIZE_FIELD_NAME=QTY_FILLED) as VWAP_PRICE,
 SUM(QTY_FILLED) as EXEC_QTY,
 FIRST(DIRECTION) as DIRECTION
 from
 (
  select o.ID as ID, o.PRICE_FILLED as PRICE_FILLED, o.QTY_FILLED as QTY_FILLED,
  (q.ASK_PRICE + q.BID_PRICE)/2 as MID_PRICE,
  case when o.SIDE='BUY' then 1 else -1 end as DIRECTION
  from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
  where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
  and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
  and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
  and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
 ) t
 group by ID
) t
where EXEC_QTY > 0
limit 1000
```

## Notional Spread (Quoted Spread)

The notional spread values the quoted spread over the executed size of an order.

`Notional_Spread = Executed_QTY × (Arrival_Ask_Price - Arrival_Bid_Price)`

The arrival ask/bid are the prevailing LSE quote at order arrival, joined as-of to the order (`sametime_as_existing`). Executed_QTY is the total filled quantity of the order.

```sql
select ID,
EXEC_QTY * (ARRIVAL_ASK_PRICE - ARRIVAL_BID_PRICE) as NOTIONAL_SPREAD
from
(
 select ID,
 SUM(QTY_FILLED) as EXEC_QTY,
 FIRST(ASK_PRICE) as ARRIVAL_ASK_PRICE,
 FIRST(BID_PRICE) as ARRIVAL_BID_PRICE
 from
 (
  select o.ID as ID, o.QTY_FILLED as QTY_FILLED,
  q.ASK_PRICE as ASK_PRICE, q.BID_PRICE as BID_PRICE
  from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
  where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
  and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
  and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
  and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
 ) t
 group by ID
) t
limit 1000
```

<a id="far-touch-and-near-touch"></a>

## Far Touch and Near Touch

Near Touch and Far Touch are the arrival quote prices on the passive / aggressive side, determined by the order side:

* `BUY`  : Near Touch = Arrival Bid Price, Far Touch = Arrival Ask Price
* `SELL` : Near Touch = Arrival Ask Price, Far Touch = Arrival Bid Price

The arrival ask/bid are the prevailing LSE quote at order arrival (`STATE = 'N'`), joined as-of to the order (`sametime_as_existing`).

```sql
select ID,
case when SIDE='BUY' then ARRIVAL_BID_PRICE else ARRIVAL_ASK_PRICE end as NT,
case when SIDE='BUY' then ARRIVAL_ASK_PRICE else ARRIVAL_BID_PRICE end as FT
from
(
 select o.ID as ID, o.SIDE as SIDE,
 q.ASK_PRICE as ARRIVAL_ASK_PRICE, q.BID_PRICE as ARRIVAL_BID_PRICE
 from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
 where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
 and o.STATE='N'
 and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
 and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
 and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
) t
limit 1000
```

## Number Spreads

Number Spreads measures how far an order executes from the touch, expressed in units of the arrival spread (Far Touch minus Near Touch).

`Num_Spreads = abs((VWAP - FT) / (FT - NT))`

Near Touch (NT) and Far Touch (FT) are the arrival quote prices by side (see [Far Touch and Near Touch](#far-touch-and-near-touch)); VWAP is the order fill VWAP. The prevailing LSE quote at arrival is joined as-of to the order. Rows where the arrival spread is zero are excluded to avoid division by zero.

```sql
select ID,
abs((VWAP_PRICE - FT) / (FT - NT)) as NUM_SPREADS
from
(
 select ID, VWAP_PRICE,
 case when SIDE='BUY' then ARRIVAL_BID_PRICE else ARRIVAL_ASK_PRICE end as NT,
 case when SIDE='BUY' then ARRIVAL_ASK_PRICE else ARRIVAL_BID_PRICE end as FT
 from
 (
  select ID,
  VWAP(PRICE_FIELD_NAME=PRICE_FILLED, SIZE_FIELD_NAME=QTY_FILLED) as VWAP_PRICE,
  FIRST(ASK_PRICE) as ARRIVAL_ASK_PRICE,
  FIRST(BID_PRICE) as ARRIVAL_BID_PRICE,
  FIRST(SIDE) as SIDE
  from
  (
   select o.ID as ID, o.SIDE as SIDE, o.PRICE_FILLED as PRICE_FILLED, o.QTY_FILLED as QTY_FILLED,
   q.ASK_PRICE as ASK_PRICE, q.BID_PRICE as BID_PRICE
   from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
   where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
   and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
   and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
   and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
  ) t
  group by ID
 ) t
) t
where FT - NT <> 0
limit 1000
```

## Offside Value

Offside value measures the impact of an aggressive order: the distance in notional units that the impacted side moved from the far touch prevailing when the order arrived.

`Offside_Value = Executed_QTY × (VWAP - FT) × Direction`

Far Touch (FT) is the arrival far touch by side (`BUY` -> arrival ask, `SELL` -> arrival bid); Direction is `+1` for a buy and `-1` for a sell. The prevailing LSE quote at arrival is joined as-of to the order.

```sql
select ID,
EXEC_QTY * (VWAP_PRICE - FT) * DIRECTION as OFFSIDE_VALUE
from
(
 select ID, VWAP_PRICE, EXEC_QTY, DIRECTION,
 case when SIDE='BUY' then ARRIVAL_ASK_PRICE else ARRIVAL_BID_PRICE end as FT
 from
 (
  select ID,
  VWAP(PRICE_FIELD_NAME=PRICE_FILLED, SIZE_FIELD_NAME=QTY_FILLED) as VWAP_PRICE,
  SUM(QTY_FILLED) as EXEC_QTY,
  FIRST(ASK_PRICE) as ARRIVAL_ASK_PRICE,
  FIRST(BID_PRICE) as ARRIVAL_BID_PRICE,
  FIRST(SIDE) as SIDE,
  FIRST(DIRECTION) as DIRECTION
  from
  (
   select o.ID as ID, o.SIDE as SIDE, o.PRICE_FILLED as PRICE_FILLED, o.QTY_FILLED as QTY_FILLED,
   q.ASK_PRICE as ASK_PRICE, q.BID_PRICE as BID_PRICE,
   case when o.SIDE='BUY' then 1 else -1 end as DIRECTION
   from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
   where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
   and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
   and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
   and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
  ) t
  group by ID
 ) t
) t
where EXEC_QTY > 0
limit 1000
```

## Price Improvement

Price improvement measures the distance from the far touch to the execution VWAP in basis points. Positive values indicate a good execution, negative a poor one.

`PriceImprovement_bps = Direction × 10000 × (FT - VWAP) / FT`

Far Touch (FT) is the arrival far touch by side (`BUY` -> arrival ask, `SELL` -> arrival bid); Direction is `+1` for a buy and `-1` for a sell. The prevailing LSE quote at arrival is joined as-of to the order.

```sql
select ID,
DIRECTION * 10000 * (FT - VWAP_PRICE) / FT as PriceImprovements_bps
from
(
 select ID, VWAP_PRICE, DIRECTION,
 case when SIDE='BUY' then ARRIVAL_ASK_PRICE else ARRIVAL_BID_PRICE end as FT
 from
 (
  select ID,
  VWAP(PRICE_FIELD_NAME=PRICE_FILLED, SIZE_FIELD_NAME=QTY_FILLED) as VWAP_PRICE,
  FIRST(ASK_PRICE) as ARRIVAL_ASK_PRICE,
  FIRST(BID_PRICE) as ARRIVAL_BID_PRICE,
  FIRST(SIDE) as SIDE,
  FIRST(DIRECTION) as DIRECTION
  from
  (
   select o.ID as ID, o.SIDE as SIDE, o.PRICE_FILLED as PRICE_FILLED, o.QTY_FILLED as QTY_FILLED,
   q.ASK_PRICE as ASK_PRICE, q.BID_PRICE as BID_PRICE,
   case when o.SIDE='BUY' then 1 else -1 end as DIRECTION
   from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
   where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
   and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
   and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
   and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
  ) t
  group by ID
 ) t
) t
where FT <> 0
limit 1000
```

## Opportunity Cost

Opportunity cost values the unexecuted quantity against the price move over the life of the order (arrival mid versus exit mid).

`OC = Direction × Unexecuted_QTY × (Arrival_Mid_Price - Exit_Mid_Price)`

Every order message is joined as-of to the prevailing LSE mid; the first mid seen for the order is the arrival mid and the last is the exit mid. Unexecuted quantity is the summed difference between ordered and filled quantity. Direction is `+1` for a buy and `-1` for a sell.

```sql
select ID,
DIRECTION * UNEXEC_QTY * (ARRIVAL_MID - EXIT_MID) as OC
from
(
 select ID,
 FIRST(MID) as ARRIVAL_MID,
 LAST(MID) as EXIT_MID,
 SUM(QTY - QTY_FILLED) as UNEXEC_QTY,
 FIRST(DIRECTION) as DIRECTION
 from
 (
  select o.ID as ID, o.QTY as QTY, o.QTY_FILLED as QTY_FILLED,
  (q.ASK_PRICE + q.BID_PRICE)/2 as MID,
  case when o.SIDE='BUY' then 1 else -1 end as DIRECTION
  from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
  where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
  and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
  and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
  and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
 ) t
 group by ID
) t
limit 1000
```

## Takeout Success

Takeout success flags whether an order captured the available size in the market on the touched side at arrival.

* `BUY`  : Takeout Success when `QTY_FILLED >= Ask_Size`
* `SELL` : Takeout Success when `QTY_FILLED >= Bid_Size`

The prevailing LSE quote at order arrival (`STATE = 'N'`) is joined as-of to the order, supplying the `ASK_SIZE` / `BID_SIZE` available at that instant. Direction is `+1` for a buy and `-1` for a sell.

```sql
select ID,
case
 when DIRECTION = 1  and QTY_FILLED >= ASK_SIZE then 1
 when DIRECTION = -1 and QTY_FILLED >= BID_SIZE then 1
 else 0
end as TAKEOUT_SUCCESS
from
(
 select o.ID as ID, o.QTY_FILLED as QTY_FILLED,
 q.ASK_SIZE as ASK_SIZE, q.BID_SIZE as BID_SIZE,
 case when o.SIDE='BUY' then 1 else -1 end as DIRECTION
 from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
 where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
 and o.STATE='N'
 and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
 and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
 and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
) t
limit 1000
```

## Volatility

A simple realised volatility proxy for an instrument over the interval, computed as the standard deviation of trade prices divided by their average, in percent.

`Volatility = STDDEV(PRICE) / AVERAGE(PRICE) × 100`

Unlike the order-level metrics above, this benchmark is computed directly on the market trade stream (`LSE_SAMPLE.TRD`) over a fixed interval.

```sql
select
STDDEV(PRICE) as STDDEV_PRICE,
AVG(PRICE) as AVERAGE_PRICE,
STDDEV(PRICE) / AVG(PRICE) * 100 as VOLATILITY
from LSE_SAMPLE.TRD
where SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
and TIMESTAMP < '2024-01-03 16:30:00 Europe/London'
```

## Exit Ask / Bid Prices for an Order

The exit price is the prevailing quote captured when an order leaves the book, i.e. when it is fully executed (`STATE = 'F'`) or cancelled (`STATE = 'C'`). The prevailing LSE quote is joined as-of to each exit message (`sametime_as_existing`); if an order has more than one exit message the first exit quote is taken per order ID.

```sql
select ID,
FIRST(ASK_PRICE) as EXIT_ASK_PRICE,
FIRST(BID_PRICE) as EXIT_BID_PRICE
from
(
 select o.ID as ID, q.ASK_PRICE as ASK_PRICE, q.BID_PRICE as BID_PRICE
 from S_ORDERS_LSE_SAMPLE.ORDER o, LSE_SAMPLE.QTE q
 where o.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
 and (o.STATE='F' or o.STATE='C')
 and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
 and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
 and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
) t
group by ID
limit 1000
```

## Interval Market VWAP

The market VWAP is the volume-weighted average trade price over an interval, a common benchmark for order execution. This example computes a single market VWAP over a fixed interval; to benchmark a specific order, restrict the interval to that order’s arrival / exit times.

`Market_VWAP = VWAP(PRICE, SIZE)`

```sql
select
VWAP(PRICE_FIELD_NAME=PRICE, SIZE_FIELD_NAME=SIZE) as MARKET_VWAP,
SUM(SIZE) as VOLUME,
COUNT(*) as TRADE_COUNT
from LSE_SAMPLE.TRD
where SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 09:30:00 Europe/London'
and TIMESTAMP < '2024-01-03 09:31:00 Europe/London'
```

## Right Way (Order Direction vs. Subsequent Mid Move)

The Right Way metric flags whether each order was placed in the “right way” - i.e. whether the market mid moved in the order’s favour on the next quote update:

* `BUY`  is Right Way (1) if the next mid price is **higher** than the mid at arrival.
* `SELL` is Right Way (1) if the next mid price is **lower**  than the mid at arrival.

The next mid is computed on the quote stream itself: `LEAD(ASK_PRICE)` and `LEAD(BID_PRICE)` in the subquery look one quote update ahead, ordered by `TIMESTAMP`, so that the “arrival” mid and the “next” mid are both available on a single quote row before the order is joined. Each order is then joined as-of to its prevailing NBBO row (`sametime_as_existing`), and the `CASE` expression compares the two mids by side.

```sql
select o.ID as ID, o.SIDE as SIDE,
q.MID_PRICE as MID_PRICE,
q.NEXT_MID_PRICE as NEXT_MID_PRICE,
case
  when o.SIDE = 'BUY'  and q.NEXT_MID_PRICE > q.MID_PRICE then 1
  when o.SIDE = 'SELL' and q.NEXT_MID_PRICE < q.MID_PRICE then 1
  else 0
end as RIGHT_WAY
from S_ORDERS_US_COMP_SAMPLE.TRD o,
(
  select TIMESTAMP,
  (ASK_PRICE + BID_PRICE)/2 as MID_PRICE,
  (LEAD(ASK_PRICE) OVER(ORDER BY TIMESTAMP) + LEAD(BID_PRICE) OVER(ORDER BY TIMESTAMP))/2 as NEXT_MID_PRICE
  from US_COMP_SAMPLE.NBBO
  where SYMBOL_NAME = 'CSCO'
  and TIMESTAMP >= '2024-01-03 00:00:00 America/New_York'
  and TIMESTAMP < '2024-01-04 00:00:00 America/New_York'
) q
where o.SYMBOL_NAME = 'CSCO'
and sametime_as_existing(o.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
and TIMESTAMP >= '2024-01-03 00:00:00 America/New_York'
and TIMESTAMP < '2024-01-04 00:00:00 America/New_York'
limit 1000
```
