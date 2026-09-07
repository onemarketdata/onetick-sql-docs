# Trading Hours & Holidays

Both Trading Hours and historic holidays are recorded in the database `OQD_MKTCAL`.
Data is divided between Equity Markets and Futures products given that different futures products on the same venue may have different trading hours.

## Equity Market Holiday History

Equity Market Holidays are retrieved by setting the `SYMBOL_NAME` to the required database prefixed with `CLOUD_DB_`.  The Time range should cover the required history of holiday events,
while the `ACTIVITY_NAME` should include `HOLIDAY`.  The results will include the fields `TIME_ZONE`, `START_TIME`, `END_TIME`, `START_DATE` and `END_DATE`.

```sql
SELECT * FROM OQD_MKTCAL.MKTCAL
WHERE SYMBOL_NAME = 'CLOUD_DB_LSE'
and TIMESTAMP >= '2023-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and ACTIVITY_NAME like '%HOLIDAY%'
LIMIT 10
```

#### Holiday History for LSE Results

| Symbol                   | Timestamp           | SYMBOL_NAME              | TICK_TYPE   |   OMDSEQ | START_TIME   | END_TIME   | DELETED_TIME   |   TICK_STATUS | CALENDAR_NAME   | ACTIVITY_NAME   | TIME_ZONE     | START_DATE          | END_DATE            |   WEEKDAYS |
|--------------------------|---------------------|--------------------------|-------------|----------|--------------|------------|----------------|---------------|-----------------|-----------------|---------------|---------------------|---------------------|------------|
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-04-07 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      621 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-04-07 00:00:00 | 2023-04-07 00:00:00 |         32 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-04-10 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      249 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-04-10 00:00:00 | 2023-04-10 00:00:00 |          2 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-05-01 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      388 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-05-01 00:00:00 | 2023-05-01 00:00:00 |          2 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-05-08 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |       34 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-05-08 00:00:00 | 2023-05-08 00:00:00 |          2 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-05-29 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      243 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-05-29 00:00:00 | 2023-05-29 00:00:00 |          2 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-08-28 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |       24 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-08-28 00:00:00 | 2023-08-28 00:00:00 |          2 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-12-22 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |       37 | 08:00        | 12:30      |                |             0 | CLOUD_DB_LSE    | DAY_HOLIDAY     | Europe/London | 2023-12-22 00:00:00 | 2023-12-22 00:00:00 |         32 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-12-25 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      563 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-12-25 00:00:00 | 2023-12-25 00:00:00 |          2 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-12-26 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      325 | 00:00        | 24:00      |                |             0 | CLOUD_DB_LSE    | ALL_HOLIDAY     | Europe/London | 2023-12-26 00:00:00 | 2023-12-26 00:00:00 |          4 |
| OQD_MKTCAL::CLOUD_DB_LSE | 2023-12-29 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |       98 | 08:00        | 12:30      |                |             0 | CLOUD_DB_LSE    | DAY_HOLIDAY     | Europe/London | 2023-12-29 00:00:00 | 2023-12-29 00:00:00 |         32 |

## Equity Trading Hour History

Equity Trading Hours are retrieved by setting the `SYMBOL_NAME` to the required database prefixed with `CLOUD_DB_`.  The Time range should cover the required history of trading hours,
while the `ACTIVITY_NAME` should not include `HOLIDAY`.  The results will include the fields `TIME_ZONE`, `START_TIME`, `END_TIME`, `START_DATE` , `END_DATE` and `ACTIVITIY_NAME`.

```sql
SELECT * FROM OQD_MKTCAL.MKTCAL t
WHERE SYMBOL_NAME = 'CLOUD_DB_LSE'
and TIMESTAMP >= '1990-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and ACTIVITY_NAME not like '%HOLIDAY%'
LIMIT 100
```

#### Trading Hours History for LSE Results

