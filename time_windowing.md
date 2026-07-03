# Windowing

A series of time windowing examples are provided showing how to aggregate across time windows, and retrieve time and record relative values, using OneTick Cloud sample databases.
These use the SQL windowing, by including the `OVER` clause, sorting by time, using `order by TIMESTAMP asc`.

e.g. `OVER (order by TIMESTAMP asc)`

Rolling time windows are defined by defining a `range interval`, within the `OVER` clause, where the units can be `MILLISECOND`, `SECOND`, `MINUTE`, `HOUR`, `DAY`, `WEEK`.

e.g. `OVER (order by TIMESTAMP asc range interval '1' minute preceding)`

Rolling record windows can be defined by specifyiing a row count within the `OVER` clause.

e.g. `OVER (order by TIMESTAMP asc rows 50 preceding)`

## Accumulative Sum

Accumulative sums are returned through use of both the `SUM` aggregate, plus the `OVER` clause, ordering by `TIMESTAMP` asc.

```sql
select PRICE, SIZE,
SUM(SIZE) OVER(order by TIMESTAMP asc) as ACCUM_SIZE
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 10
```

## Accumulative Sum of SIZE across the time window results

| Symbol

 | Timestamp

 | PRICE

 | SIZE

 | ACCUM_SIZE

 |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------- | ----------------------------- | ----------------------------- | ------------------------------- | ----------------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ------------------- | ----------------------------- | ----------------------------- | -------------------- | ----------------------- | ----------------------- | -------------- | -------------------- | ----------------------- | ----------------------- | --------------------- | ----------------------- | ----------------------- | ----------------- | ----------------------- | ------------- | ------------------- | ----------------------- | ---------------- | ---------- | ----------------------- | ------------- | ----------- | -------------- | ---------------- | -------- |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.232

                                                                                                           | 70

                                             | 184,613

              | 184,613

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.233

                                                                                                           | 70.01

                                          | 140

                  | 184,753

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.287

                                                                                                           | 70.01

                                          | 500

                  | 185,253

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:08.113

                                                                                                           | 70.104

                                         | 2,800

                | 188,053

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.380

                                                                                                           | 70.137

                                         | 49

                   | 188,102

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.943

                                                                                                           | 70.0981

                                        | 66,908

               | 255,010

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.944

                                                                                                           | 70.1097

                                        | 214

                  | 255,224

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.119

                                                                                                           | 70.146

                                         | 840

                  | 256,064

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.933

                                                                                                           | 70.132

                                         | 344

                  | 256,408

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:20.863

                                                                                                           | 70.19

                                          | 4,012

                | 260,420

                       |
## Rolling Sum

Rolling sums are returned with a syntax similar to the Accumulative Sum.  However in this case the `OVER` clause includes a `range interval`.
e.g. `over(order by TIMESTAMP asc range interval '60' second preceding)`

```sql
select PRICE, SIZE,
SUM(SIZE) over(order by TIMESTAMP asc range interval '60' second preceding) as ROLLING_SIZE_1_MIN
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 1000
```

## Rolling Sum of Trade Size across a 60 second rolling period results

| Symbol

                      | Timestamp

                                                                                                                         | PRICE

                                          | SIZE

                 | ROLLING_SIZE_1_MIN

            |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------- | ----------------------------- |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.232

                                                                                                           | 70

                                             | 184,613

              | 184,613

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.233

                                                                                                           | 70.01

                                          | 140

                  | 184,753

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.287

                                                                                                           | 70.01

                                          | 500

                  | 185,253

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:08.113

                                                                                                           | 70.104

                                         | 2,800

                | 188,053

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.380

                                                                                                           | 70.137

                                         | 49

                   | 188,102

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.943

                                                                                                           | 70.0981

                                        | 66,908

               | 255,010

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.944

                                                                                                           | 70.1097

                                        | 214

                  | 255,224

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.119

                                                                                                           | 70.146

                                         | 840

                  | 256,064

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.933

                                                                                                           | 70.132

                                         | 344

                  | 256,408

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:20.863

                                                                                                           | 70.19

                                          | 4,012

                | 260,420

                       |
## Moving Average in Time

Simple moving averages are returned with the `OVER` clause, including the `range interval`.

```sql
select PRICE,
AVG(PRICE) over (order by TIMESTAMP asc range interval '1' minute preceding) as sma1_price,
AVG(PRICE) over (order by TIMESTAMP asc range interval '5' minute preceding) as sma5_price
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 10
```

## Trade Price plus 1 and 5 Minute Moving averages retrieval results.

