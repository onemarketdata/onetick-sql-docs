# Options

A series of simple examples are provided showing how to retrieve options data, Greeks, and analytics from OneTick Cloud.

## Options Greeks

Retrieves end-of-day options data for AAPL contracts from the US Options EOD sample dataset. This query returns all available fields including Greeks (delta, gamma, theta, vega, rho), implied volatility metrics, open interest, volume, bid/ask close prices, and underlying price for a single trading day. The `LIKE 'AAPL%'` pattern matches all AAPL option contracts (calls and puts across all strikes and expirations).

```sql
select *
from US_OPTIONS_EOD_SAMPLE.DAY
where symbol_name LIKE 'AAPL%'
and TIMESTAMP >= '2025-01-03 08:00:00 UTC'
and TIMESTAMP < '2025-01-04 16:00:00 UTC'
limit 1000
```

## Options Volume, Open Interest, and Greeks Analysis

Returns aggregated options metrics by symbol and tick type for AAPL across all contracts within a specified time range. This aggregation query consolidates volume, open interest, implied volatility, and Greeks across all matching contracts using aggregation functions (SUM for total open interest and volume, AVG for underlying price, implied volatility, and delta, COUNT for total contracts). Results are grouped by `SYMBOL_NAME` and `TICK_TYPE` to separate analysis by contract and data type.

```sql
SELECT SYMBOL_NAME, TICK_TYPE,
       SUM(OPEN_INT) as total_open_interest,
       SUM(VOLUME) as total_volume,
       AVG(UNDERLYING_PRICE) as avg_underlying_price,
       AVG(IMP_VOLATILITY) as avg_implied_vol,
       AVG(DELTA) as avg_delta,
       COUNT(*) as contract_count
FROM US_OPTIONS_EOD_SAMPLE.DAY
WHERE SYMBOL_NAME LIKE 'AAPL%'
    AND TIMESTAMP >= '2025-01-20 00:00:00 UTC'
    AND TIMESTAMP < '2025-01-28 00:00:00 UTC'
GROUP BY SYMBOL_NAME, TICK_TYPE
ORDER BY SYMBOL_NAME, TICK_TYPE
```

## Options Trades

Retrieves options trade data for a specific AAPL contract from the US Options sample feed. Shows tick-level trade records for an individual option contract. The symbol format `AAPL  250103C00155000` represents: Underlying (AAPL), Expiration (2025-01-03), Type (C for Call), and Strike (155.00). This query returns all trade ticks including price, size, venue, and trade conditions for a specific contract.

```sql
SELECT *
FROM US_OPTIONS_SAMPLE.TRD
WHERE SYMBOL_NAME = 'AAPL  250103C00155000'
   AND TIMESTAMP >= '2025-01-02 00:00:00 UTC'
   AND TIMESTAMP < '2025-01-04 16:00:00 UTC'
LIMIT 1000
```

## OPRA Data Sources

OPRA (Options Price Reporting Authority) provides consolidated options pricing and activity data across US options exchanges.
OPRA data includes both intraday trades and end-of-day summaries with comprehensive Greeks and contract metadata.
OPRA options data is available in multiple databases and granularities:


* `US_OPTIONS_SAMPLE` - Real-time trade data for options contracts


* `US_OPTIONS_EOD_SAMPLE` - End-of-day summary data with Greeks and aggregated metrics


* `US_OPTIONS_SAMPLE.TRD` - Individual options trades


* `US_OPTIONS_SAMPLE.STAT` - Options contract metadata (strike, expiration, call/put indicator)


* `US_OPTIONS_EOD_SAMPLE.DAY` - Daily aggregated options data


* `US_OPTIONS_EOD_SAMPLE.STAT` - Static contract information

## OPRA Snapshot - Prevailing Prices at Specified Time

Retrieve the prevailing options prices and contract details at a specific point in time.
This uses a snapshot approach by joining trade data with static contract data using lookback windows
to capture the most recent data available at the query timestamp.

```sql
select
t.PRICE as PRICE,
t.SIZE as SIZE,
s.STRIKE_PRICE as STRIKE_PRICE,
s.EXPIRATION_DATE as EXPIRATION_DATE,
s.CALL_PUT_IND as CALL_PUT_IND,
s.UNDERLYING_SYMBOL as UNDERLYING_SYMBOL
from US_OPTIONS_SAMPLE.TRD t, US_OPTIONS_SAMPLE.STAT s
where t.SYMBOL_NAME LIKE 'AAPL %'
and t.SYMBOL_NAME = s.SYMBOL_NAME
and sametime_as_existing(t.timestamp, s.timestamp, 0) = TRUE
and TIMESTAMP = '2025-01-03 10:00:00 America/New_York'
and s.init_lookback = 86400
and t.init_lookback = 36000
```

