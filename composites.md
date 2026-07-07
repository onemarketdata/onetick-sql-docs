# Composite Market Data Analysis

A series of simple examples are provided showing how to analyze consolidated market data across multiple venues using OneTick Cloud composite databases.
Composite databases aggregate trades and quotes from multiple exchanges and venues into a single unified dataset.

## Composite Databases

Composite databases consolidate fragmented liquidity across multiple equity venues for a given region or country.
Consolidating trades, quotes, and providing an NBBO and daily rollup.

This is available for:

* `AU_COMP` - Consolidated Across All Australian Equity Venues
* `CA_COMP` - Consolidated Across All Canadian Equity Venues
* `EU_COMP` - Consolidated Across All European Equity Venues
* `IN_COMP` - Consolidated Across All Indian Equity Venues
* `JP_COMP` - Consolidated Across All Japanese Equity Venues
* `KR_COMP` - Consolidated Across All Korean Equity Venues
* `MX_COMP` - Consolidated Across All Mexican Equity Venues
* `TW_COMP` - Consolidated Across All Taiwanese Equity Venues
* `US_COMP` - Consolidated Across All US Equity Venues

Samples of the Composite datasets covering the first 3 months of 2024 are available for:

* `CA_COMP_SAMPLE` - Consolidated Trades & Quotes Across All Canadian Venues
* `EU_COMP_SAMPLE` - Consolidated Trades & Quotes Across All European Venues
* `US_COMP_SAMPLE` - Consolidated Trades & Quotes Across All US Venues

Data is aggregated by venue in fields such as `QUOTE_VENUE`, `TRADE_VENUE`, or `EXCHANGE` (for US_COMP).

## Quote Count Per Venue

The number of quotes received from each venue can be calculated by aggregating quote data.
This provides visibility into the trading activity and data volume from each exchange.

```sql
select QUOTE_VENUE, COUNT(*) as QUOTE_COUNT
from CA_COMP_SAMPLE.QTE
where SYMBOL_NAME='TD'
and TIMESTAMP >= '2024-01-03 09:30:00 UTC'
and TIMESTAMP < '2024-01-03 16:00:00 UTC'
group by QUOTE_VENUE
```

## Trade Count Per Venue

The number of trades executed on each venue can be calculated by aggregating trade data.
This shows the distribution of trading activity across different exchanges.

```sql
select TRADE_VENUE, COUNT(*) as TRADE_COUNT
from CA_COMP_SAMPLE.TRD
where SYMBOL_NAME='TD'
and TIMESTAMP >= '2024-01-03 09:30:00 UTC'
and TIMESTAMP < '2024-01-03 16:00:00 UTC'
group by TRADE_VENUE
```

## Volume Traded Across All Symbols Per Venue

The total volume traded across all symbols for each venue can be calculated using daily summary data.
This shows the overall trading volume aggregated by venue.

Daily volume data from the `DAY` table provides pre-aggregated volume statistics, avoiding the need to aggregate individual trades.
The composite venue (empty string `''`) and primary venue (`'PRIM'`) are typically filtered out to show individual venue volumes.

```sql
select sum(VOLUME) as VOLUME, EXCHANGE
from US_COMP_SAMPLE_DAILY.DAY
where SYMBOL_NAME LIKE '%'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and EXCHANGE!=''
and EXCHANGE!='PRIM'
group by EXCHANGE
```

## Daily Composite Trade Bars

Returns aggregated Daily OHLCV (Open, High, Low, Close, Volume) data for a specified symbol and date range from composite databases.
Multiple rows are returned per day, with each row representing either the exchange/venue or aggregated composite data.
In US_COMP, venues are identified by the `EXCHANGE` field, while other composites use `VENUE_ID`.

Each daily bar includes comprehensive volume and price information:

* `VOLUME` - Total volume
* `VOLUME_ODD_LOT` - Volume from odd-lot trades (< 100 shares)
* `VOLUME_ROUND_LOT` - Volume from round-lot trades (>= 100 shares)
* `VOLUME_OFF_EXCHANGE` - Volume from off-exchange trades
* `VOLUME_PRE_MARKET` - Pre-market volume (before market open)
* `VOLUME_POST_MARKET` - Post-market volume (after market close)
* `VOLUME_OPENING_AUCTION` - Volume from opening auction
* `VOLUME_CLOSING_AUCTION` - Volume from closing auction
* `OPEN_PRICE` - Opening price
* `CLOSE_PRICE` - Closing price
* `PRICE_OPENING_AUCTION` - Opening auction execution price
* `PRICE_CLOSING_AUCTION` - Closing auction execution price
* `HIGH_PRICE` - Highest price during the day
* `LOW_PRICE` - Lowest price during the day

```sql
select * from US_COMP_SAMPLE_DAILY.DAY
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 America/New_York'
and TIMESTAMP < '2024-01-04 00:00:00 America/New_York'
limit 1000
```

## Daily Quote NBBO Composite Bars

Returns aggregated Daily NBBO (National Best Bid and Offer) data from composite quote databases.
Similar to trade bars, multiple rows represent different venues, the primary exchange, and the composite aggregate.
Useful for analyzing best bid/ask pricing across consolidated venues.

```sql
select * from US_COMP_SAMPLE_DAILY.QTE
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 America/New_York'
and TIMESTAMP < '2024-01-04 00:00:00 America/New_York'
limit 1000
```

## Daily Trade NBBO Composite Bars

Returns aggregated Daily Trade and NBBO data combined from composite databases.
Provides unified view of both trade activity (OHLCV) and prevailing bid/ask prices on a daily basis.
Useful for complete market structure analysis combining executions with prevailing quotes.

```sql
select * from US_COMP_SAMPLE_DAILY.NBBO
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 America/New_York'
and TIMESTAMP < '2024-01-04 00:00:00 America/New_York'
limit 1000
```

## NBBO Retrieval from Composite

Retrieves the consolidated National Best Bid and Offer (NBBO) quote data from the composite. For the US, NBBOs are pre-calculated by the consolidated tape. For other regions, NBBOs are constructed by OneTick across all venues that contribute to the composite. NBBO records include bid and ask prices, sizes, and the exchanges providing the best bid and offer.

```sql
 select * from US_COMP_SAMPLE.NBBO
 where SYMBOL_NAME='AAPL'
 and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
 and TIMESTAMP < '2024-01-04 00:00:00 UTC'
 limit 1000
```

## Trade Volume to NBBO Calculation

Classifies and aggregates trade volumes by their position relative to the National Best Bid and Offer (NBBO).
This analysis shows how many shares were executed at the bid, at the ask, inside the spread, at the mid price, and outside the NBBO.
Useful for understanding order execution quality, market impact analysis, and trade classification for TCA (Transaction Cost Analysis).

The analysis joins trade data with prevailing NBBO quotes and classifies each trade into one of four categories:

* `VOLUME_AT_MID` - Volume executed at the midpoint price
* `VOLUME_INSIDE_NBBO` - Volume executed strictly inside the bid-ask spread (between bid and ask, but not at either)
* `VOLUME_AT_NBBO` - Volume executed at the bid or ask price
* `VOLUME_OUTSIDE_NBBO` - Volume executed outside the NBBO (below bid or above ask)