| Symbol

                      | Timestamp

                                                                                                                         | PRICE

                                          | SMA5_PRICE

           | SMA1_PRICE

                    |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------- | ----------------------------- |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.232

                                                                                                           | 70

                                             | 70

                   | 70

                            |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.233

                                                                                                           | 70.01

                                          | 70.005

               | 70.005

                        |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.287

                                                                                                           | 70.01

                                          | 70.0066666667

        | 70.0066666667

                 |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:08.113

                                                                                                           | 70.104

                                         | 70.031

               | 70.031

                        |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.380

                                                                                                           | 70.137

                                         | 70.0522

              | 70.0522

                       |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.943

                                                                                                           | 70.0981

                                        | 70.05985

             | 70.05985

                      |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.944

                                                                                                           | 70.1097

                                        | 70.0669714286

        | 70.0669714286

                 |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.119

                                                                                                           | 70.146

                                         | 70.07685

             | 70.07685

                      |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.933

                                                                                                           | 70.132

                                         | 70.0829777778

        | 70.0829777778

                 |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:20.863

                                                                                                           | 70.19

                                          | 70.09368

             | 70.09368

                      |
## Moving Average in Records

Moving averages can be calculated based on a number of `rows`, rather than a `range interval`.

```sql
select PRICE, AVG(PRICE) over (order by TIMESTAMP asc rows 5 preceding) as mavg_price
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 10
```

## 5 Row Moving average retrieval results.

| Symbol

                      | Timestamp

                                                                                                                         | PRICE

                                          | MAVG_PRICE

           |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------- |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.232

                                                                                                           | 70

                                             | 70

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.233

                                                                                                           | 70.01

                                          | 70.005

               |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.287

                                                                                                           | 70.01

                                          | 70.0066666667

        |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:08.113

                                                                                                           | 70.104

                                         | 70.031

               |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.380

                                                                                                           | 70.137

                                         | 70.0522

              |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.943

                                                                                                           | 70.0981

                                        | 70.05985

             |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.944

                                                                                                           | 70.1097

                                        | 70.0781333333

        |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.119

                                                                                                           | 70.146

                                         | 70.1008

              |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.933

                                                                                                           | 70.132

                                         | 70.1211333333

        |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:20.863

                                                                                                           | 70.19

                                          | 70.1354666667

        |
## Exponential Moving Average

Exponential moving averages are calculated using specialized window functions that apply exponential decay to historical data points.
Two types of exponential moving averages are available: record-based exponential weighting (`EXP_W_AVERAGE`) and time-based exponential weighting (`EXP_TW_AVERAGE`).


* `EXP_W_AVERAGE` - Applies exponential weighting based on record decay factor


* `EXP_TW_AVERAGE` - Applies exponential weighting based on time decay factor (in milliseconds)

```sql
select PRICE,
EXP_W_AVERAGE(PRICE,DECAY=0.01) OVER(order by TIMESTAMP asc) as ewa_price,
EXP_TW_AVERAGE(PRICE,DECAY=300) OVER(order by TIMESTAMP asc) as etwa_price
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
```

## Time Shifts

Values can be retrieved relative to the current row by a specified time period in milliseconds using the `time_shift` function. , which requires the field enclosed in quotes, plus the time shift.
Negative time shifts retrieve the prevailing value at the specified number of milliseconds before the row timestamp.
Positive time shifts retrieve the prevailing value at the specified number of milliseconds after the row timestamp.

```sql
select TIMESTAMP, PRICE, SIZE, TRADE_ID, TRADE_VENUE,
time_shift('PRICE',-1000) as PRICE_BACK_1S,
time_shift('PRICE',-10000) as PRICE_BACK_10S,
time_shift('PRICE',-60000) as PRICE_BACK_60S,
time_shift('PRICE',1000) as PRICE_FWD_1S,
time_shift('PRICE',10000) as PRICE_FWD_10S,
time_shift('PRICE',60000) as PRICE_FWD_60S
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 10
```

## Trades with Time shifts 1, 10, and 60 seconds before and after each trade retrieval results

