# Symbol Universe

A series of simple examples are provided showing how to retrieve symbol metadata and reference data from the Symbol Universe datasets.

## Symbol Universe for Database

Returns static symbol information and security descriptors for all London Stock Exchange instruments. This query retrieves all available symbol attributes from the SYMBOL_UNIVERSE.STAT table including exchange symbols, database symbols, security type, currency, ISIN, SEDOL, CUSIP identifiers, and company information for all LSE-listed securities.

```sql
select * from SYMBOL_UNIVERSE.STAT
where SYMBOL_NAME LIKE 'LSE %'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 100
```

## Symbol Universe for Database and Security Type

Returns static symbol information and security descriptors for London Stock Exchange equity instruments. This query filters by LSE Equity classification to retrieve equity-specific symbol attributes from SYMBOL_UNIVERSE.STAT including exchange symbols, database symbols, instrument identifiers (ISIN, SEDOL, CUSIP), currency, and trading venue information.

```sql
select * from SYMBOL_UNIVERSE.STAT
where SYMBOL_NAME = 'LSE Equity'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 100
```

## Symbol Universe with Mask

Returns symbol mappings and metadata for securities traded in British pence (GBX currency). This query demonstrates symbol universe filtering using multiple criteria to locate specific instruments across multiple databases. Selected fields include NAME (company name), EXCH_SYMBOL (exchange-specific symbol), DB_SYMBOL (database symbol identifier), DB_NAME (database name such as LSE_SAMPLE), CURRENCY (trading currency), and SEC_TYPE (security type). Filters by currency (GBX) and company name pattern matching.

```sql
select NAME, EXCH_SYMBOL, DB_SYMBOL, DB_NAME, CURRENCY, SEC_TYPE from SYMBOL_UNIVERSE.STAT
where SYMBOL_NAME LIKE '%'
and CURRENCY = 'GBX'
and NAME like 'Voda%'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 100
```

## Symbol Universe for Futures Retrieval

Retrieves symbol universe data for all futures contracts under a specific product code, useful for discovering available contract expirations and contract specifications.

Return all futures contracts and spreads under a futures product code, useful for discovering available contract expirations and contract specifications.

```sql
 select * from NYMEX.TRD
 where SYMBOL_NAME like 'CL\\%'     --Product Code followed by back slash and wild card
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 limit 1000
```

## Symbol Universe for Futures Chain Retrieval

Retrieves symbol universe data for individual futures contracts in a product chain, excluding spreads.

Return all individual futures contracts (excluding spreads) under a futures product code, filtering by four underscores to select the Futures symbols that correspond to NYMEX Futures.

```sql
 select * from NYMEX.TRD
 where SYMBOL_NAME like 'CL____'     --Product Code followed by 4 underscores to represent Futures
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 limit 1000
```

## Symbol Universe for NYMEX Futures Retrieval

Retrieves symbol universe data for NYMEX futures contracts, providing access to commodity and energy futures specifications and metadata.

Return all NYMEX futures contracts and spreads, providing access to commodity and energy futures symbol universe and metadata.

```sql
 select * from NYMEX.TRD
 where SYMBOL_NAME like 'CL\\%'     --Product Code followed by back slash and wild card
 and TIMESTAMP >= '2026-06-11 00:00:00 UTC'
 and TIMESTAMP < '2026-06-12 00:00:00 UTC'
 limit 1000
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
