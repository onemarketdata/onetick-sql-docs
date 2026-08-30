# Crypto

This section contains examples of retrieving and analyzing cryptocurrency market data from a crypto venue using OneTick SQL.

Crypto venues such as `BINANCE` provide trades, quotes, and order book updates.
Unlike Equities and Futures, the trade and quote sizes on a crypto venue are fractional, so order book functions require the `SIZE_MAX_FRACTIONAL_DIGITS` attribute to reconstruct the book with sizes stored to the required number of fractional digits.

## Crypto Trade Retrieval

Retrieve trades from a crypto venue. The trade size is fractional, unlike Equities and Futures.

```sql
select * from BINANCE.TRD
where SYMBOL_NAME = 'BTCUSD'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

#### Trade Retrieval for BTCUSD from BINANCE Database

| Timestamp                  |   PRICE |    SIZE | SYMBOL_NAME   | TICK_TYPE   |   OMDSEQ | EXCH_TIME                  |   TRADE_ID | TRADE_TYPE   | TRADE_VENUE   | DELETED_TIME   |   TICK_STATUS | AGGRESSOR_SIDE   | TRADE_PERIOD   |   BOOK_TYPE | BUYER   | SELLER   |
|----------------------------|---------|---------|---------------|-------------|----------|----------------------------|------------|--------------|---------------|----------------|---------------|------------------|----------------|-------------|---------|----------|
| 2026-07-28 00:00:00.145371 | 63687   | 8e-05   | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:00.144160 |     606647 |              | BINANCE       |                |             0 | B                |                |           0 |         |          |
| 2026-07-28 00:00:04.350446 | 63687   | 0.0013  | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:04.349603 |     606648 |              | BINANCE       |                |             0 | S                |                |           0 |         |          |
| 2026-07-28 00:00:04.383817 | 63687   | 0.00158 | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:04.382689 |     606649 |              | BINANCE       |                |             0 | S                |                |           0 |         |          |
| 2026-07-28 00:00:04.517794 | 63687   | 8e-05   | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:04.516315 |     606650 |              | BINANCE       |                |             0 | B                |                |           0 |         |          |
| 2026-07-28 00:00:05.096039 | 63687   | 8e-05   | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:05.094642 |     606651 |              | BINANCE       |                |             0 | B                |                |           0 |         |          |
| 2026-07-28 00:00:11.796468 | 63686.2 | 0.00158 | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:11.795289 |     606652 |              | BINANCE       |                |             0 | S                |                |           0 |         |          |
| 2026-07-28 00:00:11.919521 | 63686.2 | 0.00158 | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:11.918115 |     606653 |              | BINANCE       |                |             0 | S                |                |           0 |         |          |
| 2026-07-28 00:00:12.286485 | 63686.2 | 0.00158 | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:12.285164 |     606654 |              | BINANCE       |                |             0 | S                |                |           0 |         |          |
| 2026-07-28 00:00:12.398781 | 63686.2 | 0.00158 | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:12.397642 |     606655 |              | BINANCE       |                |             0 | S                |                |           0 |         |          |
| 2026-07-28 00:00:12.486492 | 63686.2 | 0.00158 | BTCUSD        | TRD         |        0 | 2026-07-28 00:00:12.485053 |     606656 |              | BINANCE       |                |             0 | S                |                |           0 |         |          |

## Crypto Quote Retrieval

Retrieve quotes from a crypto venue. The bid and ask sizes are fractional, unlike Equities and Futures.

```sql
select * from BINANCE.QTE
where SYMBOL_NAME = 'BTCUSD'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

#### Quote Retrieval for BTCUSD from BINANCE Database

