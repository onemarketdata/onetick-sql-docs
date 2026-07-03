# Retrieval with Continuous Contracts

A series of simple examples are provided showing how to retrieve Futures market data by continuous contract.
Continuous Contracts are provided ranging from the Front month to the Twelfth month rolling on expiry.
Additionally continuous contracts are available representing the front month rolling based on Maximum Volume or Maximum Open Interest.

Continuous Contracts are queried with:


* `[Product Code]\\\\1` to `[Product Code]\\\\12`.  - Front Month to Twelfth Month


* `[Product Code]_r_vol` - Front Month based on Maximum Volume


* `[Product Code]_r_oi` - Front Month based on Maximum Open Interest

Continuous Contracts are also queried with the Bloomberg symbology `BSYM`:


* `[Bloomberg Product Code]1` to `[Bloomberg Product Code]12`.  - Front Month to Twelfth Month


* `[Bloomberg Product Code]A` - Front Month based on Maximum Volume

## Retrieval of Futures Contracts

Futures Contracts represent specific expiration months and are identified by their product code and expiration details.

Daily data retrieval requires:


* Database & DAY Table (separated by a period, e.g. `CME_SAMPLE_DAILY.DAY`)


* Symbol using the field `SYMBOL_NAME`, formatted as `[Product Code]\\\\[Expiry Month][Expiry Year]`


* Appropriate filters on `TIMESTAMP` and `symbol_date` for accurate contract identification

Common Futures Product Codes include:


* `ES` - E-mini S&P 500 Futures


* `NQ` - E-mini Nasdaq-100 Futures


* `YM` - E-mini Dow Jones Futures


* `GC` - Gold Futures


* `CL` - Crude Oil Futures


* `NG` - Natural Gas Futures

Expiry Month Codes (for contract specification):


* `F` - January, `G` - February, `H` - March, `J` - April, `K` - May, `M` - June


* `N` - July, `Q` - August, `U` - September, `V` - October, `X` - November, `Z` - December

```sql
select * from CME_SAMPLE_DAILY.DAY
where SYMBOL_NAME='ES\\H24'                 -- E-mini S&P 500 March 2024 Contract (ES\H24)
and TIMESTAMP >= '2024-01-01 00:00:00 UTC'
and TIMESTAMP < '2024-03-15 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20240315                  -- Specifies date for contract lookup
```

```sql
select * from CME_SAMPLE_DAILY.DAY
where SYMBOL_NAME in ('GC\\Z24', 'CL\\Z24')  -- Gold December 2024 and Crude Oil December 2024
and TIMESTAMP >= '2024-06-01 00:00:00 UTC'
and TIMESTAMP < '2024-12-31 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20241231                   -- Specifies date for contract lookup
```

```sql
select * from ICE_EU_COM_SAMPLE_DAILY.DAY
where SYMBOL_NAME='BRN\\Z24'                 -- Returning the December 2024 Brent Oil Futures Contract
and TIMESTAMP >= '2024-01-01 00:00:00 UTC'
and TIMESTAMP < '2024-04-01 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20240401                  -- Specifies date for cont contract lookup
```

## Retrieval of Continuous Contract by Expiry

Daily Data is retrieved by specifying:


* Database & DAY Table (separated by a period.  e.g. `ICE_EU_COM_SAMPLE_DAILY.DAY`).


* Symbol or Symbols using the field `SYMBOL_NAME`, with the continuous contract definiition.

The Symbol represents the Continuous contract with the `[Product Code]\\\\1` to `[Product Code]\\\\12` syntax.
For Brent Crude the Product Code is `BRN`, producing `BRN\\\\1`

```sql
select * from ICE_EU_COM_SAMPLE_DAILY.DAY
where SYMBOL_NAME='BRN\\1'                 -- Front Month Continuous Contract ([Product code]\1)
and TIMESTAMP >= '2024-01-01 00:00:00 UTC'
and TIMESTAMP < '2024-04-01 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20240401                  -- Specifies date for cont contract lookup
```

## Retrieval of Continuous Contract by Max Volume

Daily Data is retrieved by specifying:


* Database & DAY Table (separated by a period.  e.g. `ICE_EU_COM_SAMPLE_DAILY.DAY`).


* Symbol or Symbols using the field `SYMBOL_NAME`, with the continuous contract definiition.

The Symbol represents the Continuous contract with the `[Product Code]_r_vol` syntax.
For Brent Crude the Product Code is `BRN`, producing `BRN_vol`

```sql
select * from ICE_EU_COM_SAMPLE_DAILY.DAY
where SYMBOL_NAME='BRN_r_vol'                -- Highest Open Interest Continuous Contract ([Product]_r_vol)
and TIMESTAMP >= '2024-01-01 00:00:00 UTC'
and TIMESTAMP < '2024-04-01 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20240401                  -- Specifies date for cont contract lookup
```

## Retrieval of Continuous Contract by Max Open Interest

Daily Data is retrieved by specifying:


* Database & DAY Table (separated by a period.  e.g. `ICE_EU_COM_SAMPLE_DAILY.DAY`).


* Symbol or Symbols using the field `SYMBOL_NAME`, with the continuous contract definiition.

The Symbol represents the Continuous contract with the `[Product Code]_r_oi` syntax.
For Brent Crude the Product Code is `BRN`, producing `BRN_r_OI`

```sql
select * from ICE_EU_COM_SAMPLE_DAILY.DAY
where SYMBOL_NAME='BRN_r_oi'                -- Highest Open Interest Continuous Contract ([Product]_r_oi)
and TIMESTAMP >= '2024-01-01 00:00:00 UTC'
and TIMESTAMP < '2024-04-01 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20240401                  -- Specifies date for cont contract lookup
```

## Futures Contract Bloomberg Symbol Retrieval

Furures Contracts can be retrieved by Bloomberg Symbol with:


* Prefixing the Database name with `BSYM::` - Setting the `BSYM` symbology


* Specifying the `SYMBOL_NAME` as the full Bloomberg Symbol

```sql
SELECT * FROM BSYM::ICE_EU_COM_DAILY.DAY
WHERE SYMBOL_NAME='COZ4 Comdty'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and SYMBOL_DATE = 20240104
LIMIT 10
```

## Retrieval of Bloomberg Continuous Contract by Expiry

Daily Data is retrieved by specifying:


* Symbology `BSYM`, Database & DAY Table (separated by a period.  e.g. `BSYM::ICE_EU_COM_SAMPLE_DAILY.DAY`).


* Symbol or Symbols using the field `SYMBOL_NAME`, with the Bloomberg continuous contract definiition.

The Symbol represents the Continuous contract with the `[Bloomberg Product Code]1` to `[Bloomberg Product Code]12` syntax.

```sql
select * from BSYM::ICE_EU_COM_SAMPLE_DAILY.DAY
where SYMBOL_NAME='CO1'                 -- Front Month Continuous Contract ([Bloomberg Product code]1)
and TIMESTAMP >= '2024-01-01 00:00:00 UTC'
and TIMESTAMP < '2024-04-01 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20240401                 -- Specifies date for cont contract lookup
```

## Retrieval of Bloomberg Continuous Contract by Max Volume

Daily Data is retrieved by specifying:


* Symbology `BSYM`, Database & DAY Table (separated by a period.  e.g. `BSYM::ICE_EU_COM_SAMPLE_DAILY.DAY`).


* Symbol or Symbols using the field `SYMBOL_NAME`, with the continuous contract definiition.

The Symbol represents the Continuous contract with the `[Product Code]A` syntax.
For Brent Crude the Product Code is `BRN`, producing `COA`

```sql
select * from BSYM::ICE_EU_COM_SAMPLE_DAILY.DAY
where SYMBOL_NAME='COA'                -- Highest Open Interest Continuous Contract ([Product]_r_vol)
and TIMESTAMP >= '2024-01-01 00:00:00 UTC'
and TIMESTAMP < '2024-04-01 00:00:00 UTC'
and UPDATE_TYPE = 'Summary'
and symbol_date = 20240401                  -- Specifies date for cont contract lookup
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