| Symbol

                      | Timestamp

                                                                                                                         | PRICE

                                          | SIZE

                 | TRADE_ID

                      | TRADE_VENUE

                   | PRICE_BACK_1S

                   | PRICE_BACK_10S

                | PRICE_BACK_60S

          | PRICE_FWD_1S

            | PRICE_FWD_10S

           | PRICE_FWD_60S

           |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------- | ----------------------------- | ----------------------------- | ------------------------------- | ----------------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.232

                                                                                                           | 70

                                             | 184,613

              | 911727684223506

               | XLON

                          |                                 |                               |                         | 70.104

                  | 70.19

                   | 70.45

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.233

                                                                                                           | 70.01

                                          | 140

                  | 911727684223588

               | XLON

                          |                                 |                               |                         | 70.104

                  | 70.19

                   | 70.45

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.287

                                                                                                           | 70.01

                                          | 500

                  | 911727684223589

               | XLON

                          |                                 |                               |                         | 70.104

                  | 70.19

                   | 70.45

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:08.113

                                                                                                           | 70.104

                                         | 2,800

                | 25706436899852400

             | XLON

                          | 70.01

                           |                               |                         | 70.137

                  | 70.19

                   | 70.08

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.380

                                                                                                           | 70.137

                                         | 49

                   | 892755055723892848

            | XLON

                          | 70.104

                          |                               |                         | 70.132

                  | 70.19

                   | 70.08

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.943

                                                                                                           | 70.0981

                                        | 66,908

               | 50599517591654512

             | XLON

                          | 70.104

                          |                               |                         | 70.19

                   | 70.19

                   | 70.08

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.944

                                                                                                           | 70.1097

                                        | 214

                  | 77621115355877488

             | XLON

                          | 70.104

                          |                               |                         | 70.19

                   | 70.19

                   | 70.08

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.119

                                                                                                           | 70.146

                                         | 840

                  | 390726911293870192

            | XLON

                          | 70.104

                          |                               |                         | 70.19

                   | 70.19

                   | 70.08

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.933

                                                                                                           | 70.132

                                         | 344

                  | 953676864715182192

            | XLON

                          | 70.137

                          |                               |                         | 70.19

                   | 70.23

                   | 70.08

                   |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:20.863

                                                                                                           | 70.19

                                          | 4,012

                | 911727684224102

               | XLON

                          | 70.132

                          | 70.146

                        |                         | 70.265

                  | 70.3

                    | 70.44

                   |
## Record Shifts / Lag & Lead

Values can be retrieved relative to the current row by a specified number of records using the `LAG` and `LEAD` functions.

    They require the field, and assume a 1 record offset, or additionally the number of records to shift is also included.


    * `LAG(MID_PRICE)` - Return the `MID_PRICE` shifted back 1 record


    * `LAG(MID_PRICE,5)` - Return the `MID_PRICE` shifted back 5 records

```sql
select TIMESTAMP, PRICE, SIZE, TRADE_ID, TRADE_VENUE,
LAG(PRICE) as PRICE_1_BACK,LAG(TRADE_ID) as TRADE_ID_1_BACK,
LAG(PRICE,5) as PRICE_5_BACK,LAG(TRADE_ID,5) as TRADE_ID_5_BACK,
LEAD(PRICE) as PRICE_1_FWD,LEAD(TRADE_ID) as TRADE_ID_1_FWD,
LEAD(PRICE,5) as PRICE_5_FWD,LEAD(TRADE_ID,5) as TRADE_ID_5_FWD
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 10
```

## Trades with shifts 1 and 5 records before and after each trade retrieval results

| Symbol

                      | Timestamp

                                                                                                                         | PRICE

                                          | SIZE

                 | TRADE_ID

                      | TRADE_VENUE

                   | PRICE_1_BACK

                    | TRADE_ID_1_BACK

               | PRICE_5_BACK

            | TRADE_ID_5_BACK

         | PRICE_1_FWD

             | TRADE_ID_1_FWD

          | PRICE_5_FWD

         | TRADE_ID_5_FWD

                |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------- | ----------------------------- | ----------------------------- | ------------------------------- | ----------------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ------------------- | ----------------------------- |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.232

                                                                                                           | 70

                                             | 184,613

              | 911727684223506

               | XLON

                          |                                 |                               |                         |                         | 70.01

                   | 911727684223588

         | 70.0981

             | 50599517591654512

             |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.233

                                                                                                           | 70.01

                                          | 140

                  | 911727684223588

               | XLON

                          | 70

                              | 911727684223506

               |                         |                         | 70.01

                   | 911727684223589

         | 70.1097

             | 77621115355877488

             |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:06.287

                                                                                                           | 70.01

                                          | 500

                  | 911727684223589

               | XLON

                          | 70.01

                           | 911727684223588

               |                         |                         | 70.104

                  | 25706436899852400

       | 70.146

              | 390726911293870192

            |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:08.113

                                                                                                           | 70.104

                                         | 2,800

                | 25706436899852400

             | XLON

                          | 70.01

                           | 911727684223589

               |                         |                         | 70.137

                  | 892755055723892848

      | 70.132

              | 953676864715182192

            |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.380

                                                                                                           | 70.137

                                         | 49

                   | 892755055723892848

            | XLON

                          | 70.104

                          | 25706436899852400

             |                         |                         | 70.0981

                 | 50599517591654512

       | 70.19

               | 911727684224102

               |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.943

                                                                                                           | 70.0981

                                        | 66,908

               | 50599517591654512

             | XLON

                          | 70.137

                          | 892755055723892848

            | 70

                      | 911727684223506

         | 70.1097

                 | 77621115355877488

       | 70.24

               | 911727684224103

               |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:09.944

                                                                                                           | 70.1097

                                        | 214

                  | 77621115355877488

             | XLON

                          | 70.0981

                         | 50599517591654512

             | 70.01

                   | 911727684223588

         | 70.146

                  | 390726911293870192

      | 70.26

               | 911727684224104

               |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.119

                                                                                                           | 70.146

                                         | 840

                  | 390726911293870192

            | XLON

                          | 70.1097

                         | 77621115355877488

             | 70.01

                   | 911727684223589

         | 70.132

                  | 953676864715182192

      | 70.26

               | 911727684224105

               |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:10.933

                                                                                                           | 70.132

                                         | 344

                  | 953676864715182192

            | XLON

                          | 70.146

                          | 390726911293870192

            | 70.104

                  | 25706436899852400

       | 70.19

                   | 911727684224102

         | 70.23

               | 911727684224190

               |