| Timestamp                  |   BID_PRICE |   ASK_PRICE |   BID_SIZE |   ASK_SIZE | SYMBOL_NAME   | TICK_TYPE   |   OMDSEQ | EXCH_TIME   | QUOTE_VENUE   |
|----------------------------|-------------|-------------|------------|------------|---------------|-------------|----------|-------------|---------------|
| 2026-07-28 00:00:00.005578 |     63687   |     63714.4 |    0.03223 |    0.00319 | BTCUSD        | QTE         |       18 |             | BINANCE       |
| 2026-07-28 00:00:00.011008 |     63687   |     63687   |    0.03223 |    8e-05   | BTCUSD        | QTE         |        0 |             | BINANCE       |
| 2026-07-28 00:00:00.141446 |     63681.9 |     63687   |    0.00099 |    8e-05   | BTCUSD        | QTE         |        1 |             | BINANCE       |
| 2026-07-28 00:00:00.144103 |     63682.2 |     63687   |    0.03228 |    8e-05   | BTCUSD        | QTE         |        1 |             | BINANCE       |
| 2026-07-28 00:00:00.145341 |     63682.2 |     63714.4 |    0.03228 |    0.00319 | BTCUSD        | QTE         |        0 |             | BINANCE       |
| 2026-07-28 00:00:03.121064 |     63682.5 |     63714.4 |    0.00785 |    0.00319 | BTCUSD        | QTE         |        1 |             | BINANCE       |
| 2026-07-28 00:00:03.122497 |     63683.7 |     63714.4 |    0.03228 |    0.00319 | BTCUSD        | QTE         |        1 |             | BINANCE       |
| 2026-07-28 00:00:03.210120 |     63684.6 |     63714.4 |    0.04722 |    0.00319 | BTCUSD        | QTE         |        8 |             | BINANCE       |
| 2026-07-28 00:00:03.212483 |     63685.8 |     63714.4 |    0.03228 |    0.00319 | BTCUSD        | QTE         |        5 |             | BINANCE       |
| 2026-07-28 00:00:03.307042 |     63685.8 |     63714.4 |    0.03228 |    0.00162 | BTCUSD        | QTE         |        0 |             | BINANCE       |

## Crypto Book Update Retrieval

Retrieve book updates from a crypto venue. Book updates are provided as an L2 dataset in the `PRL` table, providing updates to price levels.
Basic retrieval is useful for counting order book changes. To reconstruct the order book, the `OB_SNAPSHOT` functions should be used.

