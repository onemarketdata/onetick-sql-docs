# Book Depth

A series of simple examples are provided showing how to retrieve book depth from [OneTick Cloud](https://www.onetick.com/cloud-services).

## Book Depth Events

Book depth events are retrieved by specifying either the `PRL` or `PRL_FULL` table, depending on whether Market by Level (MBL) or Market by Order (MBO) data is available.

* `PRL` Table - Market by Level (MBL)
* `PRL_FULL` Table - Market by Order (MBO)

Typically book depth data is analysed using orderbook processing syntax, while the order update message can be retrieved with a simple select from the database and table.

```sql
SELECT * FROM LSE_SAMPLE.PRL_FULL
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

#### Book Depth Event Retrieval Results

| Symbol          | Timestamp               |   PRICE |   SIZE | SYMBOL_NAME     | TICK_TYPE   |   OMDSEQ | DELETED_TIME   |   TICK_STATUS |   BUY_SELL_FLAG | UPDATE_TYPE   |           ORDER_ID | PART_ID   | ORDER_TYPE   | RECORD_TYPE   |
|-----------------|-------------------------|---------|--------|-----------------|-------------|----------|----------------|---------------|-----------------|---------------|--------------------|-----------|--------------|---------------|
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |    0    |      0 | LSE_SAMPLE::VOD | PRL_FULL    |        0 |                |             0 |               0 |               |                    |           |              | Z             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   69    |    600 | LSE_SAMPLE::VOD | PRL_FULL    |        0 |                |             0 |               0 | A             | 233244094926020495 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   69    |  5,800 | LSE_SAMPLE::VOD | PRL_FULL    |        1 |                |             0 |               0 | A             | 233244094926209592 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   67.2  | 50,000 | LSE_SAMPLE::VOD | PRL_FULL    |        2 |                |             0 |               0 | A             | 232452446553925487 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   67    | 10,000 | LSE_SAMPLE::VOD | PRL_FULL    |        3 |                |             0 |               0 | A             | 233244094925636785 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   66.94 |  7,300 | LSE_SAMPLE::VOD | PRL_FULL    |        4 |                |             0 |               0 | A             | 233244094925636407 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   66.66 | 20,000 | LSE_SAMPLE::VOD | PRL_FULL    |        5 |                |             0 |               0 | A             | 233244094925845524 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   66.34 |  7,360 | LSE_SAMPLE::VOD | PRL_FULL    |        6 |                |             0 |               0 | A             | 233244094925636400 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   66    |  3,000 | LSE_SAMPLE::VOD | PRL_FULL    |        7 |                |             0 |               0 | A             | 233244094925636417 |           | L            | R             |
| LSE_SAMPLE::VOD | 2024-01-03 05:00:07.876 |   65    | 70,000 | LSE_SAMPLE::VOD | PRL_FULL    |        8 |                |             0 |               0 | A             | 233244094925636406 |           | L            | R             |

## Order Book Price Level Updates

Updates to order books are retrieved by using the `OB_SNAPSHOT` syntax, specifying `IS_RUNNING_AGGR='TRUE'` and `SHOW_ONLY_CHANGES='TRUE'`,
along with defining the table which is either  `PRL` or `PRL_FULL` , depending on whether Market by Level (MBL) or Market by Order (MBO) data is available.
In this case the `SYMBOL_NAME` should specify both the database and symbol seperated by `::`.   e.g. `LSE_SAMPLE::HSBA`
An initial state of the book is returned, following by deltas if `SHOW_ONLY_CHANGES='TRUE'`.  Alternatively each new book can be output if `SHOW_ONLY_CHANGES='FALSE'`.

When using the `OB_SNAPSHOT` syntax, data is returned with each row representing a price level identified with the `LEVEL` field, and side identified with the `BUY_SELL_FLAG` field.
`PRICE` and `SIZE` fields present the current price and remaining size or quantity of the price level.

Alternative representations are available using:

> * `OB_SNAPSHOT` for each record representing either an order, or a price level and side.
> * `OB_SNAPSHOT_WIDE` for each record representing a price level with both Bid and Ask.
> * `OB_SNAPSHOT_FLAT` for each record represnting every level in the book

The returned results can be further filtered by additionally specifying:

> * Maximum Number of Price Levels to return through `MAX_LEVELS`
> * Maximum Depth in Cumulative Size to return through `MAX_DEPTH_SHARES`
> * Maximum Depth in % Price Skew from BBO to return through `MAX_DEPTH_FOR_PRICE`
> * Maximum Depth in Absolute Spread through `MAX_SPREAD`
```sql
SELECT * FROM OTQ_CHAIN."OB_SNAPSHOT(IS_RUNNING_AGGR='TRUE',SHOW_ONLY_CHANGES='TRUE');PRL_FULL"
WHERE SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

#### Order Book Depth Updates Retrieval Results

| Symbol           | Timestamp           |   PRICE |   SIZE | SYMBOL_NAME      | TICK_TYPE   | UPDATE_TIME             |   LEVEL |   BUY_SELL_FLAG |
|------------------|---------------------|---------|--------|------------------|-------------|-------------------------|---------|-----------------|
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   640   |  7,035 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:47:11.727 |       1 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   642   |  2,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:35:12.759 |       2 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   650   | 38,600 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 17:10:02.567 |       3 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   655   | 16,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 17:30:00.059 |       4 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   659.8 |    800 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 07:50:03.005 |       5 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   660   | 16,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 17:30:00.059 |       6 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   665   | 32,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:50:00.058 |       7 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   669.3 | 12,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 07:50:03.004 |       8 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   680   | 15,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:40:15.570 |       9 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   682   |  4,800 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 07:50:03.004 |      10 |               1 |

## Order Book L2 Depth At Time

For Level 2 or Level 3 Book depth stored in `PRL` or `PRL_FULL` tables, the state of the book can be retrieved at a point in time by specifying:

* `SYMBOL_NAME` with `[Database]::[Symbol]` Syntax.  e.g. `LSE_SAMPLE::HSBA`
* `OB_SNAPSHOT()` or `OB_SNAPSHOT_WIDE()` or `OB_SNAPSHOT_FLAT` depending on the required ouptut format
* `TIMESTAMP` field set to equals `=` the required point in time.

```sql
select * from OTQ_CHAIN."OB_SNAPSHOT();PRL_FULL"
where SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP = '2024-01-03 12:00:00 UTC'
limit 1000
```

## Order Book L3 Depth At Time

For Level 3 Book depth stored in `PRL_FULL` tables, the details state of the book can be retrieved by book updates can be retrieved at an order by order basis by specifying:

* `OB_SNAPSHOT()` or `OB_SNAPSHOT_WIDE()` or `OB_SNAPSHOT_FLAT` depending on the required ouptut format
* `SHOW_FULL_DETAIL='TRUE'`  inside the OB syntax.
* `SYMBOL_NAME` with `[Database]::[Symbol]` Syntax.  e.g. `LSE_SAMPLE::HSBA`
* `TIMESTAMP` field set to equals `=` the required point in time.

In this case the ORDER_ID field will also be output in the results.

```sql
select * from OTQ_CHAIN."OB_SNAPSHOT(SHOW_FULL_DETAIL='TRUE');PRL_FULL"
where SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP = '2024-01-03 12:00:00 UTC'
limit 1000
```

## Order Book L3 Depth Updates

For Level 3 Book depth stored in `PRL_FULL` tables, book updates can be retrieved at an order by order basis by specifying `SHOW_FULL_DETAIL='TRUE'`.
In this case the ORDER_ID field will also be output in the results.

```sql
SELECT * FROM OTQ_CHAIN."OB_SNAPSHOT(IS_RUNNING_AGGR='TRUE',SHOW_ONLY_CHANGES='TRUE',SHOW_FULL_DETAIL='TRUE');PRL_FULL"
WHERE SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

#### Order Book Full Depth Updates Retrieval Results

| Symbol           | Timestamp           |   PRICE |   SIZE | SYMBOL_NAME      | TICK_TYPE   |   OMDSEQ | DELETED_TIME            |   TICK_STATUS | UPDATE_TIME             |   LEVEL |   BUY_SELL_FLAG | UPDATE_TYPE   |           ORDER_ID | PART_ID   | ORDER_TYPE   | RECORD_TYPE   |
|------------------|---------------------|---------|--------|------------------|-------------|----------|-------------------------|---------------|-------------------------|---------|-----------------|---------------|--------------------|-----------|--------------|---------------|
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   640   |  7,035 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 05:00:09.192 |            16 | 2024-01-02 05:00:09.192 |       1 |               1 | A             | 225960944935921699 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   642   |  2,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 07:50:03.010 |            16 | 2024-01-02 07:50:03.010 |       2 |               1 | A             | 233244109958022143 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   650   |  2,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 05:00:09.192 |            16 | 2024-01-02 05:00:09.192 |       3 |               1 | A             | 232452461586102765 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   650   | 36,600 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 07:50:03.006 |            16 | 2024-01-02 07:50:03.006 |       3 |               1 | A             | 233244109958022117 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   655   | 16,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 07:50:03.004 |            16 | 2024-01-02 07:50:03.004 |       4 |               1 | A             | 233244109958022100 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   659.8 |    800 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 07:50:03.005 |            16 | 2024-01-02 07:50:03.005 |       5 |               1 | A             | 233244109958022109 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   660   | 16,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 07:50:03.005 |            16 | 2024-01-02 07:50:03.005 |       6 |               1 | A             | 233244109958022111 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   665   | 16,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 05:00:09.192 |            16 | 2024-01-02 05:00:09.192 |       7 |               1 | A             | 228177560377546089 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   665   | 16,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 07:50:03.007 |            16 | 2024-01-02 07:50:03.007 |       7 |               1 | A             | 233244109958022126 |           | L            | R             |
| LSE_SAMPLE::HSBA | 2024-01-03 00:00:00 |   669.3 | 12,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       21 | 2024-01-02 07:50:03.004 |            16 | 2024-01-02 07:50:03.004 |       8 |               1 | A             | 233244109958022099 |           | L            | R             |

## Order Book Depth Bars

Order Book state can be returned at periodic intervals generating book depth bars, by specifying the `BUCKET_INTERVAL` parameter, whcih represents a time in seconds.
Milliseconds are supported by specifying a decimal (e.g. 0.1)

As before, alternative representations are available using:

> * `OB_SNAPSHOT` for each record representing either an order, or a price level and side.
> * `OB_SNAPSHOT_WIDE` for each record representing a price level with both Bid and Ask.
> * `OB_SNAPSHOT_FLAT` for each record represnting every level in the book
```sql
SELECT * FROM OTQ_CHAIN."OB_SNAPSHOT(BUCKET_INTERVAL='60',MAX_LEVELS='5');PRL_FULL"
WHERE SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

#### Order Book 1 Minute Bars to 5 Levels of Depth Retrieval Results

| Symbol           | Timestamp           |   PRICE |   SIZE | SYMBOL_NAME      | TICK_TYPE   | UPDATE_TIME             |   LEVEL |   BUY_SELL_FLAG |
|------------------|---------------------|---------|--------|------------------|-------------|-------------------------|---------|-----------------|
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   640   |  7,035 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:47:11.727 |       1 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   642   |  2,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:35:12.759 |       2 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   650   | 38,600 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 17:10:02.567 |       3 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   655   | 16,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 17:30:00.059 |       4 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   659.8 |    800 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 07:50:03.005 |       5 |               1 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   610   |     80 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:40:00.145 |       1 |               0 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   600   |    500 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 17:30:00.058 |       2 |               0 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   580   |     50 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 17:30:00.059 |       3 |               0 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   575   |  1,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 07:50:03.003 |       4 |               0 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |   500   |  2,000 | LSE_SAMPLE::HSBA | PRL_FULL    | 2024-01-02 16:40:00.145 |       5 |               0 |
```sql
SELECT * FROM OTQ_CHAIN."OB_SNAPSHOT_WIDE(BUCKET_INTERVAL='60',MAX_LEVELS='5');PRL_FULL"
WHERE SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

#### Order Book Wide (Bid & Ask on same row) 1 Minute Bars to 5 Levels of Depth Retrieval Results

| Symbol           | Timestamp           |   BID_PRICE |   ASK_PRICE |   BID_SIZE |   ASK_SIZE | SYMBOL_NAME      | TICK_TYPE   |   LEVEL | BID_UPDATE_TIME         | ASK_UPDATE_TIME         |
|------------------|---------------------|-------------|-------------|------------|------------|------------------|-------------|---------|-------------------------|-------------------------|
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |         610 |       640   |         80 |      7,035 | LSE_SAMPLE::HSBA | PRL_FULL    |       1 | 2024-01-02 16:40:00.145 | 2024-01-02 16:47:11.727 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |         600 |       642   |        500 |      2,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       2 | 2024-01-02 17:30:00.058 | 2024-01-02 16:35:12.759 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |         580 |       650   |         50 |     38,600 | LSE_SAMPLE::HSBA | PRL_FULL    |       3 | 2024-01-02 17:30:00.059 | 2024-01-02 17:10:02.567 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |         575 |       655   |      1,000 |     16,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       4 | 2024-01-02 07:50:03.003 | 2024-01-02 17:30:00.059 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |         500 |       659.8 |      2,000 |        800 | LSE_SAMPLE::HSBA | PRL_FULL    |       5 | 2024-01-02 16:40:00.145 | 2024-01-02 07:50:03.005 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:02:00 |         610 |       640   |         80 |      7,035 | LSE_SAMPLE::HSBA | PRL_FULL    |       1 | 2024-01-02 16:40:00.145 | 2024-01-02 16:47:11.727 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:02:00 |         600 |       642   |        500 |      2,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       2 | 2024-01-02 17:30:00.058 | 2024-01-02 16:35:12.759 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:02:00 |         580 |       650   |         50 |     38,600 | LSE_SAMPLE::HSBA | PRL_FULL    |       3 | 2024-01-02 17:30:00.059 | 2024-01-02 17:10:02.567 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:02:00 |         575 |       655   |      1,000 |     16,000 | LSE_SAMPLE::HSBA | PRL_FULL    |       4 | 2024-01-02 07:50:03.003 | 2024-01-02 17:30:00.059 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:02:00 |         500 |       659.8 |      2,000 |        800 | LSE_SAMPLE::HSBA | PRL_FULL    |       5 | 2024-01-02 16:40:00.145 | 2024-01-02 07:50:03.005 |
```sql
SELECT * FROM OTQ_CHAIN."OB_SNAPSHOT_FLAT(BUCKET_INTERVAL='60',MAX_LEVELS='5');PRL_FULL"
WHERE SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

#### Order Book Flat (All Levels on same row) 1 Minute Bars to 5 Levels of Depth Retrieval Results

| Symbol           | Timestamp           | SYMBOL_NAME      | TICK_TYPE   |   BID_PRICE1 | BID_UPDATE_TIME1        |   BID_SIZE1 |   ASK_PRICE1 | ASK_UPDATE_TIME1        |   ASK_SIZE1 |   BID_PRICE2 | BID_UPDATE_TIME2        |   BID_SIZE2 |   ASK_PRICE2 | ASK_UPDATE_TIME2        |   ASK_SIZE2 |   BID_PRICE3 | BID_UPDATE_TIME3        |   BID_SIZE3 |   ASK_PRICE3 | ASK_UPDATE_TIME3        |   ASK_SIZE3 |   BID_PRICE4 | BID_UPDATE_TIME4        |   BID_SIZE4 |   ASK_PRICE4 | ASK_UPDATE_TIME4        |   ASK_SIZE4 |   BID_PRICE5 | BID_UPDATE_TIME5        |   BID_SIZE5 |   ASK_PRICE5 | ASK_UPDATE_TIME5        |   ASK_SIZE5 |
|------------------|---------------------|------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|--------------|-------------------------|-------------|
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:02:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:03:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:04:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:05:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:06:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:07:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:08:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:09:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:10:00 | LSE_SAMPLE::HSBA | PRL_FULL    |          610 | 2024-01-02 16:40:00.145 |          80 |          640 | 2024-01-02 16:47:11.727 |       7,035 |          600 | 2024-01-02 17:30:00.058 |         500 |          642 | 2024-01-02 16:35:12.759 |       2,000 |          580 | 2024-01-02 17:30:00.059 |          50 |          650 | 2024-01-02 17:10:02.567 |      38,600 |          575 | 2024-01-02 07:50:03.003 |       1,000 |          655 | 2024-01-02 17:30:00.059 |      16,000 |          500 | 2024-01-02 16:40:00.145 |       2,000 |        659.8 | 2024-01-02 07:50:03.005 |         800 |

## Book Depth Statistics

Statistics on book depth can be retrieved by using the `OB_SUMMARY` syntax, and specifying a book filter

> * Maximum Number of Price Levels through `MAX_LEVELS`
> * Maximum Depth in Cumulative Size  through `MAX_DEPTH_SHARES`
> * Maximum Depth in % Price Skew from BBO through `MAX_DEPTH_FOR_PRICE`
> * Maximum Depth in Absolute Spread through `MAX_SPREAD`

Book depth statistics return:

> * `BEST_BID_PRICE` and `BEST_ASK_PRICE`
> * Cumulative `BID_SIZE` and `ASK_SIZE`
> * `NUM_ASK_LEVELS` and `NUM_BID_LEVELS`
> * `WORST_BID_PRICE` and `WORST_ASK_PRICE`
> * `BID_VWAP` and `ASK_VWAP`
```sql
SELECT * FROM OTQ_CHAIN."OB_SUMMARY(BUCKET_INTERVAL='60',MAX_DEPTH_SHARES='1000');PRL_FULL"
WHERE SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
```

#### Book Depth Statistics by minute to trade a quantity of 1000 Retrieval Results

| Symbol           | Timestamp           |   BID_SIZE |   ASK_SIZE | SYMBOL_NAME      | TICK_TYPE   |   ASK_VWAP |   BID_VWAP |   BEST_ASK_PRICE |   BEST_BID_PRICE |   WORST_ASK_PRICE |   WORST_BID_PRICE |   NUM_ASK_LEVELS |   NUM_BID_LEVELS |
|------------------|---------------------|------------|------------|------------------|-------------|------------|------------|------------------|------------------|-------------------|-------------------|------------------|------------------|
| LSE_SAMPLE::HSBA | 2024-01-03 00:01:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:02:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:03:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:04:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:05:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:06:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:07:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:08:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:09:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |
| LSE_SAMPLE::HSBA | 2024-01-03 00:10:00 |      1,000 |      1,000 | LSE_SAMPLE::HSBA | PRL_FULL    |        640 |     590.55 |              640 |              610 |               640 |               575 |                1 |                4 |

## Book Depth at Time Limited to Specific Levels

A book can be retrieved filtered to a specified number of levels by specifying: `MAX_LEVELS`
If this not specified, all levels of the book are returned (assuming no other filters).

```sql
select * from OTQ_CHAIN."OB_SNAPSHOT(MAX_LEVELS='5');PRL_FULL"
where SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP = '2024-01-03 12:00:00 UTC'
```

## Book Depth at Time Limited to a % Skew from Best

A book can be retrieved filtered to a specified % Skew from Best by specifying: `MAX_DEPTH_FOR_PRICE`
If this not specified, all levels of the book are returned (assuming no other filters).
Note:  0.005 = 0.5%

```sql
select * from OTQ_CHAIN."OB_SNAPSHOT(MAX_DEPTH_FOR_PRICE='0.005');PRL_FULL"
where SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP = '2024-01-03 12:00:00 UTC'
limit 1000
```

## Book Depth at Time Limited to an Accumulated Size

A book can be retrieved filtered to a specified Accumulated Size by specifying: `MAX_DEPTH_SHARES`
If this not specified, all levels of the book are returned (assuming no other filters).

```sql
select * from OTQ_CHAIN."OB_SNAPSHOT(MAX_DEPTH_SHARES='10000');PRL_FULL"
where SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP = '2024-01-03 12:00:00 UTC'
limit 1000
```

## Book Depth at Time Limited to a defined Spread

A book can be retrieved filtered to a specified Absolute Spread by specifying: `MAX_SPREAD`
If this not specified, all levels of the book are returned (assuming no other filters).

```sql
select * from OTQ_CHAIN."OB_SNAPSHOT(SHOW_FULL_DETAIL='TRUE',MAX_SPREAD='1');PRL_FULL"
where SYMBOL_NAME = 'LSE_SAMPLE::HSBA'
and TIMESTAMP = '2024-01-03 12:00:00 UTC'
limit 1000
```
