# Functions - Financial

The following financial functions are supported:

[GET_SYMBOLOGY_MAPPING](),
[CORP_ACTIONS](),
[MKT_ACTIVITY](),
[TICK_SHIFT](),
[TIME_SHIFT]().

## GET_SYMBOLOGY_MAPPING

Translates and returns the symbol in dest_symbology using the current timestamp as the symbol date. The remaining optional parameters are specified to overwrite the symbology of the current symbol, the current symbol, and the timestamp, respectively.

Simple Syntax:
<br/>
`string GET_SYMBOLOGY_MAPPING(string dest_symbology [, string src_symbology, string symbol, datetime timestamp])`
<br/>
```sql
select PRICE, GET_SYMBOLOGY_MAPPING('FGC') as S_GET_SYMBOLOGY_MAPPING
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_SYMBOLOGY_MAPPING Function Example Results

|    | Timestamp                     |   PRICE | SIZE         | S_GET_SYMBOLOGY_MAPPING   |
|----|-------------------------------|---------|--------------|---------------------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 | BBG000C3J3C9 |                           |

## CORP_ACTIONS

Returns the adjusted value of the specified field. Only `field_name` paramter is mandatory.
Optional `adjust_date` string parameter has format YYYYMMDD, and defaults to the end time of the query.

Optional `adjust_rule` string paramater has enumerated values `PRICE` and `SIZE`.
When set to PRICE, adjustments are applied under the assumption that fields to be adjusted contain prices.
When set to SIZE, adjustments are applied under the assumption that fields contain sizes/quantities (adjustment direction is opposite to that when the parameter’s value is PRICE).

Optional `adjustment_types` string parameter is a comma separated list of adjustment types, with a default type of `SPLIT`.
Possible values are `SPLIT`, `SPINOFF`, `CASH_DIVIDEND`, `STOCK_DIVIDEND`, `RIGHTS`, `SECURITY_SPLICE` and `ALL` (meaning to apply all adjustment types).
For specifying custom adjustment types the type should be prepended with `CUSTOM_` prefix.

Simple Syntax:
<br/>
`double CORP_ACTIONS(string field_name [, string adjustment_date, string adjust_rule, string adjustment_types])`
<br/>
```sql
select CLOSE,
CORP_ACTIONS('CLOSE') as ADJ_CLOSE
from US_COMP_SAMPLE_DAILY.DAY
where SYMBOL_NAME = 'WMT' AND EXCHANGE = ''
and TIMESTAMP >= '2024-02-10 00:00:00.000 UTC'
and TIMESTAMP < '2024-03-10 00:00:00.000 UTC'
```

#### CORP_ACTIONS Function Example Results

|    | Timestamp           |   CLOSE |   ADJ_CLOSE |
|----|---------------------|---------|-------------|
|  0 | 2024-02-10 01:15:00 |  169.28 |     56.4267 |
|  1 | 2024-02-13 01:15:00 |  170.3  |     56.7667 |
|  2 | 2024-02-14 01:15:00 |  169.14 |     56.38   |
|  3 | 2024-02-15 01:15:00 |  168.6  |     56.2    |
|  4 | 2024-02-16 01:15:00 |  169.29 |     56.43   |
|  5 | 2024-02-17 01:15:00 |  170.36 |     56.7867 |
|  6 | 2024-02-21 01:15:00 |  175.86 |     58.62   |
|  7 | 2024-02-22 01:15:00 |  173.7  |     57.9    |
|  8 | 2024-02-23 01:15:00 |  175.41 |     58.47   |
|  9 | 2024-02-24 01:15:00 |  175.56 |     58.52   |
| 10 | 2024-02-27 01:15:00 |   59.6  |     59.6    |
| 11 | 2024-02-28 01:15:00 |   59.59 |     59.59   |
| 12 | 2024-02-29 01:15:00 |   59.62 |     59.62   |
| 13 | 2024-03-01 01:15:00 |   58.61 |     58.61   |
| 14 | 2024-03-02 01:15:00 |   58.76 |     58.76   |
| 15 | 2024-03-05 01:15:00 |   59.3  |     59.3    |
| 16 | 2024-03-06 01:15:00 |   60.04 |     60.04   |
| 17 | 2024-03-07 01:15:00 |   60.57 |     60.57   |
| 18 | 2024-03-08 01:15:00 |   60.36 |     60.36   |
| 19 | 2024-03-09 01:15:00 |   60.12 |     60.12   |

## MKT_ACTIVITY

Returns a string with a value for the specified calendar.
The first character of the string represents the day type.

- `L` - Half Day,
- `H` - Holiday,
- `W` - Weekend,
- `R` - Regular

The second letter represents the general timing of trading periods.

- `Rb` - Regular - Pre-Market
- `Rr` - Regular - During Trading
- `Ra` - Regular - Post Market
- `Ra` - Regular - Post Market
- `R1` - Regular - Morning Session
- `Rx` - Regular - Lunch Break
- `R2` - Regular - Afternoon Session

Note: Markets may have specific random auction timings.  Exact timings of trading sessions can be retrieved from the `MKT` table.

Simple Syntax:
<br/>
`string MKT_ACTIVITY(string calendar_name)`
<br/>
```sql
select CLOSE, MKT_ACTIVITY('CLOUD_DB_US_COMP') as ACTIVITY_CODE
from US_COMP_SAMPLE_DAILY.DAY
where
SYMBOL_NAME = 'HD' AND EXCHANGE = ''
and TIMESTAMP >= '2024-01-01 00:00:00.000 UTC'
and TIMESTAMP < '2024-02-01 00:00:00.000 UTC'
limit 10
```

#### CORP_ACTIONS Function Daily Example Results

|    | Timestamp           |   CLOSE | ACTIVITY_CODE   |
|----|---------------------|---------|-----------------|
|  0 | 2024-01-03 01:15:00 |  345.08 | R               |
|  1 | 2024-01-04 01:15:00 |  338.26 | R               |
|  2 | 2024-01-05 01:15:00 |  338.59 | R               |
|  3 | 2024-01-06 01:15:00 |  342.94 | R               |
|  4 | 2024-01-09 01:15:00 |  347.93 | R               |
|  5 | 2024-01-10 01:15:00 |  346.19 | R               |
|  6 | 2024-01-11 01:15:00 |  356.8  | R               |
|  7 | 2024-01-12 01:15:00 |  356.53 | R               |
|  8 | 2024-01-13 01:15:00 |  355.71 | R               |
|  9 | 2024-01-17 01:15:00 |  358.43 | R               |
```sql
select PRICE,SIZE,COND,EXCHANGE,
MKT_ACTIVITY('CLOUD_DB_US_COMP') as MKT_ACTIVITY
from US_COMP_SAMPLE.TRD
where
SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2024-01-03 09:29:30.000 America/New_York'
and TIMESTAMP < '2024-01-03 09:30:00.200  America/New_York'
limit 10
```

#### CORP_ACTIONS Function Intraday Example Results

|    | Timestamp                     |   PRICE |   SIZE | COND   | EXCHANGE   | MKT_ACTIVITY   |
|----|-------------------------------|---------|--------|--------|------------|----------------|
|  0 | 2024-01-03 14:29:38.014214683 |   50.04 |     10 | @ TI   | P          | Rb             |
|  1 | 2024-01-03 14:29:38.570056786 |   50.04 |     89 | @FTI   | P          | Rb             |
|  2 | 2024-01-03 14:29:50.768847900 |   50.06 |      2 | @FTI   | P          | Rb             |
|  3 | 2024-01-03 14:29:59.990898650 |   50.16 |   1995 | @ T    | Q          | Rb             |
|  4 | 2024-01-03 14:29:59.990941169 |   50.16 |    471 | @FT    | Q          | Rb             |
|  5 | 2024-01-03 14:30:00.065443591 |   50.02 |      2 | @  I   | Z          | Rr             |
|  6 | 2024-01-03 14:30:00.111130049 |   50.16 |      3 | @  I   | Z          | Rr             |
|  7 | 2024-01-03 14:30:00.127459523 |   50.17 |    100 | @      | V          | Rr             |
|  8 | 2024-01-03 14:30:00.128498068 |   50.17 |      5 | @F I   | Z          | Rr             |
|  9 | 2024-01-03 14:30:00.135190071 |   50.13 |     46 | @FTI   | Q          | Rr             |

## TICK_SHIFT

Returns the value of the specified field from a tick preceding the current one by offset.
`group_by_field_names` if specified should represent a comma-separated list of field names, in which case offset is counted among the ticks with same values of the specied fields.

Simple Syntax:
<br/>
`field type TICK_SHIFT(string field_name,int offset [,string group_by_field_names])`
<br/>
```sql
select PRICE, SIZE, EXCHANGE,
TICK_SHIFT('PRICE',1) N_TICK_SHIFT,
TICK_SHIFT('PRICE',1,'EXCHANGE') N_TICK_SHIFT_BY_EXCHANGE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