```sql
select * from BINANCE.PRL
where SYMBOL_NAME = 'BTCUSD'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

#### Book Update Retrieval for BTCUSD from BINANCE Database

| Timestamp           |   PRICE |    SIZE | SYMBOL_NAME   | TICK_TYPE   |   OMDSEQ | EXCH_TIME   | DELETED_TIME   |   TICK_STATUS |   BUY_SELL_FLAG | RECORD_TYPE   |
|---------------------|---------|---------|---------------|-------------|----------|-------------|----------------|---------------|-----------------|---------------|
| 2026-07-28 00:00:00 |     0   | 0       | BTCUSD        | PRL         |    91088 |             |                |             0 |               0 | Z             |
| 2026-07-28 00:00:00 | 63687   | 0.03223 | BTCUSD        | PRL         |    91089 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63681.9 | 0.00099 | BTCUSD        | PRL         |    91090 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63680.5 | 0.04853 | BTCUSD        | PRL         |    91091 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63676.8 | 0.00022 | BTCUSD        | PRL         |    91092 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63672   | 8e-05   | BTCUSD        | PRL         |    91093 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63662.9 | 0.08928 | BTCUSD        | PRL         |    91094 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63661.8 | 0.03199 | BTCUSD        | PRL         |    91095 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63658.9 | 0.00157 | BTCUSD        | PRL         |    91096 |             |                |             0 |               0 | C             |
| 2026-07-28 00:00:00 | 63658   | 8e-05   | BTCUSD        | PRL         |    91097 |             |                |             0 |               0 | C             |

## Crypto Book Snapshot Retrieval

Reconstruct the order book at a specified time using the `OB_SNAPSHOT_WIDE` function, which returns the book with the bid and ask on the same row. As this is a crypto book, the `SIZE_MAX_FRACTIONAL_DIGITS` attribute is set, allowing the book to be reconstructed with size stored to up to 9 fractional digits.

```sql
SELECT BID_PRICE, BID_SIZE, ASK_PRICE, ASK_SIZE, LEVEL
FROM OTQ_CHAIN."OB_SNAPSHOT_WIDE(SIZE_MAX_FRACTIONAL_DIGITS=9);PRL"
where SYMBOL_NAME = 'BINANCE::BTCUSD'
and TIMESTAMP = '2026-07-28 12:00:00.000 GMT'
```

## Crypto Book Snapshot Retrieval With Accumulative Values

Reconstruct the order book at a specified time, outputting accumulative depth. Bid and ask value is calculated using `PRICE * SIZE`. The bid and ask sizes are used to calculate accumulative sizes across the book depth, computed using the `SUM([Field]) OVER(order by TIMESTAMP asc)` syntax.

```sql
SELECT BID_PRICE, BID_SIZE, ASK_PRICE, ASK_SIZE, LEVEL,
BID_PRICE * BID_SIZE as BID_VALUE,
ASK_PRICE * ASK_SIZE as ASK_VALUE,
SUM(BID_SIZE) OVER(order by TIMESTAMP asc) as ACCUM_BID_SIZE,
SUM(ASK_SIZE) OVER(order by TIMESTAMP asc) as ACCUM_ASK_SIZE
FROM OTQ_CHAIN."OB_SNAPSHOT_WIDE(SIZE_MAX_FRACTIONAL_DIGITS=9);PRL"
where SYMBOL_NAME = 'BINANCE::BTCUSD'
and TIMESTAMP = '2026-07-28 12:00:00.000 GMT'
```

## Crypto Book Snapshot Retrieval With Accumulative Values and Best Prices

Reconstruct the order book at a specified time, outputting accumulative depth and best prices. Bid and ask value is calculated using `PRICE * SIZE`. Accumulative sizes across the book depth are computed with `SUM([Field]) OVER(order by TIMESTAMP asc)`, and the best prices across the book depth are computed with `FIRST([Field]) OVER(order by TIMESTAMP asc)`.

```sql
SELECT BID_PRICE, BID_SIZE, ASK_PRICE, ASK_SIZE, LEVEL,
BID_PRICE * BID_SIZE as BID_VALUE,
ASK_PRICE * ASK_SIZE as ASK_VALUE,
SUM(BID_SIZE) OVER(order by TIMESTAMP asc) as ACCUM_BID_SIZE,
SUM(ASK_SIZE) OVER(order by TIMESTAMP asc) as ACCUM_ASK_SIZE,
FIRST(BID_PRICE) OVER(order by TIMESTAMP asc) as BEST_BID_PRICE,
FIRST(ASK_PRICE) OVER(order by TIMESTAMP asc) as BEST_ASK_PRICE
FROM OTQ_CHAIN."OB_SNAPSHOT_WIDE(SIZE_MAX_FRACTIONAL_DIGITS=9);PRL"
where SYMBOL_NAME = 'BINANCE::BTCUSD'
and TIMESTAMP = '2026-07-28 12:00:00.000 GMT'
```

## Crypto Book Depth Statistics to Trade a Specified Amount Across Time

Calculate bid and ask VWAP and other statistics across time using the `OB_SUMMARY` function. As this is a crypto book, the `SIZE_MAX_FRACTIONAL_DIGITS` attribute is set. The `MAX_DEPTH_SHARES` attribute determines how much should be traded, and the `BUCKET_INTERVAL` attribute determines how often to output the resulting book metrics.

The returned `BID_VWAP` and `ASK_VWAP` can be used to calculate Effective Spread. The returned `BID_SIZE` and `ASK_SIZE` identify whether the liquidity is present. The returned `BEST_ASK_PRICE` and `BEST_BID_PRICE` can be used to calculate the Price Skew together with the `BID_VWAP` and `ASK_VWAP`.

```sql
select * from
OTQ_CHAIN."OB_SUMMARY(SIZE_MAX_FRACTIONAL_DIGITS=9,BUCKET_INTERVAL=60,MAX_DEPTH_SHARES=0.5);PRL"
where symbol_name = 'BINANCE::BTCUSD'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
```

## Coin Analytics

The following examples build the L1 (top-of-book) analytics commonly seen on public crypto analytics dashboards, using only the trade (`TRD`) and quote (`QTE`) data available from a crypto venue. Trades are aggregated into candlestick bars and flow metrics, and quotes into spread and mid-price statistics. Trade and quote sizes are fractional on a crypto venue, so volume figures are reported to fractional precision.

These examples use the `BINANCE` spot symbol `BTCUSDT`. The same queries apply to any crypto coin by changing the `SYMBOL_NAME`, and to other venues by changing the database name.

### OHLCV and VWAP Bars

Aggregate trades into 1-minute candlestick bars, the core price chart of a crypto analytics dashboard. Open, High, Low, and Close come from `FIRST`, `MAX`, `MIN`, and `LAST` of `PRICE`. Volume is `SUM(SIZE)`, and `VWAP` returns the volume-weighted average price across each bucket. `COUNT(*)` returns the number of trades printed in the bucket.

```sql
select
FIRST(PRICE) as OPEN_PRICE,
MAX(PRICE) as HIGH_PRICE,
MIN(PRICE) as LOW_PRICE,
LAST(PRICE) as CLOSE_PRICE,
SUM(SIZE) as VOLUME,
VWAP(PRICE_FIELD_NAME=PRICE, SIZE_FIELD_NAME=SIZE) as VWAP_PRICE,
COUNT(*) as TRADE_COUNT
from BINANCE.TRD
where SYMBOL_NAME = 'BTCUSDT'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
group by time_bucket(INTERVAL '1' MINUTE)
limit 1000
```

### Buy and Sell Taker Flow

Crypto trades carry the `AGGRESSOR_SIDE` field, identifying which side of the book the aggressing (taker) order hit. Grouping by `AGGRESSOR_SIDE` within each 1-minute bucket returns the traded volume, notional value (`PRICE * SIZE`), and trade count for the buy side and the sell side separately, the basis of the buy-vs-sell pressure chart seen on crypto analytics dashboards.

```sql
select
AGGRESSOR_SIDE,
SUM(SIZE) as VOLUME,
SUM(PRICE * SIZE) as NOTIONAL,
COUNT(*) as TRADE_COUNT
from BINANCE.TRD
where SYMBOL_NAME = 'BTCUSDT'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
group by time_bucket(INTERVAL '1' MINUTE), AGGRESSOR_SIDE
limit 1000
```

### Top of Book Spread and Mid Price

Use the L1 top-of-book quotes in the `QTE` table to derive the classic quote analytics: the mid price, the absolute bid/ask spread, and the spread expressed in basis points of the mid. `SPREAD_BPS` normalises the spread to `10000 * SPREAD / MID_PRICE` so tight and wide markets can be compared consistently.

```sql
select TIMESTAMP, BID_PRICE, ASK_PRICE, BID_SIZE, ASK_SIZE,
(BID_PRICE + ASK_PRICE) / 2 as MID_PRICE,
ASK_PRICE - BID_PRICE as SPREAD,
10000 * (ASK_PRICE - BID_PRICE) / ((BID_PRICE + ASK_PRICE) / 2) as SPREAD_BPS
from BINANCE.QTE
where SYMBOL_NAME = 'BTCUSDT'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