## OPRA Snapshot for Specific Expiration

Retrieve all options data for a specific underlying and expiration date at a given time.
Filtered to show a specific contract expiration.

```sql
select
t.PRICE as PRICE,
t.SIZE as SIZE,
s.STRIKE_PRICE as STRIKE_PRICE,
s.EXPIRATION_DATE as EXPIRATION_DATE,
s.CALL_PUT_IND as CALL_PUT_IND,
s.UNDERLYING_SYMBOL as UNDERLYING_SYMBOL
from US_OPTIONS_SAMPLE.TRD t, US_OPTIONS_SAMPLE.STAT s
where t.SYMBOL_NAME LIKE 'AAPL %'
and t.SYMBOL_NAME = s.SYMBOL_NAME
and sametime_as_existing(t.timestamp, s.timestamp, 0) = TRUE
and s.EXPIRATION_DATE = '20250117'
and TIMESTAMP = '2025-01-03 16:00:00 America/New_York'
and s.init_lookback = 86400
and t.init_lookback = 36000
```

## OPRA Daily Volume and Open Interest by Strike

Analyze daily options volume and open interest aggregated by strike price, with separate metrics for calls and puts.
This is useful for understanding the distribution of trading activity across the strike ladder.

```sql
select
UNDERLYING_SYMBOL,
STRIKE_PRICE,
COUNT(*) as CONTRACT_COUNT,
sum(CALL_VOLUME) as CALL_VOLUME,
sum(PUT_VOLUME) as PUT_VOLUME,
sum(VOLUME) as TOTAL_VOLUME,
sum(CALL_OPEN_INT) as CALL_OPEN_INT,
sum(PUT_OPEN_INT) as PUT_OPEN_INT,
sum(OPEN_INT) as TOTAL_OPEN_INT
from (
    select
    case when s.CALL_PUT_IND='P' then d.VOLUME else 0 end as PUT_VOLUME,
    case when s.CALL_PUT_IND='C' then d.VOLUME else 0 end as CALL_VOLUME,
    case when s.CALL_PUT_IND='P' then d.OPEN_INT else 0 end as PUT_OPEN_INT,
    case when s.CALL_PUT_IND='C' then d.OPEN_INT else 0 end as CALL_OPEN_INT,
    d.VOLUME as VOLUME,
    d.OPEN_INT as OPEN_INT,
    s.STRIKE_PRICE as STRIKE_PRICE,
    s.EXPIRATION_DATE as EXPIRATION_DATE,
    s.CALL_PUT_IND as CALL_PUT_IND,
    s.UNDERLYING_SYMBOL as UNDERLYING_SYMBOL
    from US_OPTIONS_EOD_SAMPLE.DAY d, US_OPTIONS_EOD_SAMPLE.STAT s
    where d.SYMBOL_NAME LIKE 'AAPL %'
    and d.SYMBOL_NAME = s.SYMBOL_NAME
    and sametime_as_existing(d.timestamp, s.timestamp, 0) = TRUE
    and TIMESTAMP >= '2025-01-03 00:00:00 America/New_York'
    and TIMESTAMP < '2025-01-04 00:00:00 America/New_York'
    and s.init_lookback = 86400
)
group by UNDERLYING_SYMBOL, STRIKE_PRICE
```

## OPRA Daily Volume and Open Interest by Expiration

Summarize daily options volume and open interest grouped by expiration date, split between calls and puts.
This shows the term structure of trading activity.

