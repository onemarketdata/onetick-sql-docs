# Technical Analysis

This section contains examples of calculating financial indicators and technical analysis metrics from market data using OneTick SQL. These examples demonstrate how to compute common technical analysis indicators including momentum, volatility, trend, and volume-based metrics.

Technical analysis indicators are calculated using window functions to compute rolling statistics over time or tick-based windows. Many examples include both tick-based calculations and pre-calculated bar aggregations for performance comparison.

## Average True Range (ATR)

The Average True Range (ATR) measures volatility by calculating the average of true ranges over a specified period. The true range is the greatest of: (1) the high-low range, (2) absolute value of high minus prior close, or (3) absolute value of low minus prior close.

Returns the Average True Range (ATR) Indicator. The Period is defined as 1 minute. The Prevailing Price 1 Minute Previously (PRICE_N_BACK) is calculated using the `TIME_SHIFT()` function. The HIGH is calculated as a rolling Maximum across the period and the LOW is calculated as a rolling Minimum across the period. The HIGH, LOW, and PRICE_N_BACK are used to calculate three ranges: High Low Range = HIGH - LOW, Absolute value of High to Prior Price Range = HIGH - PRICE_N_BACK, and Absolute value of Low to Prior Price = LOW - PRICE_N_BACK. The maximum of these three ranges produces the True Range (TR). The Average True Range is calculated as a 14 Period Moving Average.

```sql
 -- Calculates Average True Range (ATR) Indicator from Trade Data

 select PRICE, HIGH, LOW, PRICE_N_BACK,
 TR,
 AVG(TR) over (order by TIMESTAMP asc range interval '14' minute preceding) as ATR
 from
 (
   -- Calculate True Range
   select PRICE, HIGH, LOW, PRICE_N_BACK,
   case
     when HIGH-LOW >= ABS(HIGH - PRICE_N_BACK) and HIGH-LOW >= ABS(LOW - PRICE_N_BACK) then HIGH-LOW
     when ABS(HIGH - PRICE_N_BACK) >= HIGH-LOW  and ABS(HIGH - PRICE_N_BACK) >= ABS(LOW - PRICE_N_BACK) then ABS(HIGH - PRICE_N_BACK)
     else ABS(LOW - PRICE_N_BACK)
   end as TR
   from
   (
     -- Retrieve Price, Prevailing Price 1 minute earlier, and rolling 1 minute High and Low
     select PRICE,
     TIME_SHIFT('PRICE',-(60 * 1000)) as PRICE_N_BACK,
     MAX(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as HIGH, -- Rolling Maximum
     MIN(PRICE) OVER(order by TIMESTAMP asc range interval '1' minute preceding) as LOW   -- Rolling Minimum
     from US_COMP_SAMPLE.TRD t
     where t.SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
     limit 1000000
   )
 )
```

Returns the Average True Range (ATR) Indicator from 1 Minute Trade Bars. The Previous LAST Price is retrieved using LAG(LAST). The HIGH, LOW, LAST and PRIOR_LAST are used to calculate three ranges: High Low Range = HIGH - LOW, Absolute value of High to Prior Last Range = HIGH - PRIOR_LAST, and Absolute value of Low to Prior Last Range = LOW - PRIOR_LAST. The maximum of these three ranges produces the True Range (TR). The Average True Range is calculated as a 14 Period Moving Average.

```sql
 -- Calculates Average True Range (ATR) Indicator from 1 Minute Trade Bars

 select LAST, HIGH, LOW, PRIOR_LAST,
 TR,
 AVG(TR) over (order by TIMESTAMP asc range interval '14' minute preceding) as ATR
 from
 (
   -- Calculate True Range
   select LAST, HIGH, LOW, PRIOR_LAST,
   case
     when HIGH-LOW >= ABS(HIGH - PRIOR_LAST) and HIGH-LOW >= ABS(LOW - PRIOR_LAST) then HIGH-LOW
     when ABS(HIGH - PRIOR_LAST) >= HIGH-LOW  and ABS(HIGH - PRIOR_LAST) >= ABS(LOW - PRIOR_LAST) then ABS(HIGH - PRIOR_LAST)
     else ABS(LOW - PRIOR_LAST)
   end as TR
   from
   (
     -- Retrieve Last, High, Low and Prior Last
     select LAST, HIGH, LOW,
     LAG(LAST) OVER(ORDER BY TIMESTAMP) as PRIOR_LAST
     from US_COMP_SAMPLE_BARS.TRD_1M
     where SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
     limit 1000
   )
 )
```

## Bollinger Bands

Bollinger Bands consist of a moving average (middle band) and two standard deviation bands (upper and lower) calculated from price data. The bands widen during volatile periods and narrow during calm periods, providing insight into volatility and potential overbought/oversold conditions.

In the example below, the subquery calculates a moving average and moving standard deviation across Trade Price with a window of 5 minutes. The parent query then calculates the Upper and Lower Bollinger Bands by adding/subtracting twice the moving standard deviation from the moving average price.

