# As Of (Prevailing)

## Exact Time Match

Data at a given timestamp can be retrieved by specifying `TIMESTAMP` `=` the specified timestamp.

```sql
SELECT * FROM LSE.TRD
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP = '2024-01-03 08:22:51.848 UTC'
LIMIT 10
```

#### Retrieve Records at specified Timestamp results

| Symbol   | Timestamp               |   PRICE |   SIZE | SYMBOL_NAME   | TICK_TYPE   |   OMDSEQ | EXCH_TIME               |           TRADE_ID | TRADE_TYPE   | TRADE_VENUE   | PUB_VENUE   | TRADE_CURRENCY   |   MMT_MKT_MECH |   MMT_TRD_MODE | MMT_TRANS_CAT   | MMT_NEGOTIATED_IND   | MMT_CROSS_IND   | MMT_MOD_IND   | MMT_BENCHMARK_IND   | MMT_DIVIDEND_IND   | MMT_OFF_BOOK_AUTO_IND   | MMT_PRICE_FORMING_IND   | MMT_ALGO_IND   | MMT_PUB_MODE   | MMT_DEFERRAL_TYPE   | MMT_DUP_IND   | DELETED_TIME   |   TICK_STATUS |
|----------|-------------------------|---------|--------|---------------|-------------|----------|-------------------------|--------------------|--------------|---------------|-------------|------------------|----------------|----------------|-----------------|----------------------|-----------------|---------------|---------------------|--------------------|-------------------------|-------------------------|----------------|----------------|---------------------|---------------|----------------|---------------|
| LSE::VOD | 2024-01-03 08:22:50.847 |   0.818 |    200 | LSE::VOD      | TRD         |        1 | 2024-01-03 08:22:50.763 | 427637175188410480 | OB           | SINT          | ECEU        | EUR              |              4 |              7 |                 |                      |                 |               |                     |                    |                         | P                       |                |                |                     |               |                |             0 |

## Most Recent / Prevailing Value

To return the most recent / prevailing value as of the specified time, additionally define a lookback period  `init_lookback` in seconds.

```sql
SELECT * FROM LSE.TRD t
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP = '2024-01-03 08:22:51.848 UTC'
and t.init_lookback = 86400
LIMIT 10
```

#### Retrieve the most recent trade as of the specified Timestamp results

| Symbol   | Timestamp               |   PRICE |   SIZE | SYMBOL_NAME   | TICK_TYPE   |   OMDSEQ | EXCH_TIME               |           TRADE_ID | TRADE_TYPE   | TRADE_VENUE   | PUB_VENUE   | TRADE_CURRENCY   |   MMT_MKT_MECH |   MMT_TRD_MODE | MMT_TRANS_CAT   | MMT_NEGOTIATED_IND   | MMT_CROSS_IND   | MMT_MOD_IND   | MMT_BENCHMARK_IND   | MMT_DIVIDEND_IND   | MMT_OFF_BOOK_AUTO_IND   | MMT_PRICE_FORMING_IND   | MMT_ALGO_IND   | MMT_PUB_MODE   | MMT_DEFERRAL_TYPE   | MMT_DUP_IND   | DELETED_TIME   |   TICK_STATUS |
|----------|-------------------------|---------|--------|---------------|-------------|----------|-------------------------|--------------------|--------------|---------------|-------------|------------------|----------------|----------------|-----------------|----------------------|-----------------|---------------|---------------------|--------------------|-------------------------|-------------------------|----------------|----------------|---------------------|---------------|----------------|---------------|
| LSE::VOD | 2024-01-03 08:22:51.848 |   0.818 |    200 | LSE::VOD      | TRD         |        1 | 2024-01-03 08:22:50.763 | 427637175188410480 | OB           | SINT          | ECEU        | EUR              |              4 |              7 |                 |                      |                 |               |                     |                    |                         | P                       |                |                |                     |               |                |             0 |

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

#### As of Join, Joining Trades to Prevailing Quotes results

