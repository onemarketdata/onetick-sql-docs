# Futures

This section contains examples of querying futures market data from OneTick Cloud databases. Futures examples demonstrate how to retrieve trade and quote data for individual futures contracts, futures spreads, and aggregated market statistics.

## Futures Symbol Structure

OneTick Cloud uses a hierarchical symbol naming convention for futures:

* **Futures Contracts**: `[Product Code]\[Expiry Month & Year]` - e.g., `CL\N26` for Crude Oil June 2026
* **Futures Spreads**: `[Product Code]\[Expiry Month & Year]\[Expiry Month & Year]` - e.g., `CL\N26\Z26` for a Crude Oil spread between June and December 2026

Wildcards can be used to retrieve multiple contracts:

* `CL\____` - Returns all Crude Oil futures (4 underscores for month+year)
* `CL\\%` - Returns all Crude Oil contracts (futures and spreads)
* `CL________` - Returns all Crude Oil spreads (8 underscores)

## Point in Time Trade Snapshot for Futures Product

Retrieves a snapshot of trade data at a specific point in time for all contracts in a futures product, looking back a specified number of seconds to find the prevailing trade.

Calculates Point in Time Trade Snapshot for Futures Product (Futures Chain). A specific point in time is selected with the TIMESTAMP equal to a specified value. All Futures Symbols are retrieved with SYMBOL_NAME LIKE ‘[Product Code]\\_\_\_\_’. A Lookback is defined in seconds, to check for the prevailing trade before the selected time, upto the lookback period.

```sql
 select *
 from ICE_EU_COM_SAMPLE.TRD
 where SYMBOL_NAME LIKE 'BRN\\____'     -- Retrieve All Futures Symbols for the Futures Product BRN (Brent Crude)
 and TIMESTAMP = '2024-01-03 12:30:00 Europe/London'
 and init_lookback = 86400           --how many seconds to look back for prevailing value
```

## Futures or Spreads Trades for Product

Retrieves all trades for a futures product, including both individual futures contracts and futures spreads trading under that product code.

Return the first 1000 trades for Crude Oil contracts, whether Futures or Spreads trading on NYMEX, with product code CL. Filtering with a SYMBOL_NAME LIKE ‘CL’ for the Product code followed by ‘\\%’ to return all contracts for the selected product code.

```sql
 select * from NYMEX.TRD
 where SYMBOL_NAME like 'CL\\%'     --Product Code followed by back slash and wild card
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 limit 1000
```

## Futures Spreads Trades for Product

Retrieves all trades for futures spreads under a specific product code, filtering for calendar spreads between different contract expirations.

Return the first 1000 trades for Crude Oil Futures Spreads contracts (Futures Spreads Chain) trading on NYMEX, with product code CL. Filtering with a SYMBOL_NAME LIKE ‘CL’ for the Product code followed by eight ‘_’ to select the Futures Spreads symbols that correspond to NYMEX Futures Spreads.

```sql
 select * from NYMEX.TRD
 where SYMBOL_NAME like 'CL________'     --Product Code followed by 8 underscores to represent Futures Spreads
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 limit 1000
```

## Futures Trades for Product

Retrieves trades for individual futures contracts under a specific product code, excluding futures spreads.

Return the first 1000 trades for Crude Oil Futures contracts (Futures Chain) trading on NYMEX, with product code CL. Filtering with a SYMBOL_NAME LIKE ‘CL’ for the Product code followed by four ‘_’ to select the Futures symbols that correspond to NYMEX Futures.

```sql
 select * from NYMEX.TRD
 where SYMBOL_NAME like 'CL____'     --Product Code followed by 4 underscores to represent Futures
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 limit 1000
```

## Futures Volume and Open Interest for Product

Retrieves aggregated daily volume and open interest for all futures contracts under a specific product code.

Return the Volume and Open Interest (OI) for the first 1000 Crude Oil Futures contracts (Futures Chain) trading on NYMEX, with product code CL. UPDATE_TYPE is set to ‘Summary’ to return the final daily combination of both Volume and Open Interest.

```sql
 select * from NYMEX_DAILY.DAY
 where SYMBOL_NAME like 'CL\\___'     --Product Code followed by 4 underscores to represent Futures
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 and UPDATE_TYPE = 'Summary'
 limit 1000
```

## Futures Volume and Open Interest for Product by Expiry

Retrieves aggregated daily volume and open interest for futures contracts, including expiration date information from the symbol universe.

Return the Volume and Open Interest (OI) for the first 1000 Crude Oil Futures contracts (Futures Chain) trading on NYMEX, with product code CL, including expiration date information from the symbol universe.

```sql
 select d.SYMBOL_NAME as SYMBOL_NAME, d.VOLUME as VOLUME, d.OPEN_INT as OPEN_INT, S.EXPIRATION_DATE as EXPIRATION_DATE
  from NYMEX_DAILY.DAY d, NYMEX_DAILY.STAT s
 where d.SYMBOL_NAME like 'CL\\___'     --Product Code followed by 3 underscores to represent Futures
 and d.SYMBOL_NAME = s.SYMBOL_NAME
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 and d.UPDATE_TYPE = 'Summary'
 and s.init_lookback = 86400   -- Lookback 3
 order by EXPIRATION_DATE asc
```
