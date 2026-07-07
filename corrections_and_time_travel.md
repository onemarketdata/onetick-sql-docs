# Corrections and Time Travel

This section contains examples of handling trade corrections, deleted records, and time travel queries in OneTick SQL. These examples demonstrate how to work with the temporal aspects of market data including corrections, reversals, and audit trails.

OneTick maintains a complete audit trail of all trade data including:

* Original trades
* Corrected trades
* Deleted trades
* Metadata about when corrections occurred

The examples in this section show how to query and analyze this temporal data.

## Corrected Trade Retrieval

Demonstrates how to retrieve trades with corrections already applied. Deleted trades will not be visible in the result set, only the corrected versions of trades are returned.

```sql
 select TRADE_ID,PRICE,SIZE,TRADE_TYPE,TRADE_VENUE,TICK_STATUS,DELETED_TIME
 from LSE_SAMPLE.TRD
 where SYMBOL_NAME = 'VOD'
 and TIMESTAMP >= '2024-01-04 11:04:00 UTC'
 and TIMESTAMP < '2024-01-06 00:00:00 UTC'
 limit 10
```

## Trade Corrections

Use `SHOW_CORRECTED_TICKS();TRD` syntax in the FROM clause with database and symbol format `LSE_SAMPLE::VOD`. Corrected trades are identified by their `DELETED_TIME` and `TICK_STATUS` fields (0=Default, 1=Deleted, 2=Updated, 3=Insert Corrected, 4=Canceled, 5=Corrected, 6=New Correction, 7=New cancellation).

```sql
 select TRADE_ID,PRICE,SIZE,TRADE_TYPE,TRADE_VENUE,TICK_STATUS,DELETED_TIME
 from OTQ_CHAIN."SHOW_CORRECTED_TICKS();TRD"
 where SYMBOL_NAME = 'LSE_SAMPLE::VOD'
 and TIMESTAMP >= '2024-01-04 11:04:00 UTC'
 and TIMESTAMP < '2024-01-06 00:00:00 UTC'
 limit 10
```

## Time Travel: Trades Before Corrections Applied

Use `CORRECT_TICK_FILTER(DISCARD_ON_MATCH='FALSE',AS_OF_TIME='<date>')` with `AS_OF_TIME` in YYYYMMDDHHMMSS format (e.g., ‘20240104000000’) to retrieve uncorrected data as it existed before corrections were applied. Include the database and symbol format `LSE_SAMPLE::VOD`.

```sql
 select TRADE_ID,PRICE,SIZE,TRADE_TYPE,TRADE_VENUE,TICK_STATUS,DELETED_TIME
 from OTQ_CHAIN."CORRECT_TICK_FILTER(DISCARD_ON_MATCH='FALSE',AS_OF_TIME='20240104000000');TRD"
 where SYMBOL_NAME = 'LSE_SAMPLE::VOD'
 and TIMESTAMP >= '2024-01-04 11:04:00 UTC'
 and TIMESTAMP < '2024-01-06 00:00:00 UTC'
 limit 10
```

## Time Travel: Trades After Corrections Applied

Set `AS_OF_TIME` to a date after corrections were applied to retrieve the corrected data as it existed at that point in time. Use `CORRECT_TICK_FILTER(DISCARD_ON_MATCH='FALSE',AS_OF_TIME='<date>')` with `AS_OF_TIME` in YYYYMMDDHHMMSS format (e.g., ‘20260106000000’) and database and symbol format `LSE_SAMPLE::VOD`.

```sql
 select TRADE_ID,PRICE,SIZE,TRADE_TYPE,TRADE_VENUE,TICK_STATUS,DELETED_TIME
 from OTQ_CHAIN."CORRECT_TICK_FILTER(DISCARD_ON_MATCH='FALSE',AS_OF_TIME='20260106000000');TRD"
 where SYMBOL_NAME = 'LSE_SAMPLE::VOD'
 and TIMESTAMP >= '2024-01-04 11:04:00 UTC'
 and TIMESTAMP < '2024-01-06 00:00:00 UTC'
 limit 10
```