```sql
 -- Calculates Bollinger Bands from Trade Data

 select PRICE, SIZE, MVG_AVG_PRICE,
 mvg_avg_price + 2 * MVG_STDDEV_PRICE as BOLLINGER_UPPER_BAND,
 mvg_avg_price - 2 * MVG_STDDEV_PRICE as BOLLINGER_LOWER_BAND
 from
 (
   -- Calculate Moving Averages and Standard Deviations
   select
   AVG(PRICE) over (order by TIMESTAMP asc range interval '5' minute preceding) as MVG_AVG_PRICE,
   STDDEV(PRICE) over (order by TIMESTAMP asc range interval '5' minute preceding) as MVG_STDDEV_PRICE,
   PRICE, SIZE
   from US_COMP_SAMPLE.TRD
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 )
```

In the example below, the subquery calculates a moving average and moving standard deviation across Trade Price with a window of 5 minutes. The parent query calculates the Upper and Lower Bollinger Bands, Bandwidth as 4 times the moving standard deviation, and percentage Bandwidth by dividing by the moving average price and multiplying by 100.

```sql
 -- Calculates Bollinger Bandwidth from Trade Data

 select PRICE, SIZE, MVG_AVG_PRICE,
 mvg_avg_price + 2 * MVG_STDDEV_PRICE as BOLLINGER_UPPER_BAND,
 mvg_avg_price - 2 * MVG_STDDEV_PRICE as BOLLINGER_LOWER_BAND,
 4 * MVG_STDDEV_PRICE as BOLLINGER_BANDWIDTH,
 100 * (4 * MVG_STDDEV_PRICE / MVG_AVG_PRICE) as PCNT_BOLLINGER_BANDWIDTH
 from
 (
   -- Calculate Moving Averages and Standard Deviations
   select
   AVG(PRICE) over (order by TIMESTAMP asc range interval '5' minute preceding) as MVG_AVG_PRICE,
   STDDEV(PRICE) over (order by TIMESTAMP asc range interval '5' minute preceding) as MVG_STDDEV_PRICE,
   PRICE, SIZE
   from US_COMP_SAMPLE.TRD
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 )
```

## Donchian Channels

Donchian Channels are volatility bands that track the highest high and lowest low over a specified period, with a middle channel representing the midpoint. These channels are commonly used to identify breakouts and support/resistance levels.

The most common period is 20. The Upper Donchian Channel is the rolling Maximum over the last n periods, the Lower Donchian Channel is the rolling Minimum over the last n periods, and the Middle Channel is the midpoint between them.

```sql
 -- Calculates Donchian Channels from Trade Data

 select LAST, UPPER_CHANNEL, LOWER_CHANNEL,
 (UPPER_CHANNEL + LOWER_CHANNEL)/2 as MID_CHANNEL
 from
 (
   -- Calculates the Upper and Lower Channels
   select LAST,
   MAX(HIGH) OVER(order by TIMESTAMP asc range interval '20' minute preceding) as UPPER_CHANNEL, -- Upper Donchian Channel
   MIN(LOW) OVER(order by TIMESTAMP asc range interval '20' minute preceding) as LOWER_CHANNEL -- Upper Donchian Channel
   from US_COMP_SAMPLE_BARS.TRD_1M t
   where t.SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
   limit 1000
 )
```

The most common period is 20. The Upper Donchian Channel is the rolling Maximum over the last n periods, the Lower Donchian Channel is the rolling Minimum over the last n periods, and the Middle Channel is the midpoint between them.

```sql
 -- Calculates Donchian Channels from from 1 Minute Trade Bars

 select LAST, UPPER_CHANNEL, LOWER_CHANNEL,
 (UPPER_CHANNEL + LOWER_CHANNEL)/2 as MID_CHANNEL
 from
 (
   -- Calculates the Upper and Lower Channels
   select LAST,
   MAX(HIGH) OVER(order by TIMESTAMP asc rows 19 preceding) as UPPER_CHANNEL, -- Upper Donchian Channel
   MIN(LOW) OVER(order by TIMESTAMP asc rows 19 preceding) as LOWER_CHANNEL -- Upper Donchian Channel
   from US_COMP_SAMPLE_BARS.TRD_1M t
   where t.SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
   limit 1000
 )
```

## Maximum Drawdown (MDD)

Maximum Drawdown measures the largest peak-to-trough decline from the highest price to the lowest subsequent price, expressed as a percentage. It quantifies the worst-case loss that could have occurred during a trading period.

The Running High Price is calculated across the period. The percentage difference between the Trade Price and Running High is calculated as the percentage Drawdown. The minimum drawdown is calculated across the period.

```sql
 -- Calculates % Maximum Drawdown (MDD) from Trade Data

 select
 PRICE, RUNNING_HIGH_PRICE,
 PCNT_DRAWDOWN,
 MIN(PCNT_DRAWDOWN) OVER(order by TIMESTAMP asc) as MAX_PCNT_DRAWDOWN
 from
 (
   -- Calculate % Drawdown
   select
   PRICE, RUNNING_HIGH_PRICE,
   100 * (PRICE - RUNNING_HIGH_PRICE) / RUNNING_HIGH_PRICE as PCNT_DRAWDOWN
   from
   (
     -- Calculate Running High Price
     select
     PRICE,
     MAX(PRICE) OVER(order by TIMESTAMP asc) as RUNNING_HIGH_PRICE
     from US_COMP_SAMPLE.TRD
     where SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
   )
 )
```