#### CORP_ACTIONS Function Intraday Example Results

|    | Timestamp                     |   PRICE |   SIZE | EXCHANGE   |   N_TICK_SHIFT |   N_TICK_SHIFT_BY_EXCHANGE |
|----|-------------------------------|---------|--------|------------|----------------|----------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |     17 | Q          |                |                            |
|  1 | 2024-01-03 00:00:40.413540299 | 50.57   |     33 | Q          |        50.56   |                    50.56   |
|  2 | 2024-01-03 00:03:34.026455739 | 50.52   |     26 | P          |        50.57   |                            |
|  3 | 2024-01-03 00:03:34.026458843 | 50.51   |    123 | P          |        50.52   |                    50.52   |
|  4 | 2024-01-03 00:03:34.026459421 | 50.49   |      3 | P          |        50.51   |                    50.51   |
|  5 | 2024-01-03 00:05:06.042217019 | 50.4896 |      1 | D          |        50.49   |                            |
|  6 | 2024-01-03 00:07:11.737983252 | 50.4897 |      1 | D          |        50.4896 |                    50.4896 |
|  7 | 2024-01-03 00:12:17.787582635 | 50.49   |      1 | D          |        50.4897 |                    50.4897 |
|  8 | 2024-01-03 00:13:16.630750984 | 50.49   |      3 | P          |        50.49   |                    50.49   |
|  9 | 2024-01-03 00:14:14.187418230 | 50.49   |    100 | P          |        50.49   |                    50.49   |

