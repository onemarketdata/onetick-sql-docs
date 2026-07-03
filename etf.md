# Exchange-Traded Fund (ETF) Data Retrieval

A series of simple examples are provided showing how to retrieve Exchange-Traded Fund (ETF) data, including universe information, daily metrics, constituents, cash positions, and portfolio composition.
ETF data is queried using Bloomberg symbology (`BSYM`) to access the `OQD_ETF` database.

## ETF Tables

The OQD_ETF database provides data through the following tables:


* `OQD_ETF.DES` - ETF descriptive information and metadata


* `OQD_ETF.DAY` - Daily ETF price and performance metrics


* `OQD_ETF.CONSTITUENTS` - Constituent holdings and weights


* `OQD_ETF.CASH` - Cash and cash equivalents held in the portfolio


* `OQD_ETF.PCF` - Portfolio composition and holding details

## Retrieval of ETF Universe

ETF descriptive information and metadata can be retrieved for all available ETFs.
This query returns comprehensive information about each ETF using Bloomberg symbology.

```sql
select * from BSYM::OQD_ETF.DES
where SYMBOL_NAME LIKE '%'
and TIMESTAMP >= '2024-08-08 00:00:00 UTC'
and TIMESTAMP < '2024-08-09 00:00:00 UTC'
LIMIT 100
```

## Retrieval of ETF Daily Data

Daily ETF price and performance metrics can be retrieved for a specific ETF.
This includes closing prices, performance data, and other daily statistics.

```sql
select * from BSYM::OQD_ETF.DAY
where SYMBOL_NAME = 'SPY US Equity'
and TIMESTAMP >= '2024-07-08 00:00:00 UTC'
and TIMESTAMP < '2024-08-09 00:00:00 UTC'
LIMIT 100
```

## Retrieval of ETF Constituents

The constituent holdings and their weights within an ETF can be retrieved.
This shows the individual securities that make up the ETF portfolio.

```sql
select * from BSYM::OQD_ETF.CONSTITUENTS
where SYMBOL_NAME = 'SPY US Equity'
and TIMESTAMP >= '2024-08-08 00:00:00 UTC'
and TIMESTAMP < '2024-08-09 00:00:00 UTC'
LIMIT 100
```

## Retrieval of ETF Cash Positions

Cash and cash equivalent holdings within an ETF portfolio can be retrieved.
This shows the liquid cash reserves held by the fund.

```sql
select * from BSYM::OQD_ETF.CASH
where SYMBOL_NAME = 'SPY US Equity'
and TIMESTAMP >= '2024-07-08 00:00:00 UTC'
and TIMESTAMP < '2024-08-09 00:00:00 UTC'
LIMIT 100
```

## Retrieval of ETF Portfolio Composition

Portfolio composition and holding details for an ETF can be retrieved.
This provides comprehensive information about the fund’s portfolio structure and composition.

```sql
select * from BSYM::OQD_ETF.PCF
where SYMBOL_NAME = 'SPY US Equity'
and TIMESTAMP >= '2024-08-08 00:00:00 UTC'
and TIMESTAMP < '2024-08-09 00:00:00 UTC'
LIMIT 100
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
