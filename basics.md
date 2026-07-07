# Basics

A series of simple examples are provided showing how to retrieve, filter, aggregate and join datasets, using OneTick Cloud sample databases.

## Data Retrieval

Data can be retrieved by selecting

* Database & Table (separated by a period.  e.g. `LSE_SAMPLE.TRD`).
* Symbol or Symbols using the field `SYMBOL_NAME`
* Time Range using field `TIMESTAMP` with `>=` and `<` or Time equal to, using `TIMESTAMP =`

All three must be provided for results to be retrieved.

```sql
select  PRICE, SIZE, EXCHANGE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

## Time Zones for Filtering

All times are assumed to be UTC.
The filtered time range can be specified in a given time zone by appending the time zone to the timestamp.

```sql
select PRICE, SIZE, EXCHANGE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
limit 10
```

## Time Zones for Resultsets

All times are assumed to be UTC.
The resultset can be also returned in a given time zone.

For `REST` queries:

```python
params = {
    "query_type": "sql",
    "timezone" : "America/New_York",
    "response": "csv",
    "show_times_as_nanos" : "true",
    "all_times_are_readable" : "false",
    "compression":"gzip",
    "statement":sql_statement
    }

access_token = get_access_token(access_token_url, client_id, client_secret)
headers = {
    "Authorization": f"Bearer {access_token}",
    "Content-Type": "application/json"
}
response = requests.post(http_address, json=params, headers=headers)
```

For `onetick.query_webapi` the `otq.run` method should specify the time zone of the response.

```sql
result = otq.run(sql_query,
    http_address=http_address,access_token=access_token,
    output_mode="pandas",
    timezone="America/New_York")
```

For `onetick.py` the `otp.run` method should specify the time zone of the response.

```python
result = otp.run(otp.SqlQuery(sql_statement),
                     timezone='America/New_York')
```

Or the `otp.config.tz` attribute can be set:

```python
otp.config.tz = 'America/New_York'
result = otp.run(otp.SqlQuery(sql_statement))
```

## Sample Databases & Tables

The full list of 200+ Global Equities, Futures, Options & Indices databases is available in
[OneTick Cloud](https://authdash.cloud.onetick.com/web_dashboard/?dash=db_list)

The list of sample databases are additionally included below:

#### **OneTick Cloud Sample Databases**

| Database            | Description                                             | Available Tables                        |
|---------------------|---------------------------------------------------------|-----------------------------------------|
| CA_COMP_SAMPLE      | Consolidated Trades & Quotes Across All Canadian Venues | QTE, NBBO, STAT, TRD                    |
| CA_COMP_SAMPLE_BARS | Consolidated Canadian Trade & Quote 1 Minute Bars       | QTE_1M, TRD_1M                          |
| EU_COMP_SAMPLE      | Consolidated Trades & Quotes Across All European Venues | QTE, STAT, TRD                          |
| EU_COMP_SAMPLE_BARS | Consolidated European Trade & Quote 1 Minute Bars       | QTE_1M, TRD_1M                          |
| LSE_SAMPLE          | London Stock Exchange Trades, Quotes & Book Depth       | DAY, IND, MKT, PRL_FULL, QTE, STAT, TRD |
| LSE_SAMPLE_BARS     | LSE Trade & Quote 1 Minute Bars                         | QTE_1M, TRD_1M, DAY                     |
| TDI_FUT_SAMPLE      | Global Futures  Trades & Quotes                         | QTE, STAT, TRD                          |
| TDI_FUT_SAMPLE_BARS | Global Futures Trades & Quote 1 Minute Bars             | QTE_1M, TRD_1M                          |
| US_COMP_SAMPLE      | Consolidated Trades & Quotes Across All US Venues       | QTE, STAT, TRD                          |
|                     |                                                         |                                         |
| SYMBOL_UNIVERSE     | Symbol Universe across all available Venues             | STAT                                    |
| OQD_MKT_CAL         | Market Holidays & Trading Hours                         | MKTCAL                                  |

Data is stored in standardized tables

#### **OneTick Cloud Standard Tables**

| Table    | Description                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| DAY      | End of Day Record typically covering Closing Price & Open Interest for Derivatives Markets |
| IND      | Indicative Prices occuring during Auction phases                                           |
| QTE      | Quote Events                                                                               |
| STAT     | Static Reference Data for the Instrument                                                   |
| TRD      | Trade Events                                                                               |
| NBBO     | National Best Bid & Offer Quotes                                                           |
| PRL      | Book Depth - Market By Level                                                               |
| PRL_FULL | Book Depth - Market by Order                                                               |
| MKTCAL   | Market Holiday & Trading Hours                                                             |
| TRD_1M   | 1 Minute Trade Bar                                                                         |
| QTE_1M   | 1 Minute Quote Bar                                                                         |

## Field Selection

All of the fields in a table can be retrieved by using the `*` syntax.

```sql
select *
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
```

A limited set of fields can be specified by replacing the `*` with the comma separated set of fields.

```sql
select  PRICE, SIZE, EXCHANGE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
```

## Limiting Returned Rows

The set of returned rows can be limited using the `LIMIT` and `OFFSET` clauses.
`LIMIT` specifies how many rows to retrieve.

```sql
select  PRICE, SIZE, EXCHANGE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