| Symbol          | Timestamp               |   PRICE |    SIZE |   BID_PRICE |   ASK_PRICE | QTE_TIME                |
|-----------------|-------------------------|---------|---------|-------------|-------------|-------------------------|
| LSE_SAMPLE::VOD | 2024-01-03 07:15:10.133 | 69.76   |  40,000 |       69    |       71.82 | 2024-01-03 05:00:07.876 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:06.232 | 70      | 184,613 |       70.01 |       70.19 | 2024-01-03 08:00:06.226 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:06.233 | 70.01   |     140 |       70.01 |       70.19 | 2024-01-03 08:00:06.226 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:06.287 | 70.01   |     500 |       70.01 |       70.19 | 2024-01-03 08:00:06.287 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:08.113 | 70.104  |   2,800 |       70.08 |       70.18 | 2024-01-03 08:00:07.747 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:09.380 | 70.137  |      49 |       70.08 |       70.18 | 2024-01-03 08:00:08.234 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:09.943 | 70.0981 |  66,908 |       70.08 |       70.18 | 2024-01-03 08:00:08.234 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:09.944 | 70.1097 |     214 |       70.08 |       70.18 | 2024-01-03 08:00:08.234 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:10.119 | 70.146  |     840 |       70.08 |       70.18 | 2024-01-03 08:00:08.234 |
| LSE_SAMPLE::VOD | 2024-01-03 08:00:10.933 | 70.132  |     344 |       70.08 |       70.18 | 2024-01-03 08:00:08.234 |

## As Of Join to Prevailing NBBO

For a US Composite database, trades and quotes are consolidated across multiple exchanges, so
the prevailing National Best Bid and Offer is taken from the `NBBO` table rather than a
single venue `QTE` table. The join is otherwise identical - `sametime_as_existing` matches
each trade to the prevailing NBBO record.

```sql
select t.PRICE, t.SIZE, q.BID_PRICE, q.ASK_PRICE
from US_COMP_SAMPLE.TRD t, US_COMP_SAMPLE.NBBO q
where t.symbol_name = 'CSCO' and t.symbol_name = q.symbol_name
and sametime_as_existing(t.timestamp, q.timestamp, 0) = TRUE
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 100
```

## As Of Join on Exchange (Prevailing Quote per Venue)

Because the US Composite contains trades and quotes across multiple exchanges, an unqualified
join can match a trade to a prevailing quote from a *different* venue. Adding `USING (EXCHANGE)`
to the join constrains the match so that each trade is joined to the prevailing quote from the
*same exchange* as the trade.

```sql
select t.PRICE, t.SIZE, EXCHANGE, q.BID_PRICE, q.ASK_PRICE, q.EXCHANGE
from US_COMP_SAMPLE.TRD t join US_COMP_SAMPLE.QTE q USING (EXCHANGE)
where t.symbol_name = 'CSCO' and t.symbol_name = q.symbol_name
and sametime_as_existing(t.timestamp, q.timestamp, 0) = TRUE
and TIMESTAMP >= '2024-01-03 09:30:00 America/New_York'
and TIMESTAMP < '2024-01-04 16:00:00 America/New_York'
limit 100
```

## As Of Join Across Multiple Tables

Several tables can be joined in a single query by listing them together and matching each with
its own `sametime_as_existing` condition. The example below joins trades to two quote tables:
`NBBO` provides the standard National Best Bid and Offer, while `NBBO_COMP` provides an
improved NBBO that additionally includes the Best Odd Lot Order (BOLO), allowing the two to be
compared side by side at the time of each trade.

```sql
select t.PRICE as PRICE, t.SIZE as SIZE,
n.TIMESTAMP as NBBO_TIME, n.BID_PRICE as BID_PRICE, n.ASK_PRICE as ASK_PRICE, n.BID_SIZE as BID_SIZE, n.ASK_SIZE as ASK_SIZE,
c.TIMESTAMP as NBBO_COMP_TIME, c.BID_PRICE as BID_PRICE_COMP, c.ASK_PRICE as ASK_PRICE_COMP, c.BID_SIZE as BID_SIZE_COMP, c.ASK_SIZE as ASK_SIZE_COMP
from US_COMP.TRD t, US_COMP.NBBO n, US_COMP.NBBO_COMP c
where t.SYMBOL_NAME='CSCO' and t.SYMBOL_NAME = n.SYMBOL_NAME and t.SYMBOL_NAME = c.SYMBOL_NAME
and sametime_as_existing(t.timestamp, n.timestamp, 0) = TRUE
and sametime_as_existing(t.timestamp, c.timestamp, 0) = TRUE
and TIMESTAMP >= '2026-07-23 09:30:00 America/New_York'
and TIMESTAMP < '2026-07-23 16:00:00 America/New_York'
limit 10
```
