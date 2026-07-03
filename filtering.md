# Filtering

A series of simple examples are provided showing how to filter results using the `WHERE` clause.

## Mandatory Filters

SQL requests must always specify


* Symbol or Symbols using the field `SYMBOL_NAME`


* Time Range using field `TIMESTAMP` with `>=` and `<` or Time equal to, using `TIMESTAMP =`

```sql
SELECT * FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

```sql
SELECT * FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP = '2024-01-03 14:40:05.771572791 UTC'
LIMIT 10
```

## Retrieving Multiple Symbols

Multiple symbols can be retrieved through specifying `IN` or `LIKE`.

```sql
SELECT * FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME in ('CSCO','MSFT')
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

```sql
SELECT * FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME LIKE 'BA%'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

## Additional Filter Fields

Additional filters can be added by adding additional `and` sections to the `where` clause.

```sql
SELECT * FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and SIZE > 10          -- Trade Size Greater than 10
and EXCHANGE = 'N'     -- Exchange is NYSE
LIMIT 10
```

## Filtering on Specific Times across Multiple Days

The `MATCHES_TIME_FILTER` function provides a convenient method of filtering for specific time ranges across a longer time window.

```sql
select PRICE,SIZE,COND,EXCHANGE FROM US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-05 00:00:00 UTC'
and MATCHES_TIME_FILTER('09:35:00.000','09:40:00.000','America/New_York') = TRUE
limit 10
```

## Returning a Specific Time Period across Multiple Days Example Results

|  | TIMESTAMP

 | PRICE

 | SIZE

 | COND

 | EXCHANGE

 |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ---------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | -------------- | ------------------ | ----------------------- | ------------- | ------------------ | ----------------------- | ----------- | ----------------- | ----------------------- | --------------------- | --------------------- | ----------------------- | ------------ | ----------------- | ----------------------- | ------------ | ----------- | ----------------------- | --------- | ---------- | ----------------------- | --------- |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0

                       | 2024-01-03 14:35:00.095922915

                                                                                                     | 50.01

                                   | 2

               | @F I

             | Q

                       |
| 1

                       | 2024-01-03 14:35:00.095924736

                                                                                                     | 50.01

                                   | 100

             | @F

               | Q

                       |
| 2

                       | 2024-01-03 14:35:00.095928121

                                                                                                     | 50.01

                                   | 8

               | @F I

             | Q

                       |
| 3

                       | 2024-01-03 14:35:00.096010551

                                                                                                     | 50.01

                                   | 92

              | @  I

             | Q

                       |
| 4

                       | 2024-01-03 14:35:00.096012722

                                                                                                     | 50.01

                                   | 32

              | @F I

             | U

                       |
| 5

                       | 2024-01-03 14:35:00.096052609

                                                                                                     | 50.01

                                   | 2

               | @  I

             | Q

                       |
| 6

                       | 2024-01-03 14:35:00.096054420

                                                                                                     | 50.01

                                   | 92

              | @  I

             | Q

                       |
| 7

                       | 2024-01-03 14:35:00.096058266

                                                                                                     | 50.01

                                   | 1

               | @F I

             | P

                       |
| 8

                       | 2024-01-03 14:35:00.096063675

                                                                                                     | 50.01

                                   | 8

               | @F I

             | U

                       |
| 9

                       | 2024-01-03 14:35:00.096269064

                                                                                                     | 50.01

                                   | 58

              | @F I

             | U

                       |
## Filtering for Trade Conditions

US Equity Trade Conditions are specified in the `COND` field as trade condition characters, where multiple trade conditions can be specified for a given trade.
Trades can be filtered for a single trade condition using `LIKE`.

```sql
select PRICE,SIZE, COND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and COND LIKE '%6%'
```

## Filtering with LIKE on Specific Trade Conditions Example Results

|                         | Timestamp

                                                                                                                         | PRICE

                                   | SIZE

            | COND

             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ---------------- |
| 0

                       | 2024-01-03 16:00:00.156949212

                                                                                                     | 50.51

                                   | 3994768

         | @6 X

             |
To filter on any of multiple trade conditions the `IS_CHARACTER_PRESENT` can be used, with `= TRUE` for inclusion, and `= FALSE` for exclusion.

```sql
select PRICE,SIZE, COND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and IS_CHARACTER_PRESENT(COND,'IBCGHLMNPQRVWZ479') = TRUE
limit 5
```

## Filtering on Specific Trade Conditions Example Results

|                         | Timestamp

                                                                                                                         | PRICE

                                   | SIZE

            | COND

             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ---------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | @FTI

             |
| 1

                       | 2024-01-03 00:00:40.413540299

                                                                                                     | 50.57

                                   | 33

              | @FTI

             |
| 2

                       | 2024-01-03 00:03:34.026455739

                                                                                                     | 50.52

                                   | 26

              | @ TI

             |
| 3

                       | 2024-01-03 00:03:34.026459421

                                                                                                     | 50.49

                                   | 3

               | @ TI

             |
| 4

                       | 2024-01-03 00:05:06.042217019

                                                                                                     | 50.4896

                                 | 1

               | @ TI

             |
```sql
select PRICE,SIZE, COND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and IS_CHARACTER_PRESENT(COND,'IBCGHLMNPQRVWZ479') = FALSE
limit 5
```

## Filtering on Excluding Specific Trade Conditions Example Results

|                         | Timestamp

                                                                                                                         | PRICE

                                   | SIZE

            | COND

             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ---------------- |
| 0

                       | 2024-01-03 00:03:34.026458843

                                                                                                     | 50.51

                                   | 123

             | @ T

              |
| 1

                       | 2024-01-03 00:14:14.187418230

                                                                                                     | 50.49

                                   | 100

             | @ T

              |
| 2

                       | 2024-01-03 00:47:09.486020152

                                                                                                     | 50.49

                                   | 100

             | @FT

              |
| 3

                       | 2024-01-03 00:47:58.373425468

                                                                                                     | 50.48

                                   | 200

             | @ T

              |
| 4

                       | 2024-01-03 00:50:55.662956337

                                                                                                     | 50.47

                                   | 100

             | @ T

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