```sql
SELECT
    t.SYMBOL_NAME,
    t.EXCHANGE,
    SUM(SIZE_AT_MID) AS VOLUME_AT_MID,
    SUM(SIZE_INSIDE_NBBO) AS VOLUME_INSIDE_NBBO,
    SUM(SIZE_AT_NBBO) AS VOLUME_AT_NBBO,
    SUM(SIZE_OUTSIDE_NBBO) AS VOLUME_OUTSIDE_NBBO,
    COUNT(*) AS TRD_COUNT
FROM (
    SELECT
        t.SYMBOL_NAME,
        t.PRICE,
        t.SIZE,
        t.EXCHANGE,
        q.BID_PRICE,
        q.ASK_PRICE,
        (q.BID_PRICE + q.ASK_PRICE) / 2 AS MID_PRICE,
        CASE
            WHEN t.PRICE = (q.BID_PRICE + q.ASK_PRICE) / 2 THEN t.SIZE
            ELSE 0
        END AS SIZE_AT_MID,
        CASE
            WHEN t.PRICE > q.BID_PRICE AND t.PRICE < q.ASK_PRICE THEN t.SIZE
            ELSE 0
        END AS SIZE_INSIDE_NBBO,
        CASE
            WHEN t.PRICE = q.BID_PRICE OR t.PRICE = q.ASK_PRICE THEN t.SIZE
            ELSE 0
        END AS SIZE_AT_NBBO,
        CASE
            WHEN t.PRICE < q.BID_PRICE OR t.PRICE > q.ASK_PRICE THEN t.SIZE
            ELSE 0
        END AS SIZE_OUTSIDE_NBBO
    FROM US_COMP_SAMPLE.TRD t, US_COMP_SAMPLE.NBBO q
    WHERE t.SYMBOL_NAME = q.SYMBOL_NAME
        AND sametime_as_existing(t.TIMESTAMP, q.TIMESTAMP, 0) = TRUE
        AND t.SYMBOL_NAME = 'CSCO'
        AND TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
        AND TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
) t
GROUP BY t.SYMBOL_NAME, t.EXCHANGE
ORDER BY TRD_COUNT DESC
```

## Point in Time Trade and NBBO Snapshot for Composite

Retrieves a combined snapshot of trade and NBBO quote data at a specific point in time for a composite dataset, using a lookback period to find the most recent data.

A specific point in time is selected with `TIMESTAMP` equal to a specified value. All Symbols are retrieved with `SYMBOL_NAME LIKE '%'`. A Lookback is defined in seconds, to check for the prevailing trade and NBBO before the selected time, up to the lookback period. Both the TRD and NBBO tables are retrieved from the composite.

```sql
 select t.SYMBOL_NAME as SYMBOL_NAME,
 t.PRICE as PRICE, t.SIZE as SIZE,
 n.BID_PRICE as BID_PRICE, n.ASK_PRICE as ASK_PRICE
 from US_COMP_SAMPLE.TRD t, US_COMP_SAMPLE.NBBO n     -- Both the TRD and NBBO tables are retrieved
 where t.SYMBOL_NAME LIKE '%'          -- Retrieve All Symbols
 and t.SYMBOL_NAME = n.SYMBOL_NAME
 and TIMESTAMP = '2024-01-03 12:30:00 America/New_York'
 and t.init_lookback = 86400           --how many seconds to look back for prevailing Trade
 and n.init_lookback = 86400           --how many seconds to look back for prevailing NBBO Quote
```

## Spread and Mid from Composite Quotes Per Venue

Calculates bid-ask spread and mid-price from composite quotes by venue/exchange, providing venue-level pricing analysis.

Composite datasets include both quotes per venue/exchange and an NBBO. The `QTE` table includes the Top of Book Quotes associated with each venue. US_COMP uses the `EXCHANGE` field, while other Composites use `QUOTE_VENUE`. `SPREAD = ASK_PRICE - BID_PRICE` and `MID_PRICE = (BID_PRICE + ASK_PRICE)/2`.

```sql
 select EXCHANGE, BID_PRICE, ASK_PRICE,
 (BID_PRICE + ASK_PRICE)/2 as MID_PRICE,
 (ASK_PRICE - BID_PRICE) as SPREAD
 from US_COMP_SAMPLE.QTE
 where SYMBOL_NAME = 'CSCO'
 and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
 and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 and BID_SIZE > 0 and ASK_SIZE > 0
 limit 1000
```

## Rolling Spread and Mid Statistics from Composite NBBO

Calculates rolling spread and mid-price statistics at each point in time from composite NBBO data, showing how these metrics evolve throughout a trading period.

Composite datasets include both quotes per venue/exchange and an NBBO. The NBBO can either be regulatory in the case of the US, or constructed. Rolling Statistics across the period calculate the Time Weighted Average, High and Low at every point in time for spread and mid-price metrics.

