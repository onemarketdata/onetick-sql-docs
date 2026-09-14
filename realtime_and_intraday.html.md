# Real Time & Intraday

This section contains examples of retrieving Real time snapshots, and intraday retrieval.
Real Time data typically requires exchange agreements and associated entitlement.

The `LATEST` databases provide last value caches or Market Snapshots, storing the latest prices for each instrument.
`_LATEST` is added as a suffix to all existing real time sources.  e.g. `US_COMP_LATEST`
They can only be accessed by those who are entitled to access real time data.

Snapshot tables are queried across a time range covering the current day, from `TODAY()` up to `NOW()`, rather than at a single instant.
The `TODAY` function accepts an optional time zone, e.g. `TODAY('America/New_York')`.

## Latest Trade Market Snapshot

The `SNAP_TRD` table includes latest trade for every symbol.
Querying by `SYMBOL_NAME` returns all symbols.  To filter by symbol, please use the `SYMBOL` field.

```sql
select * from US_COMP_LATEST.SNAP_TRD
where symbol_name = '-'
and TIMESTAMP >= TODAY()
and TIMESTAMP < NOW()
```

## Latest Quote Market Snapshot

The `SNAP_QTE` table includes latest quote for every symbol.  It is not available for Composite databases which use `SNAP_NBBO`.
Querying by `SYMBOL_NAME` returns all symbols.  To filter by symbol, please use the `SYMBOL` field.

```sql
select * from CME_GLOBEX_LATEST.SNAP_QTE
where symbol_name = '-'
and TIMESTAMP >= TODAY()
and TIMESTAMP < NOW()
```

## Latest NBBO Market Snapshot

The `SNAP_NBBO` table includes latest NBBO quote for every symbol.  It is only available for Composite databases such as `US_COMP_LASTEST`.
Other databases provide `SNAP_QTE`.
Querying by `SYMBOL_NAME` returns all symbols.  To filter by symbol, please use the `SYMBOL` field.

```sql
select * from US_COMP_LATEST.SNAP_NBBO
where symbol_name = '-'
and TIMESTAMP >= TODAY()
and TIMESTAMP < NOW()
```

## Latest Market Snapshot

The `SNAP` table includes the combined latest trade and quote or NBBO for every symbol.
Querying by `SYMBOL_NAME` returns all symbols.  To filter by symbol, please use the `SYMBOL` field.
The latest trade time is stored in field `LAST_TRADE_TIME`, and the latest Quote or NBBO update is stored in field `LAST_QUOTE_TIME`.

```sql
select * from US_COMP_LATEST.SNAP
where symbol_name = '-'
and TIMESTAMP >= TODAY()
and TIMESTAMP < NOW()
```

## Returns Recent Trades

Data will only be returned for those who are entitled to access real time data.
The `US_COMP_REPLAY` database replays older data, and is available to all as a sample database.
Trades across a recent time period are retrieved by filtering on  time range relative to the current time (`NOW`).

The `NOW` function returns data upto the current time.
The `DATEADD` function allow the start time to be offset to `NOW` by a specified amount. (e.g. by 15 minutes).
Possible intervals for `DATEADD` include `YEAR`, `MONTH`, `DAY`, `HOUR`, `MINUTE` and `SECOND`.

```sql
select *
from US_COMP_REPLAY.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= DATEADD('MINUTE',-15,NOW())
and TIMESTAMP < NOW()
limit 100
```

## Returns Todays Trades

Data will only be returned for those who are entitled to access real time data.
The US_COMP_REPLAY database replays older data, and is available to all.
Trades from Today are retrieved by filtering on a time range covering the start of day until the present,
using the `TODAY` and `NOW` functions.

The `TODAY` function allows specification of the required Time Zone.

```sql
select *
from US_COMP_REPLAY.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= TODAY('America/New_York')
and TIMESTAMP < NOW()
limit 100
```

## Returns Todays Trade Bars

Data will only be returned for those who are entitled to access real time data.
Pre-calculated 1 minute trade bars for the current day are retrieved from the `TRD_1M` table of the bar database,
filtering on a time range covering the start of day until the present, using the `TODAY` and `NOW` functions.

The `TODAY` function allows specification of the required Time Zone.

```sql
select *
from US_COMP_BARS.TRD_1M
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= TODAY('America/New_York')
and TIMESTAMP < NOW()
limit 100
```

## Dynamic Bar Calculation for Today

Bars can also be generated on the fly from todays trades rather than read from a pre-calculated bar database.
Data is grouped by time bucket / time bin / time bar, with the `time_bucket` function, which specifies an interval, e.g. `time_bucket(INTERVAL '5' MINUTE)`.
Intervals can be `MILLISECOND`, `SECOND`, `MINUTE`, `HOUR`, `DAY` or `WEEK`.

By default the `Timestamp` associated with a time bucket represents the end of the bucket.
This can be changed to represent the start of the time bucket by adding `bucket_start` as an additional optional argument to the `time_bucket` function.

Additionally a filter is applied using `IS_CHARACTER_PRESENT(COND,'IBCGHLMNPQRVWZ479') = FALSE` to ensure certain trade conditions are ignored, and do not contribute to the bar generation.

```sql
select
FIRST(PRICE) as first_price,
MAX(PRICE) as high_price,
MIN(PRICE) as low_price,
LAST(PRICE) as last_price,
SUM(SIZE) as sum_size,
COUNT(*) as trade_count
from US_COMP.TRD
where symbol_name = 'CSCO'
and TIMESTAMP >= TODAY('America/New_York')
and TIMESTAMP < NOW()
and IS_CHARACTER_PRESENT(COND,'IBCGHLMNPQRVWZ479') = FALSE
group by time_bucket(INTERVAL '5' MINUTE)
limit 1000
```