The Running High Price is calculated across the period. The percentage difference between the Last Price and Running High is calculated as the percentage Drawdown. The minimum drawdown is calculated across the period.

```sql
 -- Calculates % Maximum Drawdown (MDD) from 1 Minute Trade Bars

 select
 LAST, RUNNING_HIGH_PRICE,
 PCNT_DRAWDOWN,
 MIN(PCNT_DRAWDOWN) OVER(order by TIMESTAMP asc) as MAX_PCNT_DRAWDOWN
 from
 (
   -- Calculate % Drawdown
   select
   LAST, RUNNING_HIGH_PRICE,
   100 * (LAST - RUNNING_HIGH_PRICE) / RUNNING_HIGH_PRICE as PCNT_DRAWDOWN
   from
   (
     -- Calculate Running High PRice
     select
     LAST,
     MAX(HIGH) OVER(order by TIMESTAMP asc) as RUNNING_HIGH_PRICE
     from US_COMP_SAMPLE_BARS.TRD_1M
     where SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
   )
 )
```

## On-Balance Volume (OBV)

On-Balance Volume is a momentum indicator that relates price change to volume. It accumulates volume with a positive sign when prices rise and a negative sign when prices fall, helping identify the strength of price trends.

Retrieves the PRICE, SIZE and PRIOR_PRICE using `LAG()`. Calculates the Signed SIZE based on whether the PRICE is greater, less or equal to the Prior PRICE. The On-Balance Volume is calculated as the sum of the current and previous SIGNED_SIZE.

```sql
 -- Calculates On-Balance Volume (OBV) from Trade Data

 select PRICE, SIZE, SIGNED_SIZE,
 SUM(SIGNED_SIZE) over (order by TIMESTAMP asc rows between 1 preceding and current row) as OBV
 from
 (
   --Calculate Signed Volume based on whether the PRICE is higher or lower than the Prior PRICE.
   select PRICE, SIZE,
   case
     when PRICE > PRIOR_PRICE then SIZE
     when PRICE < PRIOR_PRICE then -SIZE
     else 0
   end as SIGNED_SIZE
   from
   (
     -- Return Size, Price and Prior Price
     select SIZE, PRICE, LAG(PRICE) OVER(ORDER BY TIMESTAMP) as PRIOR_PRICE
     from US_COMP_SAMPLE.TRD t
     where t.SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
     limit 1000
   )
 )
```

Retrieves the LAST, VOLUME and PRIOR_LAST using `LAG()`. Calculates the Signed VOLUME based on whether the LAST is greater, less or equal to the Prior LAST. The On-Balance Volume is calculated as the sum of the current and previous SIGNED_VOLUME.

```sql
 -- Calculates On-Balance Volume (OBV) from 1 Minute Trade Bars

 select LAST, VOLUME, SIGNED_VOLUME,
 SUM(SIGNED_VOLUME) over (order by TIMESTAMP asc rows between 1 preceding and current row) as OBV
 from
 (
   --Calculate Signed Volume based on whether the Last is higher or lower than the Prior Last.
   select LAST, VOLUME,
   case
     when LAST > PRIOR_LAST then VOLUME
     when LAST < PRIOR_LAST then -VOLUME
     else 0
   end as SIGNED_VOLUME
   from
   (
     -- Return Volume, Last and Prior Last
     select VOLUME, LAST, LAG(LAST) OVER(ORDER BY TIMESTAMP) as PRIOR_LAST
     from US_COMP_SAMPLE_BARS.TRD_1M t
     where t.SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
     limit 1000
   )
 )
```

## Rate of Change (ROC)

Rate of Change measures the percentage change in price over a specified time period. It indicates the speed at which prices are changing and can help identify momentum and potential reversals.

Returns the Rate of Change (ROC) Indicator. The lookback Period is set to 7 seconds. The `TIME_SHIFT()` function is used to return the prevailing PRICE a set number of milliseconds previously. ROC is calculated as 100 \* (PRICE - PRICE_N_BACK) / PRICE_N_BACK.

```sql
 -- Calculates Rate of Change (ROC) Indicator from Trade Data

 select PRICE, PRICE_N_BACK,
 100 * (PRICE - PRICE_N_BACK) / PRICE_N_BACK as ROC
 from
 (
   --Retrieve Price and Prevailing Price 7 seconds earlier
   select PRICE,
   TIME_SHIFT('PRICE',-(7 * 1000)) as PRICE_N_BACK
   from US_COMP_SAMPLE.TRD
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
   limit 1000
 )
```

Returns the Rate of Change (ROC) Indicator from 1 Minute Trade Bars. The lookback Period is set to 7 bars. `LAG(LAST,7)` returns the LAST field 7 records previous as LAST_N_BACK. ROC is calculated as 100 \* (LAST - LAST_N_BACK) / LAST_N_BACK.

```sql
 -- Calculates Rate of Change (ROC) Indicator from 1 Minute Trade Bars

 select LAST, LAST_N_BACK,
 100 * (LAST - LAST_N_BACK) / LAST_N_BACK as ROC
 from
 (
   --Retrieve Price and Prevailing Price 7 Bars earlier
   select LAST, LAG(LAST,7) OVER(ORDER BY TIMESTAMP) as LAST_N_BACK
   from US_COMP_SAMPLE_BARS.TRD_1M
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
   limit 1000
 )
```

