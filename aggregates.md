<a id="aggregates"></a>

# Aggregates

The following aggregations are supported:

[AT_HIGH](#at-high),
[AT_LOW](#at-low),
[AVG](#avg),
[CORR](#corr),
[COUNT](#count),
[EXP_TW_AVERAGE](#exp-tw-average),
[EXP_W_AVERAGE](#exp-w-average),
[FIRST](#first),
[FIRST_TIME](#first-time),
[HIGH_TIME](#high-time),
[LAST](#last),
[LAST_TIME](#last-time),
[LOW_TIME](#low-time),
[MAX](#max),
[MEDIAN](#median),
[MIN](#min),
[PERCENTILE_CONT](#percentile-cont),
[PERCENTILE_DISC](#percentile-disc),
[STANDARDIZED_MOMENT - KURTOSIS](#standardized-moment-kurtosis),
[STANDARDIZED_MOMENT - SKEWNESS](#standardized-moment-skewness),
[STDDEV](#stddev),
[STDDEVP](#stddevp),
[SUM](#sum),
[TW_AVG](#tw-avg),
[VAR](#var),
[VARP](#varp),
[VWAP](#vwap).

<a id="at-high"></a>

## AT_HIGH

Returns the target field value at the first high of another fields set of values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`AT_HIGH([High Field Name],[Target Field Name])`
<br/>
Window Syntax:
<br/>
`AT_HIGH([High Field Name,[Target Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`AT_HIGH([High Field Name],[Target Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="at-low"></a>

## AT_LOW

Returns the target field value at the first Low of another fields set of values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`AT_LOW([Low Field Name],[Target Field Name])`
<br/>
Window Syntax:
<br/>
`AT_LOW([Low Field Name,[Target Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`AT_LOW([Low Field Name],[Target Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="avg"></a>

## AVG

Returns the average of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`AVG([Field Name])`
<br/>
Window Syntax:
<br/>
`AVG([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`AVG([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="corr"></a>

## CORR

Returns the correlation of non-NaN pairs of values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`CORR([Field Name 1],[Field Name 2])`
<br/>
Window Syntax:
<br/>
`CORR([Field Name 1],[Field Name 2]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`CORR([Field Name 1],[Field Name 2]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="count"></a>

## COUNT

Returns the count of values.

Simple Syntax:
<br/>
`COUNT([Field Name])` or `COUNT(*)`
<br/>
Window Syntax:
<br/>
`COUNT([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`COUNT([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="exp-w-average"></a>

## EXP_W_AVERAGE

Returns the Exponential Weighted Average of a set of values.
It expects the field to aggregate upon, plus the `DECAY` value, which has a decay value type of Lambda.

Simple Syntax:
<br/>
`EXP_W_AVERAGE([Field Name],DECAY=[Lambda Value])`
<br/>
Window Syntax:
<br/>
`EXP_W_AVERAGE([Field Name],DECAY=[Lambda Value]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`EXP_W_AVERAGE([Field Name],DECAY=[Lambda Value]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="exp-tw-average"></a>

## EXP_TW_AVERAGE

Returns the Exponential Time Weighted Average of a set of values.
It expects the field to aggregate upon, plus the `DECAY` value, which has a decay value type of half life in seconds.

Simple Syntax:
<br/>
`EXP_TW_AVERAGE([Field Name],DECAY=[Half life in seconds])`
<br/>
Window Syntax:
<br/>
`EXP_TW_AVERAGE([Field Name],DECAY=[Half life in seconds]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`EXP_TW_AVERAGE([Field Name],DECAY=[Half life in seconds]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="first"></a>

## FIRST

Returns the First value of a set of values

Simple Syntax:
<br/>
`FIRST([Field Name])`
<br/>
Window Syntax:
<br/>
`FIRST([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`FIRST([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="first-time"></a>

## FIRST_TIME

Returns the Timestamp of the first value of a set of values

Simple Syntax:
<br/>
`FIRST_TIME([Field Name])` or `FIRST_TIME(*)`
<br/>
Window Syntax:
<br/>
`FIRST_TIME([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`FIRST_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="high-time"></a>

## HIGH_TIME

Returns the First Timestamp of the maximum value of a set of values

Simple Syntax:
<br/>
`HIGH_TIME([Field Name])`
<br/>
Window Syntax:
<br/>
`HIGH_TIME([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`HIGH_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="last"></a>

## LAST

Returns the Last value of a set of values

Simple Syntax:
<br/>
`LAST([Field Name])`
<br/>
Window Syntax:
<br/>
`LAST([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`LAST([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="last-time"></a>

## LAST_TIME

Returns the Timestamp of the last value of a set of values

Simple Syntax:
<br/>
`LAST_TIME([Field Name])` or `LAST_TIME(*)`
<br/>
Window Syntax:
<br/>
`LAST_TIME([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`LAST_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="low-time"></a>

## LOW_TIME

Returns the First Timestamp of the minimum value of a set of values

Simple Syntax:
<br/>
`LOW_TIME([Field Name])`
<br/>
Window Syntax:
<br/>
`LOW_TIME([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`LOW_TIME([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="max"></a>

## MAX

Returns the maximum of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`MAX([Field Name])`
<br/>
Window Syntax:
<br/>
`MAX([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`MAX([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="median"></a>

## MEDIAN

Returns the Median of a set of values.

Simple Syntax:
<br/>
`MEDIAN([Field Name])`
<br/>
Window Syntax:
<br/>
`MEDIAN([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`MEDIAN([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="min"></a>

## MIN

Returns the minimum of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`MIN([Field Name])`
<br/>
Window Syntax:
<br/>
`MIN([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`MIN([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="percentile-cont"></a>

## PERCENTILE_CONT

Returns the percentile of non-NaN values based on a continuous distribution.  If all values inside the group are NaN the aggregate returns NaN.
The Percentile is written as a decimal.  e.g. 0.9 = 90%, 0.5 = 50%, 0.1 = 10%

Simple Syntax:
<br/>
`PERCENTILE_CONT([Percentile]) WITHIN GROUP (ORDER BY [Field Name] asc)`
<br/>
```sql
select PERCENTILE_CONT(0.9) WITHIN GROUP (ORDER BY PRICE asc) as P90C
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

<a id="percentile-disc"></a>

## PERCENTILE_DISC

Returns the percentile of non-NaN values based on a discrete distribution.  If all values inside the group are NaN the aggregate returns NaN.
The Percentile is written as a decimal.  e.g. 0.9 = 90%, 0.5 = 50%, 0.1 = 10%

Simple Syntax:
<br/>
`PERCENTILE_DISC([Percentile]) WITHIN GROUP (ORDER BY [Field Name] asc)`
<br/>
```sql
select PERCENTILE_DISC(0.9) WITHIN GROUP (ORDER BY PRICE asc) as P90C
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
```

<a id="standardized-moment-skewness"></a>

## STANDARDIZED_MOMENT - SKEWNESS

Returns the Skewness of a set of values. using the `STANDARDIZED_MOMENT` aggregate.
It expects the field to aggregate upon, plus the `degree" value.   ``3` for Skewness.

Simple Syntax:
<br/>
`STANDARDIZED_MOMENT([Field Name],degree=3)`
<br/>
Window Syntax:
<br/>
`STANDARDIZED_MOMENT([Field Name],degree=3) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`STANDARDIZED_MOMENT([Field Name],degree=3) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="standardized-moment-kurtosis"></a>

## STANDARDIZED_MOMENT - KURTOSIS

Returns the Kurtosis of a set of values, using the `STANDARDIZED_MOMENT` aggregate.
It expects the field to aggregate upon, plus the `degree` value.   `4` for Kurtosis.

Simple Syntax:
<br/>
`STANDARDIZED_MOMENT([Field Name],degree=4)`
<br/>
Window Syntax:
<br/>
`STANDARDIZED_MOMENT([Field Name],degree=4) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`STANDARDIZED_MOMENT([Field Name],degree=4) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="stddev"></a>

## STDDEV

Returns the Sample Standard Deviation of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`STDDEV([Field Name])`
<br/>
Window Syntax:
<br/>
`STDDEV([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`STDDEV([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="stddevp"></a>

## STDDEVP

Returns the Population Standard Deviation of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`STDDEVP([Field Name])`
<br/>
Window Syntax:
<br/>
`STDDEVP([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`STDDEVP([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="sum"></a>

## SUM

Returns the sum of non-NaN values. If all values inside the group are NaN the aggregate returns 0.

Simple Syntax:
<br/>
`SUM([Field Name])`
<br/>
Window Syntax:
<br/>
`SUM([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`SUM([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="tw-avg"></a>

## TW_AVG

Returns the Time Weighted Average (TWAP) of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`TW_AVG([Field Name])`
<br/>
Window Syntax:
<br/>
`TW_AVG([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`TW_AVG([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="var"></a>

## VAR

Returns the Sample Variance of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`VAR([Field Name])`
<br/>
Window Syntax:
<br/>
`VAR([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`VAR([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="varp"></a>

## VARP

Returns the Population Variance of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.

Simple Syntax:
<br/>
`VARP([Field Name])`
<br/>
Window Syntax:
<br/>
`VARP([Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`VARP([Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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

<a id="vwap"></a>

## VWAP

Returns the Weighted Average, typically VWAP of non-NaN values. If all values inside the group are NaN the aggregate returns NaN.
It can also be calculated using the Product of the two fields, divided by the sum of the two fields. e,g.  `SUM(PRICE*SIZE)/SUM(SIZE)`

Simple Syntax:
<br/>
`VWAP(price_field_name=[Field Name],size_field_name=[Weight Field Name])`
<br/>
Alternative Syntax:
<br/>
`SUM([Field Name]*[Weight Field Name])/SUM([Weight Field Name])`
<br/>
Window Syntax:
<br/>
`VWAP(price_field_name=[Field Name],size_field_name=[Weight Field Name]) OVER(order by TIMESTAMP asc)`
<br/>
Moving Window Syntax:
<br/>
`VWAP(price_field_name=[Field Name],size_field_name=[Weight Field Name]) OVER(order by TIMESTAMP asc range interval '[Interval Value]' [Interval Period] preceding)`
<br/>
```sql
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