### Average Spread Bars

Summarise the L1 quote spread across 1-minute buckets, giving the average and widest spread charts seen on quote analytics dashboards. `AVG`, `MAX`, and `MIN` are applied to the per-quote spread and spread-in-bps, `AVG(MID_PRICE)` tracks the prevailing mid, and `COUNT(*)` returns the number of quote updates as a proxy for how actively the top of book is being revised.

```sql
select
AVG((BID_PRICE + ASK_PRICE) / 2) as AVG_MID_PRICE,
AVG(ASK_PRICE - BID_PRICE) as AVG_SPREAD,
MAX(ASK_PRICE - BID_PRICE) as MAX_SPREAD,
MIN(ASK_PRICE - BID_PRICE) as MIN_SPREAD,
AVG(10000 * (ASK_PRICE - BID_PRICE) / ((BID_PRICE + ASK_PRICE) / 2)) as AVG_SPREAD_BPS,
COUNT(*) as QUOTE_COUNT
from BINANCE.QTE
where SYMBOL_NAME = 'BTCUSDT'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
group by time_bucket(INTERVAL '1' MINUTE)
limit 1000
```

### Notional Traded and Largest Trades

Activity and liquidity metrics per 1-minute bucket, as seen on the volume and large-print panels of a crypto analytics dashboard. `NOTIONAL` is the traded value `SUM(PRICE * SIZE)` reported in the quote currency, `AVG_TRADE_SIZE` and `MAX_TRADE_SIZE` describe the typical and largest prints, and `MAX(PRICE * SIZE)` surfaces the single largest-notional trade in the bucket.

```sql
select
SUM(SIZE) as VOLUME,
SUM(PRICE * SIZE) as NOTIONAL,
COUNT(*) as TRADE_COUNT,
AVG(SIZE) as AVG_TRADE_SIZE,
MAX(SIZE) as MAX_TRADE_SIZE,
MAX(PRICE * SIZE) as MAX_TRADE_NOTIONAL
from BINANCE.TRD
where SYMBOL_NAME = 'BTCUSDT'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
group by time_bucket(INTERVAL '1' MINUTE)
limit 1000
```