## Relative Strength Index (RSI)

The Relative Strength Index measures momentum by comparing the magnitude of recent gains to recent losses. It oscillates between 0 and 100, with values above 70 suggesting overbought conditions and values below 30 suggesting oversold conditions.

Returns the RSI together with the RS (Average Gain over Average Loss). Uses `LAG()` to get previous Price and calculate the Change in Price. Case Statements separate the Gains from the Losses. The Gains and Losses are averaged across a rolling 14 minute period. RS and RSI are calculated from the Moving Average Gains and Losses.

```sql
 -- Calculates RSI Indicator from Trade Data

 select PRICE,
 MVG_AVG_GAIN / MVG_AVG_LOSS as RS,
 100 - (100 / (1 + (MVG_AVG_GAIN / MVG_AVG_LOSS))) as RSI,
 MVG_AVG_GAIN,MVG_AVG_LOSS
 from
 (
   -- Calculate 14 Minute Rolling Average Gain and Loss
   select TIMESTAMP, PRICE,
   AVG(GAIN) over (order by TIMESTAMP asc range interval '14' minute preceding) as MVG_AVG_GAIN,
   AVG(LOSS) over (order by TIMESTAMP asc range interval '14' minute preceding) as MVG_AVG_LOSS
   from
   (
     -- Calculate whether Change is a Gain or a Loss
     select TIMESTAMP, PRICE,
     case when CHANGE_PRICE > 0 then CHANGE_PRICE else 0 end as GAIN,
     case when CHANGE_PRICE < 0 then -CHANGE_PRICE else 0 end as LOSS
     from
     (
       -- Retrieve Prior Price and Change in Price
       select TIMESTAMP, PRICE, SIZE,
       LAG(PRICE) OVER(ORDER BY TIMESTAMP) as PRIOR_PRICE,
       PRICE - LAG(PRICE) OVER(ORDER BY TIMESTAMP) as CHANGE_PRICE
       from US_COMP_SAMPLE.TRD
       where SYMBOL_NAME = 'CSCO'
       and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
       and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
       limit 1000
     )
   )
 )
```

Returns the RSI together with the RS (Average Gain over Average Loss) from 1 Minute Trade Bars. Uses `LAG()` to get previous Last Price and calculate the Change in Last Price. Case Statements separate the Gains from the Losses. The Gains and Losses are averaged across a rolling 14 minute period.

```sql
 -- Calculates RSI Indicator from 1 Minute Trade Bars

 select LAST,
 MVG_AVG_GAIN / MVG_AVG_LOSS as RS,
 100 - (100 / (1 + (MVG_AVG_GAIN / MVG_AVG_LOSS))) as RSI,
 MVG_AVG_GAIN,MVG_AVG_LOSS
 from
 (
   -- Calculate 14 Minute Rolling Average Gain and Loss
   select TIMESTAMP, LAST,
   AVG(GAIN) over (order by TIMESTAMP asc range interval '14' minute preceding) as MVG_AVG_GAIN,
   AVG(LOSS) over (order by TIMESTAMP asc range interval '14' minute preceding) as MVG_AVG_LOSS
   from
   (
     -- Calculate whether Change is a Gain or a Loss
     select TIMESTAMP, LAST,
     case when CHANGE_LAST > 0 then CHANGE_LAST else 0 end as GAIN,
     case when CHANGE_LAST < 0 then -CHANGE_LAST else 0 end as LOSS
     from
     (
       -- Retrieve Prior Last and Change in Last
       select TIMESTAMP, LAST,
       LAG(LAST) OVER(ORDER BY TIMESTAMP) as PRIOR_LAST,
       LAST - LAG(LAST) OVER(ORDER BY TIMESTAMP) as CHANGE_LAST
       from US_COMP_SAMPLE_BARS.TRD_1M
       where SYMBOL_NAME = 'CSCO'
       and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
       and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
       limit 1000
     )
   )
 )
```

## Stochastic Oscillator

The Stochastic Oscillator compares a closing price to the price range over a specified period. It includes %K (raw value) and %D (smoothed value), with values above 80 indicating overbought and values below 20 indicating oversold conditions.

Returns the Stochastic Oscillator. The period is set to minutes in this case but could equally be seconds or days. Uses a 14 period `MAX()` to calculate the highest price traded during the period and a 14 period `MIN()` to calculate the lowest price. `%K` is calculated as `100 \* (PRICE - MLOW) / (MHIGH - MLOW)` and `%D` is calculated as a 3 period `AVG()` moving average.

```sql
 -- Calculates Stochastic Oscillator from Trade Data

 select PRICE, MLOW, MHIGH, PCNT_K,
 AVG(PCNT_K) over (order by TIMESTAMP asc range interval '3' minute preceding) as PCNT_D
 from
 (
   -- Calculate %K
   select PRICE, MLOW, MHIGH,
   100 * (PRICE - MLOW) / (MHIGH - MLOW) as PCNT_K
   from
   (
      -- Calculate 14 Minute Rolling Minimum and Maximum Prices
     select  PRICE,
     MIN(PRICE) over (order by TIMESTAMP asc range interval '14' minute preceding) as MLOW,
     MAX(PRICE) over (order by TIMESTAMP asc range interval '14' minute preceding) as MHIGH
     from US_COMP_SAMPLE.TRD
     where SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
     limit 1000
   )
 )
```