```sql
select
UNDERLYING_SYMBOL,
EXPIRATION_DATE,
COUNT(*) as CONTRACT_COUNT,
sum(CALL_VOLUME) as CALL_VOLUME,
sum(PUT_VOLUME) as PUT_VOLUME,
sum(VOLUME) as TOTAL_VOLUME,
sum(CALL_OPEN_INT) as CALL_OPEN_INT,
sum(PUT_OPEN_INT) as PUT_OPEN_INT,
sum(OPEN_INT) as TOTAL_OPEN_INT
from (
    select
    case when s.CALL_PUT_IND='P' then d.VOLUME else 0 end as PUT_VOLUME,
    case when s.CALL_PUT_IND='C' then d.VOLUME else 0 end as CALL_VOLUME,
    case when s.CALL_PUT_IND='P' then d.OPEN_INT else 0 end as PUT_OPEN_INT,
    case when s.CALL_PUT_IND='C' then d.OPEN_INT else 0 end as CALL_OPEN_INT,
    d.VOLUME as VOLUME,
    d.OPEN_INT as OPEN_INT,
    s.STRIKE_PRICE as STRIKE_PRICE,
    s.EXPIRATION_DATE as EXPIRATION_DATE,
    s.CALL_PUT_IND as CALL_PUT_IND,
    s.UNDERLYING_SYMBOL as UNDERLYING_SYMBOL
    from US_OPTIONS_EOD_SAMPLE.DAY d, US_OPTIONS_EOD_SAMPLE.STAT s
    where d.SYMBOL_NAME LIKE 'AAPL %'
    and d.SYMBOL_NAME = s.SYMBOL_NAME
    and sametime_as_existing(d.timestamp, s.timestamp, 0) = TRUE
    and TIMESTAMP >= '2025-01-03 00:00:00 America/New_York'
    and TIMESTAMP < '2025-01-04 00:00:00 America/New_York'
    and s.init_lookback = 86400
)
group by UNDERLYING_SYMBOL, EXPIRATION_DATE
```

## OPRA Intraday Volume by Strike

Calculate intraday trading volume split between calls and puts, grouped by strike price.
This analyzes actual trade flow during market hours for a specific time window.

```sql
select
UNDERLYING_SYMBOL,
STRIKE_PRICE,
COUNT(*) as CONTRACT_COUNT,
sum(CALL_VOLUME) as CALL_VOLUME,
sum(PUT_VOLUME) as PUT_VOLUME,
sum(VOLUME) as TOTAL_VOLUME
from (
    select
    case when s.CALL_PUT_IND='P' then t.SIZE else 0 end as PUT_VOLUME,
    case when s.CALL_PUT_IND='C' then t.SIZE else 0 end as CALL_VOLUME,
    t.SIZE as VOLUME,
    s.STRIKE_PRICE as STRIKE_PRICE,
    s.EXPIRATION_DATE as EXPIRATION_DATE,
    s.CALL_PUT_IND as CALL_PUT_IND,
    s.UNDERLYING_SYMBOL as UNDERLYING_SYMBOL
    from US_OPTIONS_SAMPLE.TRD t, US_OPTIONS_SAMPLE.STAT s
    where t.SYMBOL_NAME LIKE 'AAPL %'
    and t.SYMBOL_NAME = s.SYMBOL_NAME
    and sametime_as_existing(t.timestamp, s.timestamp, 0) = TRUE
    and TIMESTAMP >= '2025-01-03 10:00:00 America/New_York'
    and TIMESTAMP < '2025-01-03 12:00:00 America/New_York'
    and s.init_lookback = 86400
)
group by UNDERLYING_SYMBOL, STRIKE_PRICE
```

## OPRA Intraday Volume by Expiration

Aggregate intraday trade volume by expiration date, showing call/put split for a given time window.

```sql
select
UNDERLYING_SYMBOL,
EXPIRATION_DATE,
COUNT(*) as CONTRACT_COUNT,
sum(CALL_VOLUME) as CALL_VOLUME,
sum(PUT_VOLUME) as PUT_VOLUME,
sum(VOLUME) as TOTAL_VOLUME
from (
    select
    case when s.CALL_PUT_IND='P' then t.SIZE else 0 end as PUT_VOLUME,
    case when s.CALL_PUT_IND='C' then t.SIZE else 0 end as CALL_VOLUME,
    t.SIZE as VOLUME,
    s.STRIKE_PRICE as STRIKE_PRICE,
    s.EXPIRATION_DATE as EXPIRATION_DATE,
    s.CALL_PUT_IND as CALL_PUT_IND,
    s.UNDERLYING_SYMBOL as UNDERLYING_SYMBOL
    from US_OPTIONS_SAMPLE.TRD t, US_OPTIONS_SAMPLE.STAT s
    where t.SYMBOL_NAME LIKE 'AAPL %'
    and t.SYMBOL_NAME = s.SYMBOL_NAME
    and sametime_as_existing(t.timestamp, s.timestamp, 0) = TRUE
    and TIMESTAMP >= '2025-01-03 10:00:00 America/New_York'
    and TIMESTAMP < '2025-01-03 12:00:00 America/New_York'
    and s.init_lookback = 86400
)
group by UNDERLYING_SYMBOL, EXPIRATION_DATE
```