```sql
 select
 BID_PRICE, ASK_PRICE, MID_PRICE, SPREAD,
 TW_AVG(MID_PRICE) OVER(order by TIMESTAMP asc) as  ROLLING_TW_AVG_MID_PRICE,
 MAX(MID_PRICE) OVER(order by TIMESTAMP asc) as  ROLLING_HIGH_MID_PRICE,
 MIN(MID_PRICE) OVER(order by TIMESTAMP asc) as  ROLLING_LOW_MID_PRICE,
 TW_AVG(SPREAD) OVER(order by TIMESTAMP asc) as  ROLLING_TW_AVG_SPREAD,
 MAX(SPREAD) OVER(order by TIMESTAMP asc) as  ROLLING_HIGH_SPREAD,
 MIN(SPREAD) OVER(order by TIMESTAMP asc) as  ROLLING_LOW_SPREAD
 from
 (
   -- Calculate the Mid Price and Spread
   select BID_PRICE, ASK_PRICE,
   (BID_PRICE + ASK_PRICE)/2 as MID_PRICE,
   (ASK_PRICE - BID_PRICE) as SPREAD
   from US_COMP_SAMPLE.NBBO
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
   and BID_SIZE > 0 and ASK_SIZE > 0
 )
```

## Spread and Mid from Composite NBBO

Calculates bid-ask spread and mid-price from composite NBBO (National Best Bid and Offer) data, providing consolidated pricing metrics across all venues.

Composite datasets include both quotes per venue/exchange and an NBBO. The NBBO can either be regulatory in the case of the US, or constructed. SPREAD = ASK_PRICE - BID_PRICE and MID_PRICE = (BID_PRICE + ASK_PRICE)/2.

```sql
 select BID_PRICE, ASK_PRICE,
 (BID_PRICE + ASK_PRICE)/2 as MID_PRICE,
 (ASK_PRICE - BID_PRICE) as SPREAD
 from US_COMP_SAMPLE.NBBO
 where SYMBOL_NAME = 'CSCO'
 and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
 and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 and BID_SIZE > 0 and ASK_SIZE > 0
 limit 1000
```

## Spread and Mid Statistics from Composite NBBO

Aggregates spread and mid-price statistics from composite NBBO quote data, calculating time-weighted averages and extremes across a trading period.

Composite datasets include both quotes per venue/exchange and an NBBO. The NBBO can either be regulatory in the case of the US, or constructed. Statistics across the period calculate the Time Weighted Average, High and Low for spread and mid-price metrics.

```sql
 select
 TW_AVG(MID_PRICE) as TW_AVG_MID_PRICE,
 MAX(MID_PRICE) as HIGH_MID_PRICE,
 MIN(MID_PRICE) as LOW_MID_PRICE,
 TW_AVG(SPREAD) as TW_AVG_SPREAD,
 MAX(SPREAD) as HIGH_SPREAD,
 MIN(SPREAD) as LOW_SPREAD,
 count(SPREAD) as QTE_COUNT
 from
 (
   -- Calculate the Mid Price and Spread
   select BID_PRICE, ASK_PRICE,
   (BID_PRICE + ASK_PRICE)/2 as MID_PRICE,
   (ASK_PRICE - BID_PRICE) as SPREAD
   from US_COMP_SAMPLE.NBBO
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
   and BID_SIZE > 0 and ASK_SIZE > 0
 )
```

## Spread and Mid Statistics from Composite Venues

Aggregates spread and mid-price statistics from composite venue quotes, grouped by exchange to show venue-level pricing analysis.

Composite datasets include both quotes per venue/exchange and an NBBO. The QTE table includes the Top of Book Quotes associated with each venue. US_COMP uses the EXCHANGE field, while other Composites use QUOTE_VENUE. Statistics across the period calculate the Time Weighted Average, High and Low, grouped by EXCHANGE.

```sql
 select
 EXCHANGE,
 TW_AVG(MID_PRICE) as TW_AVG_MID_PRICE,
 MAX(MID_PRICE) as HIGH_MID_PRICE,
 MIN(MID_PRICE) as LOW_MID_PRICE,
 TW_AVG(SPREAD) as TW_AVG_SPREAD,
 MAX(SPREAD) as HIGH_SPREAD,
 MIN(SPREAD) as LOW_SPREAD,
 count(SPREAD) as QTE_COUNT
 from
 (
   -- Calculate the Mid Price and Spread
   select EXCHANGE, BID_PRICE, ASK_PRICE,
   (BID_PRICE + ASK_PRICE)/2 as MID_PRICE,
   (ASK_PRICE - BID_PRICE) as SPREAD
   from US_COMP_SAMPLE.QTE
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
   and BID_SIZE > 0 and ASK_SIZE > 0
 )
 group by EXCHANGE
```