Returns the Stochastic Oscillator from 1 Minute Trade Bars. The period is set to minutes, given the data is 1 minute bars. Uses a 14 period `MAX()` to calculate the highest price traded during the period and a 14 period `MIN()` to calculate the lowest price. `%K` is calculated as `100 \* (LAST - MLOW) / (MHIGH - MLOW)` and `%D` is calculated as a 3 period `AVG()` moving average.

```sql
 -- Calculates Stochastic Oscillator from 1 Minute Trade Bars

 select LAST, MLOW, MHIGH, PCNT_K,
 AVG(PCNT_K) over (order by TIMESTAMP asc range interval '3' minute preceding) as PCNT_D
 from
 (
   -- Calculate %K
   select LAST, MLOW, MHIGH,
   100 * (LAST - MLOW) / (MHIGH - MLOW) as PCNT_K
   from
   (
      -- Calculate 14 Minute Rolling Minimum and Maximum Prices
     select  LAST,
     MIN(LOW) over (order by TIMESTAMP asc rows 13 preceding) as MLOW,
     MAX(HIGH) over (order by TIMESTAMP asc rows 13 preceding) as MHIGH
     from US_COMP_SAMPLE_BARS.TRD_1M
     where SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
     limit 1000
   )
 )
```

## Realized Volatility

Realized Volatility measures the standard deviation of logarithmic returns over a rolling window and annualizes the result. It provides an estimate of volatility based on historical price movements and is commonly used for risk assessment.

The Log Returns are calculated by taking the natural Log of Price divided by the Price Back 1 Period. A 30 minute rolling standard deviation is calculated for the Log Returns. The Result is annualized by multiplying by the number of trading days (252) and the number of 30 minute periods in a day.

```sql
 -- Calculates Realized Volatility (RV) from Trade Data

 select
 LOG_RETURN,
 ROLLING_STDDEV_LOG_RETURN,
 ROLLING_STDDEV_LOG_RETURN * 252 * 13 as ANNUALIZED_RV   -- Calculate the Annualized Realized Volatility
 from
 (
   -- Calculate Rolling Standard Deviation of Log Returns Across a 30 Minute Period
   select
   LOG_RETURN,
   STDDEV(LOG_RETURN) OVER(order by TIMESTAMP asc range interval '30' minute preceding) as ROLLING_STDDEV_LOG_RETURN -- Rolling Standard Deviation
   from
   (
     --Calculate Log Return for the Minute Periods.
     select LOG(LAST_PRICE / LAG(LAST_PRICE) OVER(ORDER BY TIMESTAMP)) as LOG_RETURN
     from
     (
       -- Divide Into Minute Periods
       select LAST(PRICE) as LAST_PRICE
       from US_COMP_SAMPLE.TRD t
       where t.SYMBOL_NAME = 'CSCO'
       and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
       and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
       group by time_bucket(INTERVAL '1' MINUTE)
     )
   )
 )
```

The Log Returns are calculated by taking the natural Log of Price divided by the Prior Price. A 30 minute rolling standard deviation is calculated for the Log Returns. The Result is annualized by multiplying by the number of trading days (252) and the number of 30 minute periods in a day.

```sql
 -- Calculates Realized Volatility (RV) from  1 Minute Trade Bars

 select
 LOG_RETURN,
 ROLLING_STDDEV_LOG_RETURN,
 ROLLING_STDDEV_LOG_RETURN * 252 * 13 as ANNUALIZED_RV
 from
 (
   -- Calculate Rolling Standard Deviation of Log Returns Across a 30 Minute Period
   select
   LOG_RETURN,
   STDDEV(LOG_RETURN) OVER(order by TIMESTAMP asc rows 29 preceding) as ROLLING_STDDEV_LOG_RETURN -- Rolling Standard Deviation
   from
   (
     --Calculate Log Return for the Minute Periods.
     select LOG(LAST /  LAG(LAST) OVER(ORDER BY TIMESTAMP))  as LOG_RETURN
     from US_COMP_SAMPLE_BARS.TRD_1M t
     where t.SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
     limit 1000
   )
 )
```

## Rolling Standard Deviation

Rolling Standard Deviation calculates the standard deviation of prices within a rolling time window. It measures price volatility and adapts to changing market conditions, increasing during volatile periods and decreasing during stable ones.

Returns the Rolling Standard Deviation. The Period is defined as 5 minutes. The rolling Standard Deviation is calculated across the period.

```sql
 -- Calculates Rolling Standard Deviation from Trade Data

 select PRICE,
 STDDEV(PRICE) OVER(order by TIMESTAMP asc range interval '5' minute preceding) as ROLLING_STDDEV_PRICE -- Rolling Standard Deviation
 from US_COMP_SAMPLE.TRD t
 where t.SYMBOL_NAME = 'CSCO'
 and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
 and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
 limit 1000
```