## OPRA Greeks and Pricing by Strike (Straddle View)

Retrieve end-of-day pricing, volume, open interest, and Greeks (IV, Delta, Gamma, Theta, Vega)
organized as a straddle view with calls and puts side-by-side for each strike price.
This format is useful for analyzing strike-level option chains.

```sql
select
avg(C_VOLUME) as C_VOLUME,
avg(C_OPEN_INT) as C_OPEN_INT,
avg(C_BID) as C_BID,
avg(C_ASK) as C_ASK,
avg(C_IV) as C_IV,
avg(C_DELTA) as C_DELTA,
avg(C_GAMMA) as C_GAMMA,
avg(C_THETA) as C_THETA,
avg(C_VEGA) as C_VEGA,
STRIKE_PRICE,
avg(P_VOLUME) as P_VOLUME,
avg(P_OPEN_INT) as P_OPEN_INT,
avg(P_BID) as P_BID,
avg(P_ASK) as P_ASK,
avg(P_IV) as P_IV,
avg(P_DELTA) as P_DELTA,
avg(P_GAMMA) as P_GAMMA,
avg(P_THETA) as P_THETA,
avg(P_VEGA) as P_VEGA
from (
    select
    case when s.CALL_PUT_IND = 'P' then D.VOLUME else NAN() end as P_VOLUME,
    case when s.CALL_PUT_IND = 'P' then D.OPEN_INT else NAN() end as P_OPEN_INT,
    case when s.CALL_PUT_IND = 'P' then D.BID_CLOSE else NAN() end as P_BID,
    case when s.CALL_PUT_IND = 'P' then D.ASK_CLOSE else NAN() end as P_ASK,
    case when s.CALL_PUT_IND = 'P' then D.IMP_VOLATILITY else NAN() end as P_IV,
    case when s.CALL_PUT_IND = 'P' then D.DELTA else NAN() end as P_DELTA,
    case when s.CALL_PUT_IND = 'P' then D.GAMMA else NAN() end as P_GAMMA,
    case when s.CALL_PUT_IND = 'P' then D.THETA else NAN() end as P_THETA,
    case when s.CALL_PUT_IND = 'P' then D.VEGA else NAN() end as P_VEGA,
    case when s.CALL_PUT_IND = 'C' then D.VOLUME else NAN() end as C_VOLUME,
    case when s.CALL_PUT_IND = 'C' then D.OPEN_INT else NAN() end as C_OPEN_INT,
    case when s.CALL_PUT_IND = 'C' then D.BID_CLOSE else NAN() end as C_BID,
    case when s.CALL_PUT_IND = 'C' then D.ASK_CLOSE else NAN() end as C_ASK,
    case when s.CALL_PUT_IND = 'C' then D.IMP_VOLATILITY else NAN() end as C_IV,
    case when s.CALL_PUT_IND = 'C' then D.DELTA else NAN() end as C_DELTA,
    case when s.CALL_PUT_IND = 'C' then D.GAMMA else NAN() end as C_GAMMA,
    case when s.CALL_PUT_IND = 'C' then D.THETA else NAN() end as C_THETA,
    case when s.CALL_PUT_IND = 'C' then D.VEGA else NAN() end as C_VEGA,
    S.UNDERLYING_SYMBOL,
    S.EXPIRATION_DATE,
    S.STRIKE_PRICE as STRIKE_PRICE,
    S.CALL_PUT_IND
    from US_OPTIONS_EOD_SAMPLE.DAY d, US_OPTIONS_EOD_SAMPLE.STAT s
    where d.SYMBOL_NAME LIKE 'AAPL %'
    and d.SYMBOL_NAME = s.SYMBOL_NAME
    and sametime_as_existing(d.timestamp, s.timestamp, 0) = TRUE
    and TIMESTAMP >= '2025-01-03 00:00:00 America/New_York'
    and TIMESTAMP < '2025-01-04 00:00:00 America/New_York'
    and s.EXPIRATION_DATE = '20270115'
    and s.init_lookback = 86400
)
group by STRIKE_PRICE
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
