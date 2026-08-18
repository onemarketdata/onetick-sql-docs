<a id="crypto"></a>

# Crypto

This section contains examples of retrieving and analyzing cryptocurrency market data from a crypto venue using OneTick SQL.

Crypto venues such as `BINANCE` provide trades, quotes, and order book updates. Unlike Equities and Futures, the trade and quote sizes on a crypto venue are fractional, so order book functions require the `SIZE_MAX_FRACTIONAL_DIGITS` attribute to reconstruct the book with sizes stored to the required number of fractional digits.

<a id="crypto-trade-retrieval"></a>

## Crypto Trade Retrieval

Retrieve trades from a crypto venue. The trade size is fractional, unlike Equities and Futures.

```sql
select * from BINANCE.TRD
where SYMBOL_NAME = 'BTCUSD'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

<a id="crypto-quote-retrieval"></a>

## Crypto Quote Retrieval

Retrieve quotes from a crypto venue. The bid and ask sizes are fractional, unlike Equities and Futures.

```sql
select * from BINANCE.QTE
where SYMBOL_NAME = 'BTCUSD'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

<a id="crypto-book-update-retrieval"></a>

## Crypto Book Update Retrieval

Retrieve book updates from a crypto venue. Book updates are provided as an L2 dataset in the `PRL` table, providing updates to price levels. Basic retrieval is useful for counting order book changes. To reconstruct the order book, the `OB_SNAPSHOT` functions should be used.

```sql
select * from BINANCE.PRL
where SYMBOL_NAME = 'BTCUSD'
and TIMESTAMP >= '2026-07-28 00:00:00.000 GMT'
and TIMESTAMP < '2026-07-29 00:00:00.000 GMT'
limit 1000
```

<a id="crypto-book-snapshot-retrieval"></a>

## Crypto Book Snapshot Retrieval

Reconstruct the order book at a specified time using the `OB_SNAPSHOT_WIDE` function, which returns the book with the bid and ask on the same row. As this is a crypto book, the `SIZE_MAX_FRACTIONAL_DIGITS` attribute is set, allowing the book to be reconstructed with size stored to up to 9 fractional digits.

```sql
SELECT BID_PRICE, BID_SIZE, ASK_PRICE, ASK_SIZE, LEVEL
FROM OTQ_CHAIN."OB_SNAPSHOT_WIDE(SIZE_MAX_FRACTIONAL_DIGITS=9);PRL"
where SYMBOL_NAME = 'BINANCE::BTCUSD'
and TIMESTAMP = '2026-07-28 12:00:00.000 GMT'
```

<a id="crypto-book-snapshot-retrieval-with-accumulative-values"></a>

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

<a id="crypto-book-snapshot-retrieval-with-accumulative-values-and-best-prices"></a>

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

<a id="crypto-book-depth-statistics-to-trade-a-specified-amount-across-time"></a>

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