Returns the Rolling Standard Deviation from 1 Minute Trade Bars. The Period is defined as 5 minutes. The rolling Standard Deviation is calculated across the period.

```sql
 -- Calculates Rolling Standard Deviation from 1 Minute Trade Bars

 select LAST,
 STDDEV(LAST) OVER(order by TIMESTAMP asc rows 4 preceding) as ROLLING_STDDEV_PRICE -- Rolling Standard Deviation
 from US_COMP_SAMPLE_BARS.TRD_1M t
 where t.SYMBOL_NAME = 'CSCO'
 and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
 and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
 limit 1000
```

## Volume Bars

Volume Bars aggregate trades into fixed-volume bins rather than fixed-time intervals. This approach focuses analysis on periods of significant trading activity and can reveal patterns masked by time-based aggregation during varying liquidity conditions.

The accumulative volume is calculated across the period and divided by the required Fixed Volume, then floored to create a set of Volume Bins. The trading day is then aggregated grouped by the Volume Bin. As the Volume Bins cover different time ranges, the Start and End time for each Bin are also retrieved.

```sql
 -- Calculates Bars by Fixed Volume Bin from Trade Data

 select
 VOL_BIN,
 MIN(TIMESTAMP) as BIN_START,
 MAX(TIMESTAMP) as BIN_END,
 FIRST(PRICE) as FIRST,
 MAX(PRICE) as HIGH,
 MIN(PRICE) as LOW,
 LAST(PRICE) as LAST,
 COUNT(SIZE) as TRADE_COUNT,
 SUM(SIZE) as VOLUME
 from
 (
     -- Retrieve Volume and Calculate Volume Bins
     select
     PRICE,
     SIZE,
     floor(sum(SIZE) over(order by TIMESTAMP asc) / 100000) as VOL_BIN
     from LSE_SAMPLE.TRD
     where SYMBOL_NAME = 'VOD'
     and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
     and TIMESTAMP < '2024-01-03 16:00:00 Europe/London'
 )
 group by VOL_BIN
```

## Volume Profile

Volume Profile shows the distribution of trading volume across different price levels during a trading period. It identifies support and resistance levels based on where the most trading activity occurred.

Retrieves the VOLUME and the TRADE_COUNT grouped by PRICE.

```sql
 -- Calculates Volume Profile or Volume Histogram from Trade Data

 select PRICE, sum(SIZE) as VOLUME,
 count(SIZE) as TRADE_COUNT
 from US_COMP_SAMPLE.TRD t
 where t.SYMBOL_NAME = 'CSCO'
 and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
 and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 group by PRICE
```

Calculate the Price Range Across the Trading Period and divide by the number of Required Samples Minus 1 to produce the TICK_SIZE. Join the TICK_SIZE to the Trading Data and calculate the Price floored to a specific sample as PRICE_BIN. Retrieves the VOLUME and the TRADE_COUNT grouped by PRICE_BIN.

```sql
 -- Calculates Volume Profile or Volume Histogram by Sample from Trade Data

 select FLOOR(PRICE / TICK_SIZE) as PRICE_BIN,
 sum(SIZE) as VOLUME,
 count(SIZE) as TRADE_COUNT
 from
 (
   select t.PRICE as PRICE,
   r.TICK_SIZE as TICK_SIZE,
   t.SIZE as SIZE
   from US_COMP_SAMPLE.TRD t,
   (
     -- Calculates the Tick Size based on the Price Range
     select SYMBOL_NAME, (MAX(PRICE) - MIN(PRICE))/99 as TICK_SIZE
     from US_COMP_SAMPLE.TRD
     where SYMBOL_NAME = 'CSCO'
     group by SYMBOL_NAME
   ) r
   where t.SYMBOL_NAME = 'CSCO'
   and t.SYMBOL_NAME = r.SYMBOL_NAME
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 )
 group by PRICE_BIN
```

Specify a Tick Size (in this case 1 cent, 0.01). Calculate the Price floored to a specific Tick Size as PRICE_BIN. Retrieves the VOLUME and the TRADE_COUNT grouped by PRICE_BIN.

```sql
 -- Calculates Volume Profile or Volume Histrogram by Tick Size from Trade Data

 select FLOOR(PRICE / 0.01) * 0.01 as PRICE_BIN,
 sum(SIZE) as VOLUME,
 count(SIZE) as TRADE_COUNT
 from US_COMP_SAMPLE.TRD t
 where t.SYMBOL_NAME = 'CSCO'
 and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
 and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 group by PRICE_BIN
```

## Volume Spike Detection

Volume Spike Detection identifies periods of abnormally high trading volume compared to recent historical averages. This can signal significant market events, breakouts, or changes in market sentiment.

Volume is aggregated based on defined bucket period. Average Volume is calculated based on the last N buckets. Spikes are identified if the Volume is greater than twice the average volume. Retrieves the VOLUME and the TRADE_COUNT grouped by PRICE.

