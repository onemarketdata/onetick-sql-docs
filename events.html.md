# Earnings Events Analysis

A series of examples showing how to query and analyze corporate events data from OneTick Cloud.
Earnings annoucements are recorded in the EVENT table and can be combined with market data to analyze market reactions and trading patterns around these events.

## Event Data Sources

Earnings annoucement events are available in the daily market data databases:

* `US_COMP_DAILY.EVENT` - US earnings events

## Event Types

The EVENT table records two primary event types:

* `EARNING_DATE` - Earnings announcement dates
* `COMPANY_CONFERENCE_CALL` - Scheduled conference call dates

These events are useful for analyzing market behavior around significant corporate announcements.

## Retrieving Event History for a Symbol

Retrieve all earnings events recorded for a specific symbol within a time range.
This shows all historical earnings announcements for a company.

```sql
select * from US_COMP_DAILY.EVENT
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2026-01-01 00:00:00 America/New_York'
and TIMESTAMP < '2026-06-12 00:00:00 America/New_York'
```

## Querying Event History for a Time Range

Retrieve all earnings events across all symbols that occurred within a specified time range.
This provides a comprehensive view of earnings announcements during a particular period.

```sql
select SYMBOL_NAME, TIMESTAMP, EVENT_TYPE
from US_COMP_DAILY.EVENT
where TIMESTAMP >= '2026-01-01 00:00:00 America/New_York'
and TIMESTAMP < '2026-06-12 00:00:00 America/New_York'
and EVENT_TYPE = 'EARNING_DATE'
order by TIMESTAMP
```

## Joining Daily Pricing to Earnings Events

Correlate daily pricing data with earnings events to analyze closing prices and volume
on earnings announcement dates. A left join ensures all trading dates are preserved,
with events populated only when they occur.

```sql
select d.CLOSE, d.VOLUME,
replace(tostring(e.EVENT_TYPE),'nan','') as EVENT_TYPE
from US_COMP_DAILY.DAY d
left join US_COMP_DAILY.EVENT e on d.SYMBOL_NAME = e.SYMBOL_NAME
and date_trunc('DAY',d.TIMESTAMP,'America/New_York') = date_trunc('DAY',e.TIMESTAMP,'America/New_York')
where d.SYMBOL_NAME = 'CSCO'
and e.SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-01-01 00:00:00 America/New_York'
and TIMESTAMP < '2024-04-01 00:00:00 America/New_York'
and e.EVENT_TYPE = 'EARNING_DATE'
and d.EXCHANGE = ''
```

## Combining Bars with Earnings Events

Merge intraday 1-minute bar data with earnings events on the same day, allowing analysis
of intraday price movement patterns around earnings announcements.

```sql
select OPEN, HIGH, LOW, CLOSE, VOLUME,
0 as EVENT, '' as EVENT_TYPE
from US_COMP.BAR
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-02-14 09:30:00 America/New_York'
and TIMESTAMP < '2024-02-15 16:00:00 America/New_York'
and INTERVAL = 60

UNION ALL

select NAN() as OPEN, NAN() as HIGH, NAN() as LOW, NAN() as CLOSE, 0 as VOLUME,
1 as EVENT, EVENT_TYPE as EVENT_TYPE
from US_COMP_DAILY.EVENT
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-02-14 00:00:00 America/New_York'
and TIMESTAMP < '2024-02-15 00:00:00 America/New_York'
and EVENT_TYPE = 'EARNING_DATE'
```

## Combining Daily Pricing with Earnings Events

Union daily price data with earnings events, creating a combined dataset that shows both
daily price bars and discrete event occurrences. This is useful for time series visualization
and analysis spanning both continuous pricing and discrete events.

```sql
select OPEN, HIGH, LOW, CLOSE, VOLUME,
0 as EVENT, '' as EVENT_TYPE
from US_COMP_DAILY.DAY
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-01-01 00:00:00 America/New_York'
and TIMESTAMP < '2024-04-01 00:00:00 America/New_York'
and EXCHANGE = ''

UNION ALL

select NAN() as OPEN, NAN() as HIGH, NAN() as LOW, NAN() as CLOSE, 0 as VOLUME,
1 as EVENT, EVENT_TYPE as EVENT_TYPE
from US_COMP_DAILY.EVENT
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-01-01 00:00:00 America/New_York'
and TIMESTAMP < '2024-04-01 00:00:00 America/New_York'
and EVENT_TYPE = 'EARNING_DATE'
```

## Combining Trades with Earnings Events

Combine individual trade data with earnings events into a unified stream, creating a single
data source that captures both transaction-level activity and corporate announcements.
The output schema is standardized by setting `NAN()` for price fields in events and
0 for size/volume fields in trades.

```sql
select PRICE, SIZE,
0 as EVENT, '' as EVENT_TYPE
from US_COMP.TRD
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-02-14 00:00:00 America/New_York'
and TIMESTAMP < '2024-02-15 00:00:00 America/New_York'

UNION ALL

select NAN() as PRICE, 0 as SIZE,
1 as EVENT, EVENT_TYPE as EVENT_TYPE
from US_COMP_DAILY.EVENT
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-02-14 00:00:00 America/New_York'
and TIMESTAMP < '2024-02-15 00:00:00 America/New_York'
and EVENT_TYPE = 'EARNING_DATE'
```