## TIME_SHIFT

Returns the value of the specified field, from a record, whose timestamp differs from current record’s timestamp by shift time in milliseconds.
If shift time is negative, it returns the value for the preceding record, and if shift time is positive, succeeding ticks (those that have not arrived yet) are considered.

The `selection criteria` optional parameter is a constant string indicating record selection criteria.
Possible values for the criteria parameter are: `LAST_BEFORE`, `LAST_AT_OR_BEFORE`, `FIRST_AFTER`, ``FIRST_AT_OR_AFTER`.
The default value for tick selection criteria is:

- LAST_AT_OR_BEFORE for negative shift times
- FIRST_AT_OR_AFTER for positive shifts

Simple Syntax:
<br/>
`field type TIME_SHIFT(field name,shift time [,selection criteria,timestamp field])`
<br/>
```sql
select PRICE, SIZE, EXCHANGE,
TIME_SHIFT('PRICE',-1000) N_TIME_SHIFT_BACK_1S,
TIME_SHIFT('PRICE',1000) N_TIME_SHIFT_FWD_1S
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

#### TIME_SHIFT Function Intraday Example Results

|    | Timestamp                     |   PRICE |   SIZE | EXCHANGE   |   N_TIME_SHIFT_BACK_1S |   N_TIME_SHIFT_FWD_1S |
|----|-------------------------------|---------|--------|------------|------------------------|-----------------------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |     17 | Q          |                        |               50.52   |
|  1 | 2024-01-03 00:00:40.413540299 | 50.57   |     33 | Q          |                        |               50.52   |
|  2 | 2024-01-03 00:03:34.026455739 | 50.52   |     26 | P          |                50.57   |               50.4896 |
|  3 | 2024-01-03 00:03:34.026458843 | 50.51   |    123 | P          |                50.57   |               50.4896 |
|  4 | 2024-01-03 00:03:34.026459421 | 50.49   |      3 | P          |                50.57   |               50.4896 |
|  5 | 2024-01-03 00:05:06.042217019 | 50.4896 |      1 | D          |                50.49   |               50.4897 |
|  6 | 2024-01-03 00:07:11.737983252 | 50.4897 |      1 | D          |                50.4896 |               50.49   |
|  7 | 2024-01-03 00:12:17.787582635 | 50.49   |      1 | D          |                50.4897 |               50.49   |
|  8 | 2024-01-03 00:13:16.630750984 | 50.49   |      3 | P          |                50.49   |               50.49   |
|  9 | 2024-01-03 00:14:14.187418230 | 50.49   |    100 | P          |                50.49   |               50.49   |