| LSE_SAMPLE::VOD

             | 2024-01-03 08:00:20.863

                                                                                                           | 70.19

                                          | 4,012

                | 911727684224102

               | XLON

                          | 70.132

                          | 953676864715182192

            | 70.137

                  | 892755055723892848

      | 70.24

                   | 911727684224103

         | 70.23

               | 911727684224191

               |
## Time-Weighted Average Price (TWAP)

Time-Weighted Average Price calculates the average price weighted by the time intervals between trades.
Useful for trade execution analysis and as a benchmark for algorithm performance evaluation.

```sql
select * from US_COMP_SAMPLE.TRD
where symbol_name = 'CSCO'
and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
LIMIT 1000
```

## Time-Weighted Average Size (TWAS)

Time-Weighted Average Size calculates the average trade size weighted by the time intervals between trades across a specified period.
Unlike a simple arithmetic average of trade sizes, TWAS accounts for how long each trade size persists in the market.
A trade size that exists for longer periods has greater influence on the final TWAS value.

The calculation weights each trade size by the time interval until the next trade, providing insight into typical execution sizes over different time windows.

Calculates TWAS for the entire specified time range.

```sql
select avg(SIZE) as TWAS
from US_COMP_SAMPLE.TRD
where symbol_name = 'CSCO'
and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
```

Aggregates TWAS grouped by exchange venue to analyze size distribution across different markets.

```sql
select EXCHANGE, avg(SIZE) as TWAS
from US_COMP_SAMPLE.TRD
where symbol_name = 'CSCO'
and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
group by EXCHANGE
```

Divides the time range into equal intervals and calculates TWAS for each bucket to show size trends over time.

```sql
select
floor(TIMESTAMP, 1 minute) as BUCKET_TIME,
avg(SIZE) as TWAS
from US_COMP_SAMPLE.TRD
where symbol_name = 'CSCO'
and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
group by BUCKET_TIME
```

Calculates TWAS using a rolling time window to show how average size changes as time advances.

```sql
select
TIMESTAMP,
avg(SIZE) over(order by TIMESTAMP asc range interval '1' minute preceding) as ROLLING_TWAS
from US_COMP_SAMPLE.TRD
where symbol_name = 'CSCO'
and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
LIMIT 1000
```

## Dynamic Bar Creation with Fill Forward

Returns OHLC bars with the last price filled forward into subsequent time buckets even when no trades occur. Uses `TIME_SERIES_TYPE='STATE_TS'` on the `LAST` aggregate function to extend the previous bar’s closing price forward. Useful for creating complete time series where every bucket has a value, even during low-volume periods.

```sql
 SELECT
 FIRST(PRICE) as FIRST_PRICE,
 MAX(PRICE) as HIGH_PRICE,
 MIN(PRICE) as LOW_PRICE,
 LAST(PRICE,TIME_SERIES_TYPE='STATE_TS') as LAST_PRICE,
 VWAP(PRICE_FIELD_NAME=PRICE, SIZE_FIELD_NAME=SIZE) as VWAP_PRICE,
 SUM(SIZE) as VOLUME,
 COUNT(SIZE) as TRADE_COUNT
 FROM US_COMP_SAMPLE.TRD
 WHERE SYMBOL_NAME = 'HD'
 and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
 and TIMESTAMP < '2024-01-03 16:00:00 America/New_York'
 and SIZE > 1000
 group by time_bucket(INTERVAL '5' MINUTE)
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