| Symbol                   | Timestamp           | SYMBOL_NAME              | TICK_TYPE   |   OMDSEQ | START_TIME   | END_TIME   | DELETED_TIME   |   TICK_STATUS | CALENDAR_NAME   | ACTIVITY_NAME   | TIME_ZONE     | START_DATE          | END_DATE            |   WEEKDAYS |
|--------------------------|---------------------|--------------------------|-------------|----------|--------------|------------|----------------|---------------|-----------------|-----------------|---------------|---------------------|---------------------|------------|
| OQD_MKTCAL::CLOUD_DB_LSE | 1993-01-01 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      582 | 08:00        | 16:30      |                |             0 | CLOUD_DB_LSE    | MARKET          | Europe/London | 1993-01-01 00:00:00 | 2068-07-19 00:00:00 |         62 |
| OQD_MKTCAL::CLOUD_DB_LSE | 1993-01-01 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      583 | 16:30        | 18:30      |                |             0 | CLOUD_DB_LSE    | POST_MARKET     | Europe/London | 1993-01-01 00:00:00 | 2068-07-19 00:00:00 |         62 |
| OQD_MKTCAL::CLOUD_DB_LSE | 1993-01-01 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      584 | 07:00        | 08:00      |                |             0 | CLOUD_DB_LSE    | PRE_MARKET      | Europe/London | 1993-01-01 00:00:00 | 2068-07-19 00:00:00 |         62 |
| OQD_MKTCAL::CLOUD_DB_LSE | 1993-01-01 00:00:00 | OQD_MKTCAL::CLOUD_DB_LSE | MKTCAL      |      585 | 08:00        | 16:30      |                |             0 | CLOUD_DB_LSE    | X_MARKET_MTF    | Europe/London | 1993-01-01 00:00:00 | 2068-07-19 00:00:00 |         62 |

## Futures Product Holiday History

Futures products may have different market holidays within the same exchange.
Consequently Futures product Holidays are retrieved by setting the `SYMBOL_NAME` to the required Futures product prefixed with `TDI_F_`.  The Time range should cover the required history of holiday events,
while the `ACTIVITY_NAME` should include `HOLIDAY`.  The results will include the fields `TIME_ZONE`, `START_TIME`, `END_TIME`, `START_DATE` and `END_DATE`.

```sql
SELECT * FROM OQD_MKTCAL.MKTCAL
WHERE SYMBOL_NAME = 'TDI_F_CL'
and TIMESTAMP >= '2023-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and ACTIVITY_NAME like '%HOLIDAY%'
LIMIT 10
```

#### Trading Hours History for CL Results

| Symbol               | Timestamp           | SYMBOL_NAME          | TICK_TYPE   |   OMDSEQ | START_TIME   | END_TIME   | DELETED_TIME   |   TICK_STATUS | CALENDAR_NAME   | ACTIVITY_NAME   | TIME_ZONE        | START_DATE          | END_DATE            |   WEEKDAYS |
|----------------------|---------------------|----------------------|-------------|----------|--------------|------------|----------------|---------------|-----------------|-----------------|------------------|---------------------|---------------------|------------|
| OQD_MKTCAL::TDI_F_CL | 2023-01-16 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      502 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-01-16 00:00:00 | 2023-01-16 00:00:00 |          2 |
| OQD_MKTCAL::TDI_F_CL | 2023-02-20 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      689 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-02-20 00:00:00 | 2023-02-20 00:00:00 |          2 |
| OQD_MKTCAL::TDI_F_CL | 2023-04-07 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |    1,896 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | ALL_HOLIDAY     | America/New_York | 2023-04-07 00:00:00 | 2023-04-07 00:00:00 |         32 |
| OQD_MKTCAL::TDI_F_CL | 2023-05-29 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      708 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-05-29 00:00:00 | 2023-05-29 00:00:00 |          2 |
| OQD_MKTCAL::TDI_F_CL | 2023-06-19 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      494 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-06-19 00:00:00 | 2023-06-19 00:00:00 |          2 |
| OQD_MKTCAL::TDI_F_CL | 2023-07-04 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      480 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-07-04 00:00:00 | 2023-07-04 00:00:00 |          4 |
| OQD_MKTCAL::TDI_F_CL | 2023-09-04 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      649 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-09-04 00:00:00 | 2023-09-04 00:00:00 |          2 |
| OQD_MKTCAL::TDI_F_CL | 2023-11-23 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      608 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-11-23 00:00:00 | 2023-11-23 00:00:00 |         16 |
| OQD_MKTCAL::TDI_F_CL | 2023-11-24 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      540 | 09:00        | 13:45      |                |             0 | TDI_F_CL        | DAY_HOLIDAY     | America/New_York | 2023-11-24 00:00:00 | 2023-11-24 00:00:00 |         32 |
| OQD_MKTCAL::TDI_F_CL | 2023-11-24 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      541 | 00:00        | 24:00      |                |             0 | TDI_F_CL        | NIGHT2_HOLIDAY  | America/New_York | 2023-11-24 00:00:00 | 2023-11-24 00:00:00 |         32 |

## Futures Product Trading Hour History