```sql
 -- Identifying Volume Spikes from Trade Data

 select LAST_PRICE, VOLUME, TRADE_COUNT, MAVG_VOLUME,
 case
   when VOLUME > 2 * MAVG_VOLUME then 1
   else 0
 end as SPIKES
 from
 (
   -- Calculate Average Volume Across Recent Prior Intervals
   select LAST_PRICE, VOLUME, TRADE_COUNT,
   AVG(VOLUME) over (order by TIMESTAMP asc rows 4 preceding) as MAVG_VOLUME
   from
   (
     -- Bucket Volume Per Time Interval
     select last (PRICE) as LAST_PRICE,
     sum(SIZE) as VOLUME,
     count(SIZE) as TRADE_COUNT
     from US_COMP_SAMPLE.TRD t
     where t.SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
     group by time_bucket(INTERVAL '1' MINUTE)
   )
 )
```

Volume is aggregated based on defined bucket period from 1 Minute Trade Bars. Average Volume is calculated based on the last N buckets. Spikes are identified if the Volume is greater than twice the average volume.

```sql
 -- Identifying Volume Spikes from 1 Minute Trade Bars

 select LAST, VOLUME, MAVG_VOLUME,
 case
   when VOLUME > 2 * MAVG_VOLUME then 1
   else 0
 end as SPIKES
 from
 (
   -- Calculate Average Volume Across Recent Prior Intervals
   select LAST, VOLUME,
   AVG(VOLUME) over (order by TIMESTAMP asc rows 4 preceding) as MAVG_VOLUME
   from US_COMP_SAMPLE_BARS.TRD_1M
   where SYMBOL_NAME = 'CSCO'
   and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
   and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 )
```

## Volume Surge Indicator

The Volume Surge Indicator measures the ratio of current volume to average volume, highlighting periods when volume significantly deviates from normal levels. Volume is aggregated based on a defined bucket period. Average Volume is calculated based on the last 100 buckets. Volume Surge is calculated as the Current Volume divided by the Average Volume, expressed as a percentage ratio.

```sql
 -- Calculates Volume Surge Indicator from Trade Data

 select LAST_PRICE, VOLUME, TRADE_COUNT, MAVG_VOLUME,
 100 * VOLUME / MAVG_VOLUME as VOLUME_SURGE
 from
 (
   -- Calculate Average Volume Across Recent Prior Intervals
   select LAST_PRICE, VOLUME, TRADE_COUNT,
   AVG(VOLUME) over (order by TIMESTAMP asc rows 99 preceding) as MAVG_VOLUME
   from
   (
     -- Bucket Volume Per Time Interval
     select last (PRICE) as LAST_PRICE,
     sum(SIZE) as VOLUME,
     count(SIZE) as TRADE_COUNT
     from US_COMP_SAMPLE.TRD t
     where t.SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
     group by time_bucket(INTERVAL '1' MINUTE)
   )
 )
```

Volume is aggregated based on a defined bucket period from 1-Minute Bars. Average Volume is calculated based on the last 100 buckets. Volume Surge is calculated as a percentage ratio of current to average volume.

```sql
 -- Calculates Volume Surge Indicator from 1 Minute Trade Bars

 select LAST_PRICE, VOLUME, TRADE_COUNT, MAVG_VOLUME,
 100 * VOLUME / MAVG_VOLUME as VOLUME_SURGE
 from
 (
   -- Calculate Average Volume Across Recent Prior Intervals
   select LAST_PRICE, VOLUME, TRADE_COUNT,
   AVG(VOLUME) over (order by TIMESTAMP asc rows 99 preceding) as MAVG_VOLUME
   from
   (
     -- Bucket Volume Per Time Interval
     select last (PRICE) as LAST_PRICE,
     sum(SIZE) as VOLUME,
     count(SIZE) as TRADE_COUNT
     from US_COMP_SAMPLE.TRD t
     where t.SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
     group by time_bucket(INTERVAL '1' MINUTE)
   )
 )
```

## Aggressor Volume Imbalance

Aggressor Volume Imbalance measures the imbalance between buy-side and sell-side volume, determined by the aggressor side of trades. High imbalances can indicate directional pressure in the market and potential price movements.

Aggressor Volume Imbalance is calculated for Venues that publish the AGGRESSOR_SIDE field. BUY_VOLUME is returned when AGGRESSOR_SIDE = ‘B’ and SELL_VOLUME when AGGRESSOR_SIDE = ‘S’. Volume is aggregated based on a defined bucket period. `IMBALANCE_VOLUME = BUY_VOLUME - SELL_VOLUME` and percentage imbalance `= (BUY_VOLUME - SELL_VOLUME) / (BUY_VOLUME + SELL_VOLUME)`.

```sql
 -- Calculates Aggressor Volume Imbalance from Trade Data

 select VOLUME, BUY_VOLUME, SELL_VOLUME,
 BUY_VOLUME - SELL_VOLUME as IMBALANCE_VOLUME,
 100 * (BUY_VOLUME - SELL_VOLUME)/(BUY_VOLUME + SELL_VOLUME) as PCNT_IMBALANCE_VOLUME
 from
 (
   -- Sum Buy and Sell Volume
   select
     sum(SIZE) as VOLUME,
     sum(BUY_VOLUME) as BUY_VOLUME,
     sum(SELL_VOLUME) as SELL_VOLUME
   from
   (
     -- Calculate Buy and Sell Volume based on Aggressor Side
     select AGGRESSOR_SIDE,
     SIZE,
     case when AGGRESSOR_SIDE = 'B' then SIZE else 0 end as BUY_VOLUME,
     case when AGGRESSOR_SIDE = 'S' then SIZE else 0 end as SELL_VOLUME
     from LSE.TRD
     where SYMBOL_NAME = 'VOD'
     and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
     and TIMESTAMP < '2024-01-03 16:00:00 Europe/London'
   )
   group by time_bucket(INTERVAL '1' MINUTE)
 )
```

