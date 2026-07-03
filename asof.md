# As Of (Prevailing)

## Exact Time Match

Data at a given timestamp can be retrieved by specifying `TIMESTAMP` `=` the specified timestamp.

```sql
SELECT * FROM LSE.TRD
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP = '2024-01-03 08:22:51.848 UTC'
LIMIT 10
```

## Retrieve Records at specified Timestamp results

| Symbol

      | Timestamp

               | PRICE

                   | SIZE

                    | SYMBOL_NAME

             | TICK_TYPE

      | OMDSEQ

       | EXCH_TIME

    | TRADE_ID

      | TRADE_TYPE

         | TRADE_VENUE

   | PUB_VENUE

   | TRADE_CURRENCY

    | MMT_MKT_MECH

     | MMT_TRD_MODE

          | MMT_TRANS_CAT

         | MMT_NEGOTIATED_IND

 | MMT_CROSS_IND

 | MMT_MOD_IND

       | MMT_BENCHMARK_IND

 | MMT_DIVIDEND_IND

 | MMT_OFF_BOOK_AUTO_IND

 | MMT_PRICE_FORMING_IND

 | MMT_ALGO_IND

 | MMT_PUB_MODE

 | MMT_DEFERRAL_TYPE

 | MMT_DUP_IND

 | DELETED_TIME

 | TICK_STATUS

 |
| --------------- | ----------------------- | -------------- | --------------- | --------------- | --------- | ----------- | ----------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | -------------- | ------------ | ------------ | ------------- | ------------------ | ------------- | ----------- | ----------------- | ---------------- | --------------------- | --------------------- | ------------ | ------------ | ----------------- | ----------- | ------------ | ----------- |  |  |  |  |  |  |  |
| LSE::VOD

        | 2024-01-03 08:22:50.847

 | 0.818

          | 200

             | LSE::VOD

        | TRD

       | 1

           | 2024-01-03 08:22:50.763

 | 427637175188410480

      | OB

                      | SINT

                    | ECEU

                    | EUR

            | 4

            | 7

            | 
*               | 
*                    | 
*               | 
*             | 
*                   | 
*                  |                       | P

                     | 
*              | 
*              | 
*                   | 
*             |              | 0

           |
## Most Recent / Prevailing Value

To return the most recent / prevailing value as of the specified time, additionally define a lookback period  `init_lookback` in seconds.

```sql
SELECT * FROM LSE.TRD t
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP = '2024-01-03 08:22:51.848 UTC'
and t.init_lookback = 86400
LIMIT 10
```

## Retrieve the most recent trade as of the specified Timestamp results

| Symbol

          | Timestamp

               | PRICE

          | SIZE

            | SYMBOL_NAME

     | TICK_TYPE

 | OMDSEQ

      | EXCH_TIME

               | TRADE_ID

                | TRADE_TYPE

              | TRADE_VENUE

             | PUB_VENUE

               | TRADE_CURRENCY

 | MMT_MKT_MECH

 | MMT_TRD_MODE

 | MMT_TRANS_CAT

 | MMT_NEGOTIATED_IND

 | MMT_CROSS_IND

 | MMT_MOD_IND

 | MMT_BENCHMARK_IND

 | MMT_DIVIDEND_IND

 | MMT_OFF_BOOK_AUTO_IND

 | MMT_PRICE_FORMING_IND

 | MMT_ALGO_IND

 | MMT_PUB_MODE

 | MMT_DEFERRAL_TYPE

 | MMT_DUP_IND

 | DELETED_TIME

 | TICK_STATUS

 |
| --------------- | ----------------------- | -------------- | --------------- | --------------- | --------- | ----------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | -------------- | ------------ | ------------ | ------------- | ------------------ | ------------- | ----------- | ----------------- | ---------------- | --------------------- | --------------------- | ------------ | ------------ | ----------------- | ----------- | ------------ | ----------- |
| LSE::VOD

        | 2024-01-03 08:22:51.848

 | 0.818

          | 200

             | LSE::VOD

        | TRD

       | 1

           | 2024-01-03 08:22:50.763

 | 427637175188410480

      | OB

                      | SINT

                    | ECEU

                    | EUR

            | 4

            | 7

            | 
*               | 
*                    | 
*               | 
*             | 
*                   | 
*                  |                       | P

                     | 
*              | 
*              | 
*                   | 
*             |              | 0

           |
## As Of Join

To retrieve trades joined to prevailing quotes an `asof` join is specified using the `sametime_as_existing` function, specifying the trade timestamp as the leading source, followed by the quote timestamp.

```sql
SELECT t.PRICE as PRICE,t.SIZE as SIZE,
q.TIMESTAMP QTE_TIME, q.BID_PRICE as BID_PRICE, q.ASK_PRICE as ASK_PRICE
FROM LSE_SAMPLE.TRD t, LSE_SAMPLE.QTE q
WHERE t.SYMBOL_NAME='VOD' and q.SYMBOL_NAME='VOD'
and sametime_as_existing(t.timestamp, q.timestamp, 0) = TRUE
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

## As of Join, Joining Trades to Prevailing Quotes results

| Symbol

          | Timestamp

               | PRICE

          | SIZE

            | BID_PRICE

       | ASK_PRICE

 | QTE_TIME

    |
| --------------- | ----------------------- | -------------- | --------------- | --------------- | --------- | ----------- |
| LSE_SAMPLE::VOD

 | 2024-01-03 07:15:10.133

 | 69.76

          | 40,000

          | 69

              | 71.82

     | 2024-01-03 05:00:07.876

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:06.232

 | 70

             | 184,613

         | 70.01

           | 70.19

     | 2024-01-03 08:00:06.226

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:06.233

 | 70.01

          | 140

             | 70.01

           | 70.19

     | 2024-01-03 08:00:06.226

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:06.287

 | 70.01

          | 500

             | 70.01

           | 70.19

     | 2024-01-03 08:00:06.287

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:08.113

 | 70.104

         | 2,800

           | 70.08

           | 70.18

     | 2024-01-03 08:00:07.747

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:09.380

 | 70.137

         | 49

              | 70.08

           | 70.18

     | 2024-01-03 08:00:08.234

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:09.943

 | 70.0981

        | 66,908

          | 70.08

           | 70.18

     | 2024-01-03 08:00:08.234

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:09.944

 | 70.1097

        | 214

             | 70.08

           | 70.18

     | 2024-01-03 08:00:08.234

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:10.119

 | 70.146

         | 840

             | 70.08

           | 70.18

     | 2024-01-03 08:00:08.234

 |
| LSE_SAMPLE::VOD

 | 2024-01-03 08:00:10.933

 | 70.132

         | 344

             | 70.08

           | 70.18

     | 2024-01-03 08:00:08.234

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