While `OFFSET` specifis how many initial records should be skipped before returning results.

```sql
select  PRICE, SIZE, EXCHANGE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10 offset 5
```

## Retrieving Multiple Symbols

Multiple symbols can be retrieved through specifying `IN` or `LIKE`.

```sql
SELECT * FROM LSE_SAMPLE.TRD
WHERE SYMBOL_NAME in ('VOD','BARC')
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

```sql
SELECT * FROM LSE_SAMPLE.TRD
WHERE SYMBOL_NAME LIKE 'BA%'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

## Adding Calculated Fields

Additional fields can be added through calculation and assigned an alias.

```sql
SELECT PRICE, SIZE, TRADE_CURRENCY, (PRICE*SIZE) as TRD_VALUE FROM LSE_SAMPLE.TRD
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

## Adding Filters

Results can be filtered by adding additional filters to the `WHERE` clause.

```sql
SELECT PRICE, SIZE, TRADE_CURRENCY, (PRICE*SIZE) as TRD_VALUE FROM LSE_SAMPLE.TRD
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and TRADE_CURRENCY = 'EUR'
and SIZE > 100
LIMIT 10
```

## Aggregating Across Results

Results can be aggregated by specifying an aggregation for each field and defining an alias.

```sql
select
AVG(PRICE) as MEAN_PRICE,
STDDEV(PRICE) as STDDEV_PRICE,
MEDIAN(PRICE) as MEDIAN_PRICE,
MAX(PRICE) as MAX_PRICE,
MIN(PRICE) as MIN_PRICE,
TW_AVG(PRICE) as TWA_PRICE,
SUM(PRICE*SIZE)/SUM(SIZE) as VWAP_PRICE,
COUNT(PRICE) as COUNT_PRICE
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
```

Additionally results can be grouped by adding the `GROUP BY` clause.

```sql
select  TRADE_CURRENCY,
AVG(PRICE) as MEAN_PRICE,
STDDEV(PRICE) as STDDEV_PRICE,
MEDIAN(PRICE) as MEDIAN_PRICE,
MAX(PRICE) as MAX_PRICE,
MIN(PRICE) as MIN_PRICE,
TW_AVG(PRICE) as TWA_PRICE,
SUM(PRICE*SIZE)/SUM(SIZE) as VWAP_PRICE,
COUNT(PRICE) as COUNT_PRICE
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
group by TRADE_CURRENCY
```

## Query Data Availability Status

The data availability status can be queried to determine when data loads have completed for a specific database.
This is useful for monitoring data freshness and verifying that recent data is available before running analysis queries.

Data availability is tracked through the `DB_INFO.PROC_EVENTS` table, which contains load completion events for each database.
The `EVENT_NAME` field indicates the type of event, with `'Load finished'` indicating successful data load completion.

```sql
SELECT *
FROM DB_INFO.PROC_EVENTS                      -- Database info and process events table
WHERE SYMBOL_NAME LIKE '%'                    -- Filter for all symbols using wildcard
  AND TIMESTAMP >= DATEADD('DAY', -7, NOW())  -- Include events from the last 7 days
  AND TIMESTAMP < NOW()                       -- Exclude future events, capture up to current time
  AND EVENT_NAME = 'Load finished'            -- Filter to only successful load completion events
