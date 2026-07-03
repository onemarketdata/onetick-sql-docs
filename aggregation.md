# Aggregation

The following aggregations are supported:

`SUM()`,
`AVG()`,
`TW_AVG()`,
`MIN()`,
`MAX()`,
`COUNT()`,
`FIRST()`,
`LAST()`,
`VAR()`,
`VARP()`,
`STDDEV()`,
`STDDEVP()`,
`FIRST_TIME()`,
`LAST_TIME()`,
`HIGH_TIME()`,
`LOW_TIME()`,
`VWAP()`,
`MEDIAN()`,
`CORRELATION()`,
`EXP_TW_AVERAGE()`,
`EXP_W_AVERAGE()`,
`STANDARDIZED_MOMENT()`,
`RANKING()`.

Results can be grouped using `GROUP BY` with either:


* A field from the table. e.g. `EXCHANGE`


* A time bucket using the `time_bucket` function


* A row bucket using the `ticks_bucket` function

A series of simple examples are provided below showing how to retrieve aggregated market data from [OneTick Cloud](https://www.onetick.com/cloud-services).

## Common Aggregations

Common aggregation methods include `SUM()`, `AVG()`, `MIN()`, `MAX()`, `COUNT()`, `VAR()`, `VARP()`, `STDDEV()`, `STDDEVP()`.

```sql
select
AVG(PRICE) as MEAN_PRICE,
STDDEV(PRICE) as STDDEV_PRICE,
MAX(PRICE) as MAX_PRICE,
MIN(PRICE) as MIN_PRICE,
COUNT(PRICE) as COUNT_PRICE
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
```

## Common Aggregations for Vodafone retrieval results

| Symbol

 | Timestamp

 | MEAN_PRICE

 | STDDEV_PRICE

 | MAX_PRICE

 | MIN_PRICE

 | COUNT_PRICE

 |
| ------ | --------- | ---------- | ------------ | --------- | --------- | ----------- |
| LSE_SAMPLE::VOD

 | 2024-01-04 16:00:00

 | 69.7709393124

 | 4.87753568151

 | 71.0166

   | 0.812

     | 11,925

      |
## Group by Field

Data can be grouped by field such as `TRADE_CURRENCY`

```sql
select
TRADE_CURRENCY,
AVG(PRICE) as MEAN_PRICE,
STDDEV(PRICE) as STDDEV_PRICE,
MAX(PRICE) as MAX_PRICE,
MIN(PRICE) as MIN_PRICE,
COUNT(PRICE) as COUNT_PRICE
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
group by TRADE_CURRENCY
```

## Common Aggregations for Vodafone grouped by Trade Currency retrieval results

| Symbol

          | Timestamp

           | TRADE_CURRENCY

 | MEAN_PRICE

    | STDDEV_PRICE

 | MAX_PRICE

 | MIN_PRICE

   | COUNT_PRICE

 |
| --------------- | ------------------- | -------------- | ------------- | ------------ | --------- | ----------- | ----------- |
| LSE_SAMPLE::VOD

 | 2024-01-04 16:00:00

 | EUR

            | 0.817847457627

 | 0.0025159049878

 | 0.825

     | 0.812

       | 59

          |
| LSE_SAMPLE::VOD

 | 2024-01-04 16:00:00

 | GBX

            | 70.1137871482

  | 0.385602260811

  | 71.0166

   | 68.05

       | 11,866

      |
## Group by Time Bucket

Data can be grouped by time bucket, with the `time_bucket` function, which specifies an interval. e.g. `time_bucket(INTERVAL '5' MINUTE)`.
Intervals can be: `MILLISECOND`, `SECOND`, `MINUTE`, `HOUR`, `DAY` or `WEEK`.

```sql
select
AVG(PRICE) as MEAN_PRICE,
STDDEV(PRICE) as STDDEV_PRICE,
MAX(PRICE) as MAX_PRICE,
MIN(PRICE) as MIN_PRICE,
COUNT(PRICE) as COUNT_PRICE
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
group by time_bucket(INTERVAL '5' MINUTE)
LIMIT 10
```

## Common Aggregations for Vodafone grouped by 5 Minute time bucket retrieval results

| Symbol

          | Timestamp

           | MEAN_PRICE

     | STDDEV_PRICE

   | MAX_PRICE

       | MIN_PRICE

 | COUNT_PRICE

 |
| --------------- | ------------------- | -------------- | -------------- | --------------- | --------- | ----------- |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:05:00

 | 69.5115052632

  | 7.70537091338

  | 70.57

           | 0.8175

    | 323

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:10:00

 | 69.1482768519

  | 9.42987377888

  | 70.54

           | 0.8185

    | 108

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:15:00

 | 68.4568388889

  | 11.5137614925

  | 70.54

           | 0.817

     | 72

          |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:20:00

 | 69.4476636364

  | 7.40070551216

  | 70.3798

         | 0.816

     | 88

          |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:25:00

 | 69.4555013333

  | 8.03271759341

  | 70.45

           | 0.818

     | 75

          |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:30:00

 | 68.0768596774

  | 12.3804045254

  | 70.48

           | 0.817

     | 62

          |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:35:00

 | 65.174885

      | 18.5591156419

  | 70.49

           | 0.816

     | 40

          |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:40:00

 | 70.4912977778

  | 0.0197907059097

 | 70.54

           | 70.4598

   | 45

          |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:45:00

 | 69.6322601449

  | 8.37524346447

   | 70.72

           | 0.8195

    | 138

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:50:00

 | 70.6536

        | 0.0184115275799

 | 70.7

            | 70.62

     | 47

          |
## Group by Record Bucket

Data can be grouped by count of records, with the `ticks_bucket` function, which specifies the number of records to group on `ticks_bucket(1000)`.

```sql
select
AVG(PRICE) as MEAN_PRICE,
STDDEV(PRICE) as STDDEV_PRICE,
MAX(PRICE) as MAX_PRICE,
MIN(PRICE) as MIN_PRICE,
COUNT(PRICE) as COUNT_PRICE
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
group by ticks_bucket(1000)
LIMIT 10
```

## Common Aggregations for Vodafone grouped by 1000 Records retrieval results

| Symbol

          | Timestamp

           | MEAN_PRICE

     | STDDEV_PRICE

    | MAX_PRICE

       | MIN_PRICE

 | COUNT_PRICE

 |
| --------------- | ------------------- | -------------- | --------------- | --------------- | --------- | ----------- |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:50:40.916

 | 69.2407897

     | 9.00384446298

   | 70.72

           | 0.816

     | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-03 09:56:27.519

 | 70.1026542

     | 5.82059741845

   | 71.0166

         | 0.818

     | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-03 11:30:39.417

 | 70.100826

      | 3.80441888802

   | 70.54

           | 0.8165

    | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-03 13:35:26.643

 | 70.1238924

     | 2.19545060204

   | 70.46

           | 0.8175

    | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-03 14:42:02.384

 | 69.8510987

     | 3.79117371178

   | 70.33

           | 0.814

     | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-03 15:48:14.860

 | 69.8859224

     | 2.18726682647

   | 70.11

           | 0.8165

    | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-04 08:00:34.083

 | 69.5407809

     | 2.18618336609

   | 70.74

           | 0.8135

    | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-04 09:31:44.801

 | 69.0322738

     | 7.52460074146

   | 70.74

           | 0.812

     | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-04 12:12:21.302

 | 69.439274

      | 4.86809167691

   | 69.97

           | 0.813

     | 1,000

       |
| LSE_SAMPLE::VOD

 | 2024-01-04 14:30:00.962

 | 69.7273201

     | 5.3607976549

    | 70.55

           | 0.816

     | 1,000

       |
## Generating Bars

Intraday bars can be generated by querying with both

> 
> * `time_bucket` as the `group by`


> * Additional aggregations for `FIRST`, `LAST`, `FIRST_TIME`, `HIGH_TIME`, `LOW_TIME`, `LAST_TIME`

```sql
select
FIRST(PRICE) as first_price,
MAX(PRICE) as high_price,
MIN(PRICE) as low_price,
LAST(PRICE) as last_price,
FIRST_TIME(PRICE) as first_price_time,
HIGH_TIME(PRICE) as high_price_time,
LOW_TIME(PRICE) as low_price_time,
LAST_TIME(PRICE) as last_price_time,
SUM(SIZE) as sum_size,
COUNT(*) as trade_count
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
group by time_bucket(INTERVAL '5' MINUTE)
LIMIT 10
```

## 5 Minute Bars for Vodafone retrieval results

| Symbol

          | Timestamp

               | LAST_PRICE

     | FIRST_PRICE

     | HIGH_PRICE

      | LOW_PRICE

 | SUM_SIZE

    | TRADE_COUNT

 | FIRST_PRICE_TIME

 | HIGH_PRICE_TIME

 | LOW_PRICE_TIME

 | LAST_PRICE_TIME

 |
| --------------- | ----------------------- | -------------- | --------------- | --------------- | --------- | ----------- | ----------- | ---------------- | --------------- | -------------- | --------------- |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:05:00

     | 70.543

         | 70

              | 70.57

           | 0.8175

    | 711,209

     | 323

         | 2024-01-03 08:00:06.232

 | 2024-01-03 08:01:55.576

 | 2024-01-03 08:04:14.972

 | 2024-01-03 08:04:54.729

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:10:00

     | 70.5

           | 70.43

           | 70.54

           | 0.8185

    | 181,979

     | 108

         | 2024-01-03 08:05:00.144

 | 2024-01-03 08:09:05.774

 | 2024-01-03 08:05:23.956

 | 2024-01-03 08:09:54.846

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:15:00

     | 70.22

          | 70.5

            | 70.54

           | 0.817

     | 193,178

     | 72

          | 2024-01-03 08:10:02.391

 | 2024-01-03 08:10:03.089

 | 2024-01-03 08:14:36.021

 | 2024-01-03 08:14:55.376

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:20:00

     | 70.3498

        | 70.21

           | 70.3798

         | 0.816

     | 308,094

     | 88

          | 2024-01-03 08:15:09.168

 | 2024-01-03 08:19:23.245

 | 2024-01-03 08:16:31.755

 | 2024-01-03 08:19:58.251

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:25:00

     | 70.45

          | 70.33

           | 70.45

           | 0.818

     | 59,217

      | 75

          | 2024-01-03 08:20:02.567

 | 2024-01-03 08:24:57.229

 | 2024-01-03 08:22:50.847

 | 2024-01-03 08:24:57.229

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:30:00

     | 70.19

          | 70.43

           | 70.48

           | 0.817

     | 170,563

     | 62

          | 2024-01-03 08:25:09.474

 | 2024-01-03 08:25:31.087

 | 2024-01-03 08:29:13.359

 | 2024-01-03 08:29:40.994

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:35:00

     | 70.49

          | 70.2631

         | 70.49

           | 0.816

     | 111,955

     | 40

          | 2024-01-03 08:30:10.383

 | 2024-01-03 08:34:21.010

 | 2024-01-03 08:30:46.534

 | 2024-01-03 08:34:42.362

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:40:00

     | 70.51

          | 70.4798

         | 70.54

           | 70.4598

   | 152,794

     | 45

          | 2024-01-03 08:35:01.445

 | 2024-01-03 08:39:25.268

 | 2024-01-03 08:38:29.489

 | 2024-01-03 08:39:31.358

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:45:00

     | 70.65

          | 70.55

           | 70.72

           | 0.8195

    | 926,490

     | 138

         | 2024-01-03 08:40:03.746

 | 2024-01-03 08:43:02.677

 | 2024-01-03 08:41:00.810

 | 2024-01-03 08:44:26.284

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:50:00

     | 70.63

          | 70.67

           | 70.7

            | 70.62

     | 130,943

     | 47

          | 2024-01-03 08:45:20.921

 | 2024-01-03 08:46:04.406

 | 2024-01-03 08:47:56.618

 | 2024-01-03 08:49:28.304

 |
## Ranking & Row Numbering

Both the `RANK` and `ROW_NUMBER` aggregates can be used to rank results.  They both use the `over` clause to define the field and sort order to rank over.

```sql
SELECT PRICE,SIZE, RANK() over (order by SIZE desc) as rank_size
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 10
```

## Ranking by Trade Size descending retrieval results

| Symbol

          | Timestamp

               | PRICE

          | SIZE

            | RANK_SIZE

       |
| --------------- | ----------------------- | -------------- | --------------- | --------------- |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:06.232

 | 70

             | 184,613

         | 22

              |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:06.233

 | 70.01

          | 140

             | 9,310

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:06.287

 | 70.01

          | 500

             | 8,145

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:08.113

 | 70.104

         | 2,800

           | 3,992

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:09.380

 | 70.137

         | 49

              | 9,974

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:09.943

 | 70.0981

        | 66,908

          | 60

              |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:09.944

 | 70.1097

        | 214

             | 9,106

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:10.119

 | 70.146

         | 840

             | 7,072

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:10.933

 | 70.132

         | 344

             | 8,376

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:20.863

 | 70.19

          | 4,012

           | 3,250

           |
```sql
select PRICE,SIZE, ROW_NUMBER() over (order by SIZE desc) as rowid
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC'
LIMIT 10
```

## Row Number by Trade Size descending retrieval results

| Symbol

          | Timestamp

               | PRICE

          | SIZE

            | RANK_SIZE

       |
| --------------- | ----------------------- | -------------- | --------------- | --------------- |
| Symbol

          | Timestamp

               | PRICE

          | SIZE

            | ROWID

           |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:45:39.254

 | 70

             | 18,757,000

      | 1

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:46:42.278

 | 70

             | 18,757,000

      | 2

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 16:35:07.353

 | 69.51

          | 15,788,154

      | 3

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:38:17.754

 | 70.5

           | 4,400,000

       | 4

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:49:25.635

 | 70.5

           | 4,400,000

       | 5

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 09:52:50.608

 | 70.5

           | 1,320,000

       | 6

               |
| LSE_SAMPLE::VOD

 | 2024-01-04 08:00:02.764

 | 70.33

          | 922,507

         | 7

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 17:52:28.171

 | 69.502

         | 644,286

         | 8

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 17:46:40.536

 | 70.283

         | 425,511

         | 9

               |
| LSE_SAMPLE::VOD

 | 2024-01-03 17:11:16.706

 | 70.08

          | 419,767

         | 10

              |
Ranking can also be used in a nested query to filter the returned results to the top n per group.

```sql
select * from (
select PRICE,SIZE, TRADE_VENUE, RANK() over (partition by TRADE_VENUE order by SIZE desc) as rank_size
from LSE_SAMPLE.TRD
where symbol_name = 'VOD'
and TIMESTAMP >= '2024-01-03 08:00:00 UTC'
and TIMESTAMP < '2024-01-04 16:00:00 UTC')
where rank_size <= 3
LIMIT 10
```

## Top 3 Trades by Trade Size desc, across each Trade Venue results

| Symbol

          | Timestamp

               | PRICE

          | SIZE

            | TRADE_VENUE

     | RANK_SIZE

 |
| --------------- | ----------------------- | -------------- | --------------- | --------------- | --------- |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:52:45.440

 | 70.6

           | 331,071

         | XLON

            | 3

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:38:17.754

 | 70.5

           | 4,400,000

       | XOFF

            | 3

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:45:39.254

 | 70

             | 18,757,000

      | XOFF

            | 1

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:46:42.278

 | 70

             | 18,757,000

      | XOFF

            | 1

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 10:49:25.635

 | 70.5

           | 4,400,000

       | XOFF

            | 3

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 13:28:29.451

 | 70.39

          | 167,204

         | SINT

            | 2

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 13:55:13.851

 | 70.29

          | 100,000

         | SINT

            | 3

         |
| LSE_SAMPLE::VOD

 | 2024-01-03 16:35:07.353

 | 69.51

          | 15,788,154

      | XLON

            | 1

         |
| LSE_SAMPLE::VOD

 | 2024-01-04 08:00:02.764

 | 70.33

          | 922,507

         | XLON

            | 2

         |
| LSE_SAMPLE::VOD

 | 2024-01-04 14:26:11.598

 | 70.5

           | 174,573

         | SINT

            | 1

         |

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