Futures products may have different trading hours within the same exchange.
Futures Prodict Trading Hours are retrieved by setting the `SYMBOL_NAME` to the required database prefixed with `TDI_F_CL_`.  The Time range should cover the required history of trading hours,
while the `ACTIVITY_NAME` should not include `HOLIDAY`.  The results will include the fields `TIME_ZONE`, `START_TIME`, `END_TIME`, `START_DATE` , `END_DATE` and `ACTIVITIY_NAME`.

```sql
SELECT * FROM OQD_MKTCAL.MKTCAL t
WHERE SYMBOL_NAME = 'TDI_F_CL'
and TIMESTAMP >= '1990-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and ACTIVITY_NAME not like '%HOLIDAY%'
LIMIT 100
```

#### Trading Hours History for CL Results

| Symbol               | Timestamp           | SYMBOL_NAME          | TICK_TYPE   |   OMDSEQ | START_TIME   | END_TIME   | DELETED_TIME   |   TICK_STATUS | CALENDAR_NAME   | ACTIVITY_NAME   | TIME_ZONE        | START_DATE          | END_DATE            |   WEEKDAYS |
|----------------------|---------------------|----------------------|-------------|----------|--------------|------------|----------------|---------------|-----------------|-----------------|------------------|---------------------|---------------------|------------|
| OQD_MKTCAL::TDI_F_CL | 1993-01-06 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |        2 | 09:45        | 15:40      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 1993-01-06 00:00:00 | 1993-03-12 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 1993-03-13 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |        2 | 09:45        | 15:15      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 1993-03-13 00:00:00 | 2001-09-10 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2001-09-17 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       20 | 11:45        | 14:40      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 2001-09-17 00:00:00 | 2001-09-17 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2001-09-18 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |        8 | 09:45        | 13:45      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 2001-09-18 00:00:00 | 2001-09-19 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2001-09-20 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       43 | 10:45        | 13:50      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 2001-09-20 00:00:00 | 2001-09-28 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2001-10-01 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       10 | 10:00        | 14:00      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 2001-10-01 00:00:00 | 2001-10-05 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2001-10-06 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       33 | 10:00        | 14:30      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 2001-10-06 00:00:00 | 2006-06-10 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2001-10-06 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       34 | 15:15        | 09:30      |                |             0 | TDI_F_CL        | NIGHT1          | America/New_York | 2001-10-06 00:00:00 | 2006-06-10 00:00:00 |         60 |
| OQD_MKTCAL::TDI_F_CL | 2001-10-06 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       35 | 19:00        | 09:30      |                |             0 | TDI_F_CL        | NIGHT1          | America/New_York | 2001-10-06 00:00:00 | 2006-06-10 00:00:00 |          2 |
| OQD_MKTCAL::TDI_F_CL | 2006-06-11 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       45 | 10:00        | 14:30      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 2006-06-11 00:00:00 | 2007-01-31 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2006-06-11 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       46 | 18:00        | 10:00      |                |             0 | TDI_F_CL        | NIGHT1          | America/New_York | 2006-06-11 00:00:00 | 2007-01-31 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2006-06-11 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       47 | 14:30        | 17:15      |                |             0 | TDI_F_CL        | NIGHT2          | America/New_York | 2006-06-11 00:00:00 | 2007-01-31 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2007-02-01 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       33 | 09:00        | 14:30      |                |             0 | TDI_F_CL        | DAY             | America/New_York | 2007-02-01 00:00:00 | 2050-12-31 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2007-02-01 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       34 | 18:00        | 09:00      |                |             0 | TDI_F_CL        | NIGHT1          | America/New_York | 2007-02-01 00:00:00 | 2008-09-14 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2007-02-01 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       35 | 14:30        | 17:15      |                |             0 | TDI_F_CL        | NIGHT2          | America/New_York | 2007-02-01 00:00:00 | 2015-09-18 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2008-09-15 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      196 | 10:00        | 09:00      |                |             0 | TDI_F_CL        | NIGHT1          | America/New_York | 2008-09-15 00:00:00 | 2008-09-15 00:00:00 |          2 |
| OQD_MKTCAL::TDI_F_CL | 2008-09-16 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |       27 | 18:00        | 09:00      |                |             0 | TDI_F_CL        | NIGHT1          | America/New_York | 2008-09-16 00:00:00 | 2068-07-19 00:00:00 |         62 |
| OQD_MKTCAL::TDI_F_CL | 2015-09-21 00:00:00 | OQD_MKTCAL::TDI_F_CL | MKTCAL      |      196 | 14:30        | 17:00      |                |             0 | TDI_F_CL        | NIGHT2          | America/New_York | 2015-09-21 00:00:00 | 2068-07-19 00:00:00 |         62 |