## Order Flow Imbalance

Order Flow Imbalance (OFI) is a quantitative metric that measures the net change in supply and demand at the best bid and ask prices. It captures order flow dynamics and can be used to detect informed trading activity. OFI is a metric introduced by Rama Cont, Arseniy Kukanov, and Sasha Stoikov in 2014, measuring the net change in supply and demand at the best bid and ask prices across a specific time interval. As a Composite is used (US_COMP), the NBBO table is retrieved, rather than the QTE table.

```sql
 -- Calculates Order Flow Imbalance (OFI) from Quote Data

 select BID_FLOW - ASK_FLOW as OFI
 from
 (
   -- Calculate Bid and Ask Flow
   select
     case
       when BID_PRICE > PRIOR_BID_PRICE then BID_SIZE
       when BID_PRICE < PRIOR_BID_PRICE then PRIOR_BID_SIZE
       else BID_SIZE - PRIOR_BID_SIZE
     end as BID_FLOW,
     case
       when ASK_PRICE > PRIOR_ASK_PRICE then ASK_SIZE
       when ASK_PRICE < PRIOR_ASK_PRICE then PRIOR_ASK_SIZE
       else ASK_SIZE - PRIOR_ASK_SIZE
     end as ASK_FLOW
   from
   (
     -- Retrieve Current and Prior Bid and Ask Prices and Sizes
     select
     BID_PRICE,BID_SIZE,
     ASK_PRICE,ASK_SIZE,
     LAG(BID_PRICE) OVER(ORDER BY TIMESTAMP) as PRIOR_BID_PRICE,
     LAG(BID_SIZE) OVER(ORDER BY TIMESTAMP) as PRIOR_BID_SIZE,
     LAG(ASK_PRICE) OVER(ORDER BY TIMESTAMP) as PRIOR_ASK_PRICE,
     LAG(ASK_SIZE) OVER(ORDER BY TIMESTAMP) as PRIOR_ASK_SIZE
     from US_COMP_SAMPLE.NBBO
     where SYMBOL_NAME = 'CSCO'
     and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
     and TIMESTAMP < '2024-01-03 16:30:00 America/New_York'
     limit 1000
   )
 )
```

## Volume Synchronized Probability of Informed Trading (VPIN)

VPIN is an advanced metric that estimates the probability of informed trading based on volume-synchronized order flow imbalance. It divides trading into equal-volume buckets and analyzes the imbalance within each bucket to detect informed trading activity. Volume Bins are calculated based on fixed volume thresholds (e.g., every 100,000 shares). BUY_VOLUME is calculated when AGGRESSOR_SIDE = ‘B’ and SELL_VOLUME when AGGRESSOR_SIDE = ‘S’. Volume is aggregated within each Volume Bin. BIN_IMBALANCE is calculated as the absolute difference between buy and sell volume divided by total volume. VPIN is calculated as a rolling average of BIN_IMBALANCE over a specified number of bins.

```sql
 -- Calculates Volume Synchronized Probability of Informed Trading (VPIN) from Trade Data

 select
 VOL_BIN, BIN_START, BIN_END, TRADE_COUNT, VOLUME, BUY_VOLUME, SELL_VOLUME, BIN_IMBALANCE,
 AVG(BIN_IMBALANCE) over (order by VOL_BIN asc rows 49 preceding) as VPIN
 from
 (
   -- Calculate Aggregates for Volume Bins
   select
   VOL_BIN,
   MIN(TIMESTAMP) as BIN_START,
   MAX(TIMESTAMP) as BIN_END,
   COUNT(SIZE) as TRADE_COUNT,
   SUM(SIZE) as VOLUME,
   SUM(BUY_VOLUME) as BUY_VOLUME,
   SUM(SELL_VOLUME) as SELL_VOLUME,
   ABS(SUM(BUY_VOLUME) - SUM(SELL_VOLUME)) / SUM(SIZE) as BIN_IMBALANCE
   from
   (
       -- Retrieve Buy and Sell Volume based on Aggressor Side and Calculate Volume Bins
       select
       AGGRESSOR_SIDE,
       SIZE,
       case when AGGRESSOR_SIDE = 'B' then SIZE else 0 end as BUY_VOLUME,
       case when AGGRESSOR_SIDE = 'S' then SIZE else 0 end as SELL_VOLUME,
       floor(sum(SIZE) over(order by TIMESTAMP asc) / 100000) as VOL_BIN
       from LSE.TRD
       where SYMBOL_NAME = 'VOD'
       and TIMESTAMP >= '2024-01-03 08:00:00 Europe/London'
       and TIMESTAMP < '2024-01-03 16:00:00 Europe/London'
   )
   group by VOL_BIN
 )
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
