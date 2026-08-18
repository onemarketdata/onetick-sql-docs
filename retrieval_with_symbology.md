<a id="retrieval-with-symbology"></a>

# Retrieval with Symbology

A series of simple examples are provided showing how to retrieve data with a specified symbology.

<a id="standard-retrieval"></a>

## Standard Retrieval

Data is generally retrieved by specifying

* Database & Table (separated by a period.  e.g. `LSE_SAMPLE.TRD`).
* Symbol or Symbols using the field `SYMBOL_NAME`
* Time Range using field `TIMESTAMP`
* Limiting the volume of returned records with `LIMIT`

Where the Symbol typically represents the exchange ticker symbol for the instrument.
e.g. AAPL for Apple Inc.

```sql
SELECT * FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='AAPL'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

<a id="bloomberg-symbol-retrieval"></a>

## Bloomberg Symbol Retrieval

Symbols can be retrieved by Bloomberg Symbol with:

* Prefixing the Database name with `BSYM::`
* Specifying the `SYMBOL_NAME` as the full Bloomberg Symbol
* Specifying the Date when the symbol is active (as symbols can change across time), using `SYMBOL_DATE`

```sql
SELECT * FROM BSYM::US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='AAPL US Equity'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and SYMBOL_DATE = 20240104
LIMIT 10
```

<a id="figi-composite-symbol-retrieval"></a>

## FIGI Composite Symbol Retrieval

Symbols can be retrieved by FGI Composite with:

* Prefixing the Database name with `FGC::`
* Specifying the `SYMBOL_NAME` as the FIGI Symbol
* Specifying the Date when the symbol is active (as symbols can change across time), using `SYMBOL_DATE`

```sql
SELECT * FROM FGC::US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='BBG000B9XRY4'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and SYMBOL_DATE = 20240104
LIMIT 10
```

<a id="cusip-symbol-retrieval"></a>

## CUSIP Symbol Retrieval

Symbols can be retrieved by CUSIP with:

* Prefixing the Database name with `CUS::`
* Specifying the `SYMBOL_NAME` as the CUSIP
* Specifying the Date when the symbol is active (as symbols can change across time), using `SYMBOL_DATE`

```sql
SELECT * FROM CUS::US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='037833100'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and SYMBOL_DATE = 20240104
LIMIT 10
```

<a id="isin-symbol-retrieval"></a>

## ISIN Symbol Retrieval

> Symbols can be retrieved by ISIN with:
* Prefixing the Database name with `ISN::`
* Specifying the `SYMBOL_NAME` as the ISIN
* Specifying the Date when the symbol is active (as symbols can change across time), using `SYMBOL_DATE`

```sql
SELECT * FROM ISN::LSE_SAMPLE.TRD
WHERE SYMBOL_NAME='GB00BH4HKS39'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and SYMBOL_DATE = 20240104
LIMIT 10
```

<a id="sedol-symbol-retrieval"></a>

## SEDOL Symbol Retrieval

Symbols can be retrieved by SEDOL with:

* Prefixing the Database name with `SED::`
* Specifying the `SYMBOL_NAME` as the SEDOL
* Specifying the Date when the symbol is active (as symbols can change across time), using `SYMBOL_DATE`

```sql
SELECT * FROM SED::LSE_SAMPLE.TRD
WHERE SYMBOL_NAME='BH4HKS3'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and SYMBOL_DATE = 20240104
LIMIT 10
```

<a id="reallocated-symbol-retrieval"></a>

## Reallocated Symbol Retrieval

Symbols can be reallocated to a different instrument across a relatively small time period.
For example, `SPCX` has represented two instruments in 2026:

* January 2026 to mid June 2026 - the SPAC and New Issue ETF
* Mid June 2026 onwards - SpaceX

A simple retrieval by symbol combines the history across both instruments.

```sql
select * from US_COMP_DAILY.DAY
where SYMBOL_NAME = 'SPCX'
and TIMESTAMP >= '2026-01-01 00:00:00 UTC'
and TIMESTAMP < '2026-07-01 00:00:00 UTC'
and EXCHANGE = ''
limit 1000
```

<a id="reallocated-symbol-retrieval-specifying-the-etf"></a>

## Reallocated Symbol Retrieval, specifying the ETF

Specifying the `SYMBOL_DATE` as a January date, when the ETF was active, ensures just the ETF history is retrieved.

```sql
select * from US_COMP_DAILY.DAY
where SYMBOL_NAME = 'SPCX'
and TIMESTAMP >= '2026-01-01 00:00:00 UTC'
and TIMESTAMP < '2026-07-01 00:00:00 UTC'
and EXCHANGE = ''
and SYMBOL_DATE = 20260101
limit 1000
```

<a id="reallocated-symbol-retrieval-specifying-the-latest-instrument"></a>

## Reallocated Symbol Retrieval, specifying the Latest Instrument

Specifying the `SYMBOL_DATE` as a July date, when SpaceX is active, ensures just the SpaceX history is retrieved.

```sql
select * from US_COMP_DAILY.DAY
where SYMBOL_NAME = 'SPCX'
and TIMESTAMP >= '2026-01-01 00:00:00 UTC'
and TIMESTAMP < '2026-07-01 00:00:00 UTC'
and EXCHANGE = ''
and SYMBOL_DATE = 20260701
limit 1000
```