## European Composite Data

The European Composite (`EU_COMP`) consolidates trades and quotes across multiple European venues. Symbols in `EU_COMP` are defined as ISINs rather than ticker symbols, as ticker symbols are not consistent across European venues. Trades include additional MiFID-related fields such as trading venue, currency, trade period, and book type.

## European Composite Trade Data Retrieval

Retrieves trade records from the European Composite across multiple venues. Symbols are defined as ISINs to provide consistency across European trading venues.

```sql
 SELECT * FROM EU_COMP_SAMPLE.TRD
 WHERE SYMBOL_NAME in ('GB00BH4HKS39','FR001400J770','DE0005140008','GB00BP6MXD84')
 and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
 and TIMESTAMP < '2024-01-03 16:00:00 Europe/London'
 LIMIT 1000
```

## European Composite Bar Creation

Returns OHLC prices for the European Composite aggregated by venue and currency into fixed time intervals. The query applies grouping by `TRADE_VENUE` and `CURRENCY` to return calculated bars per venue, since the European Composite includes trades in multiple currencies.

```sql
 SELECT TRADE_VENUE, CURRENCY,
 FIRST(PRICE) as FIRST_PRICE,
 MAX(PRICE) as HIGH_PRICE,
 MIN(PRICE) as LOW_PRICE,
 LAST(PRICE) as LAST_PRICE,
 VWAP(PRICE_FIELD_NAME=PRICE, SIZE_FIELD_NAME=SIZE) as VWAP_PRICE,
 SUM(SIZE) as VOLUME,
 SUM(PRICE*SIZE) as TRADE_VALUE,
 COUNT(SIZE) as TRADE_COUNT
 FROM EU_COMP_SAMPLE.TRD
 WHERE SYMBOL_NAME = 'GB00BH4HKS39'
 and TIMESTAMP >= '2024-01-03 10:23:01 Europe/London'
 and TIMESTAMP < '2024-01-03 10:27:48 Europe/London'
 group by TRADE_VENUE, CURRENCY
```

## European Composite Bar Creation with Fill Forward

Returns OHLC prices for the European Composite with the last price filled forward for periods without trades. Uses `TIME_SERIES_TYPE='STATE_TS'` to extend the last price forward into subsequent time buckets. Filters trades to continuous trading periods and lit order book trades.

```sql
 SELECT TRADE_VENUE,CURRENCY,
 FIRST(PRICE) as FIRST_PRICE,
 MAX(PRICE) as HIGH_PRICE,
 MIN(PRICE) as LOW_PRICE,
 LAST(PRICE,TIME_SERIES_TYPE='STATE_TS') as LAST_PRICE,
 VWAP(PRICE_FIELD_NAME=PRICE, SIZE_FIELD_NAME=SIZE) as VWAP_PRICE,
 SUM(SIZE) as VOLUME,
 COUNT(SIZE) as TRADE_COUNT
 FROM EU_COMP.TRD
 WHERE SYMBOL_NAME = 'GB00BH4HKS39'
 and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
 and TIMESTAMP < '2024-01-03 16:00:00 Europe/London'
 and TRADE_PERIOD = '-'
 and BOOK_TYPE = '0'
 group by time_bucket(INTERVAL '5' MINUTE), TRADE_VENUE, CURRENCY
```

## European Trade Daily Composite Bars

Returns pre-calculated Daily OHLCV data from the European Composite for specified symbols and date ranges. Multiple rows are returned per day representing different venues and currencies. The composite aggregate is represented by empty string values for `VENUE_ID` and `CURRENCY` fields.

```sql
 SELECT * FROM EU_COMP.DAY
 WHERE SYMBOL_NAME ='GB00BH4HKS39'
 and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
 and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
 LIMIT 1000
```
