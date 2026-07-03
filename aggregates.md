# Aggregates

The following aggregations are supported:

AT_HIGH,
AT_LOW,
AVG,
CORR,
COUNT,
EXP_TW_AVERAGE,
EXP_W_AVERAGE,
FIRST,
FIRST_TIME,
HIGH_TIME,
LAST,
LAST_TIME,
LOW_TIME,
MAX,
MEDIAN,
MIN,
PERCENTILE_CONT,
PERCENTILE_DISC,
STANDARDIZED_MOMENT - KURTOSIS,
STANDARDIZED_MOMENT - SKEWNESS,
STDDEV,
STDDEVP,
SUM,
TW_AVG,
VAR,
VARP,
VWAP.

## AT_HIGH

Returns the target field value at the first high of another fields set of values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`AT_HIGH([High Field Name],[Target Field Name])`Window Syntax:`AT_HIGH([High Field Name,[Target Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`AT_HIGH([High Field Name],[Target Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select AT_HIGH(PRICE,SIZE) as SIZE_AT_HIGH_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, AT_HIGH(PRICE,SIZE) as SIZE_AT_HIGH_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
select AT_HIGH(PRICE,SIZE) OVER(order by TIMESTAMP asc) as SIZE_AT_HIGH_ROLLING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select AT_HIGH(PRICE,SIZE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as SIZE_AT_HIGH_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## AT_LOW

Returns the target field value at the first Low of another fields set of values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`AT_LOW([Low Field Name],[Target Field Name])`Window Syntax:`AT_LOW([Low Field Name,[Target Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`AT_LOW([Low Field Name],[Target Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select AT_LOW(PRICE,SIZE) as SIZE_AT_LOW_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, AT_LOW(PRICE,SIZE) as SIZE_AT_LOW_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
select AT_LOW(PRICE,SIZE) OVER(order by TIMESTAMP asc) as SIZE_AT_LOW_ROLLING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select AT_LOW(PRICE,SIZE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as SIZE_AT_LOW_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## AVG

Returns the average of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`AVG([Field Name])`Window Syntax:`AVG([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`AVG([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select AVG(PRICE) as AVG_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, AVG(PRICE) as AVG_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT AVG(PRICE) OVER(order by TIMESTAMP asc) as AVG_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select AVG(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as AVG_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## CORR

Returns the correlation of non-NaN pairs of values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`CORR([Field Name 1],[Field Name 2])`Window Syntax:`CORR([Field Name 1],[Field Name 2]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`CORR([Field Name 1],[Field Name 2]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select CORRELATION(PRICE,SIZE) as CORR_PAIR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, CORRELATION(PRICE,SIZE) as CORR_PAIR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
select CORRELATION(PRICE,SIZE) OVER(order by TIMESTAMP asc) as CORR_ROLLING_PAIR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select CORRELATION(PRICE,SIZE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as CORR_MOVING_PAIR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## COUNT

Returns the count of values.

Simple Syntax:`COUNT([Field Name])` or `COUNT(\*)`Window Syntax:`COUNT([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`COUNT([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select COUNT(PRICE) as COUNT_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, COUNT(PRICE) as COUNT_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT COUNT(PRICE) OVER(order by TIMESTAMP asc) as COUNT_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select COUNT(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as COUNT_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## EXP_W_AVERAGE

Returns the Exponential Weighted Average of a set of values.
It expects the field to aggregate upon, plus the `DECAY` value, which has a decay value type of Lambda.

Simple Syntax:`EXP_W_AVERAGE([Field Name],DECAY=[Lambda Value])`Window Syntax:`EXP_W_AVERAGE([Field Name],DECAY=[Lambda Value]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`EXP_W_AVERAGE([Field Name],DECAY=[Lambda Value]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select EXP_W_AVERAGE(PRICE,DECAY=0.1) as EXP_W_AVG_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, EXP_W_AVERAGE(PRICE,DECAY=0.1) as EXP_W_AVG_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT EXP_W_AVERAGE(PRICE,DECAY=0.1) OVER(order by TIMESTAMP asc) as EXP_W_AVG_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXP_W_AVERAGE(PRICE,DECAY=0.1) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as EXP_W_AVG_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## EXP_TW_AVERAGE

Returns the Exponential Time Weighted Average of a set of values.
It expects the field to aggregate upon, plus the `DECAY` value, which has a decay value type of half life in seconds.

Simple Syntax:`EXP_TW_AVERAGE([Field Name],DECAY=[Half life in seconds])`Window Syntax:`EXP_TW_AVERAGE([Field Name],DECAY=[Half life in seconds]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`EXP_TW_AVERAGE([Field Name],DECAY=[Half life in seconds]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select EXP_TW_AVERAGE(PRICE,DECAY=0.1) as EXP_W_AVG_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, EXP_TW_AVERAGE(PRICE,DECAY=0.1) as EXP_TW_AVERAGE_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT EXP_W_AVERAGE(PRICE,DECAY=0.1) OVER(order by TIMESTAMP asc) as EXP_TW_AVERAGE_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXP_TW_AVERAGE(PRICE,DECAY=0.1) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as EXP_TW_AVERAGE_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## FIRST

Returns the First value of a set of values

Simple Syntax:`FIRST([Field Name])`Window Syntax:`FIRST([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`FIRST([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select FIRST(PRICE) as FIRST_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, FIRST(PRICE) as FIRST_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT FIRST(PRICE) OVER(order by TIMESTAMP asc) as FIRST_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select FIRST(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as FIRST_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## FIRST_TIME

Returns the Timestamp of the first value of a set of values

Simple Syntax:`FIRST_TIME([Field Name])` or `FIRST_TIME(\*)`Window Syntax:`FIRST_TIME([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`FIRST_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select FIRST_TIME(PRICE) as FIRST_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, FIRST_TIME(PRICE) as FIRST_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT FIRST_TIME(PRICE) OVER(order by TIMESTAMP asc) as FIRST_TIME_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select FIRST_TIME(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as FIRST_TIME_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## HIGH_TIME

Returns the First Timestamp of the maximum value of a set of values

Simple Syntax:`HIGH_TIME([Field Name])`Window Syntax:`HIGH_TIME([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`HIGH_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select HIGH_TIME(PRICE) as HIGH_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, HIGH_TIME(PRICE) as HIGH_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT HIGH_TIME(PRICE) OVER(order by TIMESTAMP asc) as HIGH_TIME_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select HIGH_TIME(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as HIGH_TIME_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## LAST

Returns the Last value of a set of values

Simple Syntax:`LAST([Field Name])`Window Syntax:`LAST([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`LAST([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select LAST(PRICE) as LAST_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, LAST(PRICE) as LAST_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT LAST(PRICE) OVER(order by TIMESTAMP asc) as LAST_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select LAST(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as LAST_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## LAST_TIME

Returns the Timestamp of the last value of a set of values

Simple Syntax:`LAST_TIME([Field Name])` or `LAST_TIME(\*)`Window Syntax:`LAST_TIME([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`LAST_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select LAST_TIME(PRICE) as LAST_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, LAST_TIME(PRICE) as LAST_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT LAST_TIME(PRICE) OVER(order by TIMESTAMP asc) as LAST_TIME_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select LAST_TIME(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as LAST_TIME_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## LOW_TIME

Returns the First Timestamp of the minimum value of a set of values

Simple Syntax:`LOW_TIME([Field Name])`Window Syntax:`LOW_TIME([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`LOW_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select LOW_TIME(PRICE) as LOW_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, LOW_TIME(PRICE) as LOW_TIME_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT LOW_TIME(PRICE) OVER(order by TIMESTAMP asc) as LOW_TIME_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select LOW_TIME(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as LOW_TIME_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## MAX

Returns the maximum of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`MAX([Field Name])`Window Syntax:`MAX([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`MAX([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select MAX(PRICE) as MAX_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, MAX(PRICE) as MAX_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT MAX(PRICE) OVER(order by TIMESTAMP asc) as MAX_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select MAX(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as MAX_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## MEDIAN

Returns the Median of a set of values.

Simple Syntax:`MEDIAN([Field Name])`Window Syntax:`MEDIAN([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`MEDIAN([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select MEDIAN(PRICE) as MEDIAN_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, MEDIAN(PRICE) as MEDIAN_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT MEDIAN(PRICE) OVER(order by TIMESTAMP asc) as MEDIAN_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select MEDIAN(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as MEDIAN_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## MIN

Returns the minimum of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`MIN([Field Name])`Window Syntax:`MIN([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`MIN([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select MIN(PRICE) as MIN_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, MIN(PRICE) as MIN_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT MIN(PRICE) OVER(order by TIMESTAMP asc) as MIN_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select MIN(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as MIN_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## PERCENTILE_CONT

Returns the percentile of non-NaN values based on a continuous distribution.  If all values inside the group are NaN the aggregate returns NaN.
The Percentile is written as a decimal.  e.g. 0.9 = 90%, 0.5 = 50%, 0.1 = 10%

Simple Syntax:`PERCENTILE_CONT([Percentile]) WITHIN GROUP (ORDER BY [Field Name] asc)````sql
select PERCENTILE_CONT(0.9) WITHIN GROUP (ORDER BY PRICE asc) as P90C
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## PERCENTILE_DISC

Returns the percentile of non-NaN values based on a discrete distribution.  If all values inside the group are NaN the aggregate returns NaN.
The Percentile is written as a decimal.  e.g. 0.9 = 90%, 0.5 = 50%, 0.1 = 10%

Simple Syntax:`PERCENTILE_DISC([Percentile]) WITHIN GROUP (ORDER BY [Field Name] asc)````sql
select PERCENTILE_DISC(0.9) WITHIN GROUP (ORDER BY PRICE asc) as P90C
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## STANDARDIZED_MOMENT - SKEWNESS

Returns the Skewness of a set of values. using the `STANDARDIZED_MOMENT` aggregate.
It expects the field to aggregate upon, plus the `degree" value.   \`\`3` for Skewness.

Simple Syntax:`STANDARDIZED_MOMENT([Field Name],degree=3)`Window Syntax:`STANDARDIZED_MOMENT([Field Name],degree=3) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`STANDARDIZED_MOMENT([Field Name],degree=3) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select STANDARDIZED_MOMENT(PRICE,degree=3) as SKEWNESS_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, STANDARDIZED_MOMENT(PRICE,degree=3) as SKEWNESS_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT STANDARDIZED_MOMENT(PRICE,degree=3) OVER(order by TIMESTAMP asc) as SKEWNESS_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select STANDARDIZED_MOMENT(PRICE,degree=3) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as SKEWNESS_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## STANDARDIZED_MOMENT - KURTOSIS

Returns the Kurtosis of a set of values, using the `STANDARDIZED_MOMENT` aggregate.
It expects the field to aggregate upon, plus the `degree` value.   `4` for Kurtosis.

Simple Syntax:`STANDARDIZED_MOMENT([Field Name],degree=4)`Window Syntax:`STANDARDIZED_MOMENT([Field Name],degree=4) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`STANDARDIZED_MOMENT([Field Name],degree=4) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select STANDARDIZED_MOMENT(PRICE,degree=4) as KURTOSIS_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, STANDARDIZED_MOMENT(PRICE,degree=4) as KURTOSIS_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT STANDARDIZED_MOMENT(PRICE,degree=4) OVER(order by TIMESTAMP asc) as KURTOSIS_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select STANDARDIZED_MOMENT(PRICE,degree=4) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as KURTOSIS_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## STDDEV

Returns the Sample Standard Deviation of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`STDDEV([Field Name])`Window Syntax:`STDDEV([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`STDDEV([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select STDDEV(PRICE) as STDDEV_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, STDDEV(PRICE) as STDDEV_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT STDDEV(PRICE) OVER(order by TIMESTAMP asc) as STDDEV_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select STDDEV(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as STDDEV_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## STDDEVP

Returns the Population Standard Deviation of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`STDDEVP([Field Name])`Window Syntax:`STDDEVP([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`STDDEVP([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select STDDEVP(PRICE) as STDDEVP_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, STDDEVP(PRICE) as STDDEVP_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT STDDEVP(PRICE) OVER(order by TIMESTAMP asc) as STDDEVP_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select STDDEVP(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as STDDEVP_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## SUM

Returns the sum of non-NaN values. If all values inside the group are NaN the aggregate returns 0.

Simple Syntax:`SUM([Field Name])`Window Syntax:`SUM([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`SUM([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select SUM(SIZE) as SUM_SIZE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, SUM(SIZE) as SUM_SIZE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
select SUM(SIZE) OVER(order by TIMESTAMP asc) as SUM_ROLLING_SIZE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select SUM(SIZE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as SUM_MOVING_SIZE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## TW_AVG

Returns the Time Weighted Average (TWAP) of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`TW_AVG([Field Name])`Window Syntax:`TW_AVG([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`TW_AVG([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select TW_AVG(PRICE) as TW_AVG_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, TW_AVG(PRICE) as TW_AVG_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT TW_AVG(PRICE) OVER(order by TIMESTAMP asc) as TW_AVG_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select TW_AVG(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as TW_AVG_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## VAR

Returns the Sample Variance of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`VAR([Field Name])`Window Syntax:`VAR([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`VAR([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select VAR(PRICE) as VAR_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, VAR(PRICE) as VAR_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT VAR(PRICE) OVER(order by TIMESTAMP asc) as VAR_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select VAR(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as VAR_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## VARP

Returns the Population Variance of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:`VARP([Field Name])`Window Syntax:`VARP([Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`VARP([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select VARP(PRICE) as VARP_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, VARP(PRICE) as VARP_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT VARP(PRICE) OVER(order by TIMESTAMP asc) as VARP_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select VARP(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as VARP_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

## VWAP

Returns the Weighted Average, typically VWAP of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.
It can also be calculated using the Product of the two fields, divided by the sum of the two fields. e,g.  `SUM(PRICE\*SIZE)/SUM(SIZE)`

Simple Syntax:`VWAP(price_field_name=[Field Name],size_field_name=[Weight Field Name])`Alternative Syntax:`SUM([Field Name]\*[Weight Field Name])/SUM([Weight Field Name])`Window Syntax:`VWAP(price_field_name=[Field Name],size_field_name=[Weight Field Name]) OVER(order by TIMESTAMP asc)`Moving Window Syntax:`VWAP(price_field_name=[Field Name],size_field_name=[Weight Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)````sql
select VWAP(price_field_name=PRICE,size_field_name=SIZE) as VWAP_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select EXCHANGE, VWAP(price_field_name=PRICE,size_field_name=SIZE) as VWAP_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
group by EXCHANGE
```

```sql
SELECT VWAP(price_field_name=PRICE,size_field_name=SIZE) OVER(order by TIMESTAMP asc) as VWAP_ROLLING_PRICE
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

```sql
select VWAP(price_field_name=PRICE,size_field_name=SIZE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as VWAP_MOVING_PRICE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
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