### Rolling Volume and Notional

A trailing liquidity measure computed on every trade, as seen on the rolling volume charts of a crypto analytics dashboard. Rather than fixed buckets, the `OVER` clause defines a 5-minute trailing window ordered by `TIMESTAMP`, so each trade reports the volume, notional, and trade count over the preceding 5 minutes.

```sql
select TIMESTAMP, PRICE, SIZE, AGGRESSOR_SIDE,
SUM(SIZE) over(order by TIMESTAMP asc range interval '5' minute preceding) as ROLLING_VOLUME_5MIN,
SUM(PRICE * SIZE) over(order by TIMESTAMP asc range interval '5' minute preceding) as ROLLING_NOTIONAL_5MIN,
COUNT(*) over(order by TIMESTAMP asc range interval '5' minute preceding) as ROLLING_TRADE_COUNT_5MIN
from BINANCE.TRD
where SYMBOL_NAME = 'BTCUSDT'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

### Hyperliquid OHLCV and VWAP Bars

The same coin analytics apply across crypto venues. Here the trades come from `HYPERLIQUID`, whose perpetual contracts are symboled by the bare coin ticker (for example `BTC` for the Bitcoin perpetual) rather than an exchange pair. The bar construction is otherwise identical to the `BINANCE` example above.

```sql
select
FIRST(PRICE) as OPEN_PRICE,
MAX(PRICE) as HIGH_PRICE,
MIN(PRICE) as LOW_PRICE,
LAST(PRICE) as CLOSE_PRICE,
SUM(SIZE) as VOLUME,
VWAP(PRICE_FIELD_NAME=PRICE, SIZE_FIELD_NAME=SIZE) as VWAP_PRICE,
COUNT(*) as TRADE_COUNT
from HYPERLIQUID.TRD
where SYMBOL_NAME = 'BTC'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
group by time_bucket(INTERVAL '1' MINUTE)
limit 1000
```

## Daily Trade Metrics

Each crypto venue is also published as a companion `_DAILY` database (for example `BINANCE_DAILY` and `DERIBIT_DAILY`) that carries pre-computed end-of-day summaries, so a full trading day can be summarised without scanning the underlying tick data. Two tables are provided:

* `DAY` - one summary tick per symbol per day (`OPEN` / `HIGH` / `LOW` / `CLOSE`, `VOLUME`, `VWAP`, `TRADE_COUNT` and the `BUY_VOLUME` / `SELL_VOLUME` split).
* `STAT` - reference data for each symbol, including its security type (`SEC_TYPE`).

### Daily Trade Rollup Across All Symbols

Retrieve the end-of-day metrics for every symbol on the venue for a single day. The `DAY` table carries one summary tick per symbol, so selecting all symbols for one day returns the daily board for the whole venue. `SYMBOL_NAME LIKE '%'` selects all symbols; narrow the pattern to filter (for example `'BTC%'`).

```sql
select *
from BINANCE_DAILY.DAY
where SYMBOL_NAME LIKE '%'
and TIMESTAMP >= '2026-08-20 00:00:00 UTC'
and TIMESTAMP < '2026-08-21 00:00:00 UTC'
limit 100000
```

### Total Trade Statistics by Security Type

Aggregate the daily trade metrics across all symbols and group them by security type (for example `FX Spot`, `Future`, `Option`). The `DAY` table holds the per-symbol daily metrics and the `STAT` table provides the reference `SEC_TYPE` for each symbol; the two are joined on `SYMBOL_NAME`. `VOLUME`, `TRADE_COUNT` and the `BUY` / `SELL` volume split are summed per security type.

```sql
select
s.SEC_TYPE,
sum(d.VOLUME) as VOLUME,
sum(d.TRADE_COUNT) as TRADE_COUNT,
sum(d.BUY_VOLUME) as BUY_VOLUME,
sum(d.SELL_VOLUME) as SELL_VOLUME
from DERIBIT_DAILY.DAY d, DERIBIT_DAILY.STAT s
where d.SYMBOL_NAME LIKE '%'
and d.SYMBOL_NAME = s.SYMBOL_NAME
and TIMESTAMP >= '2026-08-20 00:00:00 UTC'
and TIMESTAMP < '2026-08-21 00:00:00 UTC'
group by s.SEC_TYPE
limit 100000
```