```

## Point in Time Trade Snapshot Across Venue

Retrieves a snapshot of trade data at a specific point in time across a venue, looking back a specified number of seconds to find the prevailing trade before the selected time.

A specific point in time is selected with the TIMESTAMP equal to a specified value. All Symbols are retrieved with SYMBOL_NAME LIKE ‘%’. A Lookback is defined in seconds, to check for the prevailing trade before the selected time, up to the lookback period.

```sql
 select SYMBOL_NAME, PRICE, SIZE, COND, EXCHANGE
 from US_COMP_SAMPLE.TRD
 where SYMBOL_NAME LIKE '%'          -- Retrieve All Symbols
 and TIMESTAMP = '2024-01-03 12:30:00 America/New_York'
 and init_lookback = 86400           --how many seconds to look back for prevailing value
```

## Point in Time Trade and Quote Snapshot Across Venue

Retrieves a combined snapshot of both trade and quote data at a specific point in time across a venue, using a lookback period to find the most recent data before the selected time.

A specific point in time is selected with the TIMESTAMP equal to a specified value. All Symbols are retrieved with SYMBOL_NAME LIKE ‘%’. A Lookback is defined in seconds, to check for the prevailing trade and quote before the selected time, up to the lookback period.

```sql
 select t.SYMBOL_NAME as SYMBOL_NAME,
 t.PRICE as PRICE, t.SIZE as SIZE,
 q.BID_PRICE as BID_PRICE, q.ASK_PRICE as ASK_PRICE
 from LSE_SAMPLE.TRD t, LSE_SAMPLE.QTE q
 where t.SYMBOL_NAME LIKE '%'          -- Retrieve All Symbols
 and t.SYMBOL_NAME = q.SYMBOL_NAME
 and TIMESTAMP = '2024-01-03 12:30:00 Europe/London'
 and t.init_lookback = 86400           --how many seconds to look back for prevailing Trade
 and q.init_lookback = 86400           --how many seconds to look back for prevailing Quote
```

## Spread and Mid from Venue

Calculates bid-ask spread and mid-price from quote data at a venue, providing key pricing metrics for market analysis.

Venue datasets include Top of Book Quotes in the QTE table. SPREAD = ASK_PRICE - BID_PRICE and MID_PRICE = (BID_PRICE + ASK_PRICE)/2.

```sql
 select BID_PRICE, ASK_PRICE,
 (BID_PRICE + ASK_PRICE)/2 as MID_PRICE,
 (ASK_PRICE - BID_PRICE) as SPREAD
 from LSE_SAMPLE.QTE
 where SYMBOL_NAME = 'VOD'
 and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
 and TIMESTAMP < '2024-01-03 16:00:00 Europe/London'
 and BID_SIZE > 0 and ASK_SIZE > 0
 limit 1000
```

## Spread and Mid Statistics from Venue

Aggregates spread and mid-price statistics from venue quote data, calculating time-weighted averages and extremes across a trading period.

Venue datasets include Top of Book Quotes in the QTE table. Statistics across the period calculate the Time Weighted Average, High and Low for spread and mid-price metrics.

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
   from LSE_SAMPLE.QTE
   where SYMBOL_NAME = 'VOD'
   and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
   and TIMESTAMP < '2024-01-03 16:00:00 Europe/London'
   and BID_SIZE > 0 and ASK_SIZE > 0
 )
```

## Retrieving Valid Quotes for Specified Trading Sessions

Retrieves valid quotes for a specified market phase or trading session by joining quote data with market phase information. Quotes are filtered to those with valid sizes and then matched with the prevailing market phase. This allows analysis of quotes during specific trading periods such as continuous trading.

```sql
 select *
 from (
 select q.BID_PRICE as BID_PRICE, q.ASK_PRICE as ASK_PRICE,
 m.OMD_STATUS as OMD_STATUS
 from LSE_SAMPLE.QTE q, LSE_SAMPLE.MKT m
 where q.SYMBOL_NAME='VOD'
 and q.SYMBOL_NAME = m.SYMBOL_NAME
 and sametime_as_existing(q.TIMESTAMP, m.TIMESTAMP, 0) = TRUE
 and TIMESTAMP >= '2024-01-03 00:00:00 Europe/London'
 and TIMESTAMP < '2024-01-04 00:00:00 Europe/London'
 and q.BID_SIZE > 0 and q.ASK_SIZE > 0
 )
 where OMD_STATUS = 'T'
```
