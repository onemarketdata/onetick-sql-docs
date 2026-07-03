# Functions - Date Time

The following Date Time functions are supported:

AS_YYYYMMDDHHMMSS,
CURDATE,
CURRENT_DATE,
CURRENT_TIME,
CURRENT_TIMESTAMP,
CURTIME,
DATEADD,
DATEDIFF,
DATE_TRUNC,
DATENAME,
DAY,
DAY_OF_WEEK,
DAYNAME,
DAYOFMONTH,
DAYOFWEEK,
DAYOFYEAR,
HOUR,
MATCHES_TIME_FILTER,
MINUTE,
MONTH,
MONTHNAME,
NOW,
NSECTIME,
NSECTIME_FORMAT,
NSECTIME_TO_LONG,
PARSE_TIME,
PARSE_NSECTIME,
QUARTER,
SECOND,
TIME_FORMAT,
TODAY,
WEEK,
YEAR.

## AS_YYYYMMDDHHMMSS

Converts the number of milliseconds since 1970/01/01 UTC into the form YYYYmmddHHMMSS for a specified timezone. The Timezone parameter is optional, and its default value is UTC.

Simple Syntax:`long AS_YYYYMMDDHHMMSS (datetime timestamp [,string timezone])````sql
select PRICE, SIZE,
AS_YYYYMMDDHHMMSS(TIMESTAMP,'America/New_York') as TIME_NUMERIC
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 5
```

## AS_YYYYMMDDHHMMSS Function Example Results

|  | Timestamp

 | PRICE

 | SIZE

 | TIME_NUMERIC

 |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ---------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- | -------------- | ------------------ | ----------------------- | ------------- | ------------------ | ----------------------- | ----------- | ----------------- | ----------------------- | --------------------- | --------------------- | ----------------------- | ------------ | ----------------- | ----------------------- | ------------ | ----------- | ----------------------- | --------- | ---------- | ----------------------- | --------- |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 20240103090011

   |
| 1

                       | 2024-01-03 14:00:24.055585176

                                                                                                     | 50.18

                                   | 2

               | 20240103090024

   |
| 2

                       | 2024-01-03 14:00:34.459323050

                                                                                                     | 50.17

                                   | 200

             | 20240103090034

   |
| 3

                       | 2024-01-03 14:00:53.553753985

                                                                                                     | 50.17

                                   | 250

             | 20240103090053

   |
| 4

                       | 2024-01-03 14:02:01.214949697

                                                                                                     | 50.47

                                   | 1

               | 20240103090201

   |
## CURDATE

Returns the number of milliseconds since 1970/01/01 UTC to the beginning of the current day in the local timezone.

Simple Syntax:`datetime CURDATE()````sql
select PRICE, SIZE,
CURDATE() as T_CURDATE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

## CURDATE Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_CURDATE

        |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ---------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | 2025-04-15 00:00:00

 |
## CURRENT_DATE

Returns the number of milliseconds since 1970/01/01 UTC to the beginning of the current day in the local timezone.

Simple Syntax:`datetime CURRENT_DATE()````sql
select PRICE, SIZE,
CURRENT_DATE() as T_CURRENT_DATE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

## CURRENT_DATE Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_CURRENT_DATE

      |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ------------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | 2025-04-15 00:00:00

 |
## CURRENT_TIME

Returns the current time expressed as the number of milliseconds since 1970/01/01 UTC.

Simple Syntax:`datetime CURRENT_TIME()````sql
select PRICE, SIZE,
CURRENT_TIME() as T_CURRENT_TIME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

## CURRENT_TIME Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_CURRENT_TIME

      |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ------------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | 2025-04-15 12:07:18.754

 |
## CURRENT_TIMESTAMP

Returns the current time expressed as the number of milliseconds since 1970/01/01 UTC.

Simple Syntax:`datetime CURRENT_TIMESTAMP()````sql
select PRICE, SIZE,
CURRENT_TIMESTAMP() as T_CURRENT_TIMESTAMP
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

## CURRENT_TIMESTAMP Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_CURRENT_TIMESTAMP

     |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | 2025-04-15 12:07:18.754

 |
## CURTIME

Returns the current time expressed as the number of milliseconds since 1970/01/01 UTC.

Simple Syntax:datetime CURTIME()\`\````sql
select PRICE, SIZE,
CURTIME() as T_CURTIME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

## CURTIME Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_CURTIME

               |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | 2025-04-15 12:07:18.754

 |
## DATEADD

Returns a specified datetime (number of milliseconds since 1970/01/01 UTC) with the specified number interval (signed integer) added to a specified datepart of that datetime (interpreted in the specified timezone, local by default).
The supported values for datepart are: `nanosecond`, `millisecond`, `second`, `minute`, `hour`, `day`, `dayofyear`, `weekday`, `week`, `month`, `quarter`, `year`.

Simple Syntax:`datetime DATEADD(string datepart, long number, datetime datetime [, string timezone])````sql
select PRICE, SIZE,
-- Date Part options are:  nanosecond, millisecond, second, minute, hour, day, dayofyear, weekday, week, month, quarter, year
DATEADD('MINUTE',1,TIMESTAMP,'America/New_York') as PLUS_MIN
from FULL_DEMO_L1.TRD
where SYMBOL_NAME = 'CSCO'
and TIMESTAMP >= '2006-06-01 00:00:00.000 UTC'
and TIMESTAMP < '2006-06-02 00:00:00.000 UTC'
limit 5
```

## DATEADD Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | PLUS_MIN

                |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2006-06-01 10:54:09.480

                                                                                                           | 19.65

                                   | 1000

            | 2006-06-01 10:55:09.480

 |
| 1

                       | 2006-06-01 11:01:38.983

                                                                                                           | 19.65

                                   | 700

             | 2006-06-01 11:02:38.983

 |
| 2

                       | 2006-06-01 11:01:41.773

                                                                                                           | 19.65

                                   | 300

             | 2006-06-01 11:02:41.773

 |
| 3

                       | 2006-06-01 11:01:41.773

                                                                                                           | 19.65

                                   | 300

             | 2006-06-01 11:02:41.773

 |
| 4

                       | 2006-06-01 11:43:48.078

                                                                                                           | 19.66

                                   | 600

             | 2006-06-01 11:44:48.078

 |
## DATEDIFF

Returns the count (signed integer) of the specified datepart boundaries crossed between the specified start time and end time (number of milliseconds since 1970/01/01 UTC, interpreted in the specified timezone, local by default).
The supported values for datepart are: `nanosecond`, `millisecond`, `second`, `minute`, `hour`, `day`, `dayofyear`, `weekday`, `week`, `month`, `quarter`, `year`.

Simple Syntax:`long DATEDIFF(string datepart, datetime starttime, datetime endtime [,string timezone])````sql
select PRICE,SIZE,EXCHANGE,PARTICIPANT_TIME,
-- Date Part options are:  nanosecond, millisecond, second, minute, hour, day, dayofyear, weekday, week, month, quarter, year
DATEDIFF("MILLISECOND",PARTICIPANT_TIME,TIMESTAMP) as DIFF_MILLIS,
DATEDIFF("MILLISECOND",PARTICIPANT_TIME,TIMESTAMP,'America/New_York') as DIFF_MILLIS2
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 5
```

## DATEDIFF Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | EXCHANGE

                | PARTICIPANT_TIME

        | DIFF_MILLIS

             | DIFF_MILLIS2

            |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | D

                       | 2024-01-03 14:00:11.388000000

 | 2

                       | 2

                       |
| 1

                       | 2024-01-03 14:00:24.055585176

                                                                                                     | 50.18

                                   | 2

               | P

                       | 2024-01-03 14:00:24.055241236

 | 0

                       | 0

                       |
| 2

                       | 2024-01-03 14:00:34.459323050

                                                                                                     | 50.17

                                   | 200

             | D

                       | 2024-01-03 14:00:34.457000000

 | 2

                       | 2

                       |
| 3

                       | 2024-01-03 14:00:53.553753985

                                                                                                     | 50.17

                                   | 250

             | D

                       | 2024-01-03 14:00:53.552000000

 | 1

                       | 1

                       |
| 4

                       | 2024-01-03 14:02:01.214949697

                                                                                                     | 50.47

                                   | 1

               | D

                       | 2024-01-03 14:01:59.771318000

 | 1443

                    | 1443

                    |
## DATE_TRUNC

Truncates a TIMESTAMP to the specified precision(datepart) in the given timezone (removes the unwanted detail of a timestamp).
The supported values for datepart are: `nanosecond`, `millisecond`, `second`, `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year`.

Simple Syntax:`datetime DATE_TRUNC (string datepart, datetime timestamp [, string timezone])````sql
select PRICE,SIZE,EXCHANGE,
-- Date Part options are:  nanosecond, millisecond, second, minute, hour, day, week, month, quarter, year
DATE_TRUNC('HOUR',TIMESTAMP) as D_DATE_TRUNC_HOUR,
DATE_TRUNC('MINUTE',TIMESTAMP,'America/New_York') as D_DATE_TRUNC_MINUTE,
DATE_TRUNC('SECOND',TIMESTAMP,'America/New_York') as D_DATE_TRUNC_SECOND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 10
```

## DATEDIFF Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | EXCHANGE

                | D_DATE_TRUNC_HOUR

             | D_DATE_TRUNC_MINUTE

     | D_DATE_TRUNC_SECOND

     |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- | ----------------------------- | ----------------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:00:00

     | 2024-01-03 14:00:11

     |
| 1

                       | 2024-01-03 14:00:24.055585176

                                                                                                     | 50.18

                                   | 2

               | P

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:00:00

     | 2024-01-03 14:00:24

     |
| 2

                       | 2024-01-03 14:00:34.459323050

                                                                                                     | 50.17

                                   | 200

             | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:00:00

     | 2024-01-03 14:00:34

     |
| 3

                       | 2024-01-03 14:00:53.553753985

                                                                                                     | 50.17

                                   | 250

             | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:00:00

     | 2024-01-03 14:00:53

     |
| 4

                       | 2024-01-03 14:02:01.214949697

                                                                                                     | 50.47

                                   | 1

               | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:02:00

     | 2024-01-03 14:02:01

     |
| 5

                       | 2024-01-03 14:02:30.637032166

                                                                                                     | 50.17

                                   | 10

              | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:02:00

     | 2024-01-03 14:02:30

     |
| 6

                       | 2024-01-03 14:02:56.469530136

                                                                                                     | 50.17

                                   | 130

             | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:02:00

     | 2024-01-03 14:02:56

     |
| 7

                       | 2024-01-03 14:03:23.521127861

                                                                                                     | 50.18

                                   | 50

              | K

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:03:00

     | 2024-01-03 14:03:23

     |
| 8

                       | 2024-01-03 14:03:53.535607777

                                                                                                     | 50.1702

                                 | 24

              | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:03:00

     | 2024-01-03 14:03:53

     |
| 9

                       | 2024-01-03 14:05:43.010257474

                                                                                                     | 50.19

                                   | 2

               | D

                       | 2024-01-03 14:00:00

           | 2024-01-03 14:05:00

     | 2024-01-03 14:05:43

     |
## DATENAME

Returns a number representing the specified datepart of the specified datetime, interpreted in the specified timezone, local by default).
The supported values for datepart are: `nanosecond`, `millisecond`, `second`, `minute`, `hour`, `day`, `dayofyear`, `weekday`, `week`, `month`, `quarter`, `year`.

Simple Syntax:`long DATENAME(string datepart, datetime datetime [,string timezone])````sql
select PRICE,SIZE,EXCHANGE,PARTICIPANT_TIME,
-- Date Part options are:  nanosecond, millisecond, second, minute, hour, day, dayofyear, weekday, week, month, quarter, year
DATENAME('HOUR',TIMESTAMP) as T_DATENAME_HOUR,
DATENAME('MINUTE',TIMESTAMP,'America/New_York') as T_DATENAME_MINUTE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 5
```

## DATEDIFF Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | EXCHANGE

                | PARTICIPANT_TIME

              | T_DATENAME_HOUR

         | T_DATENAME_MINUTE

       |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- | ----------------------------- | ----------------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | D

                       | 2024-01-03 14:00:11.388000000

 | 14

                      | 0

                       |
| 1

                       | 2024-01-03 14:00:24.055585176

                                                                                                     | 50.18

                                   | 2

               | P

                       | 2024-01-03 14:00:24.055241236

 | 14

                      | 0

                       |
| 2

                       | 2024-01-03 14:00:34.459323050

                                                                                                     | 50.17

                                   | 200

             | D

                       | 2024-01-03 14:00:34.457000000

 | 14

                      | 0

                       |
| 3

                       | 2024-01-03 14:00:53.553753985

                                                                                                     | 50.17

                                   | 250

             | D

                       | 2024-01-03 14:00:53.552000000

 | 14

                      | 0

                       |
| 4

                       | 2024-01-03 14:02:01.214949697

                                                                                                     | 50.47

                                   | 1

               | D

                       | 2024-01-03 14:01:59.771318000

 | 14

                      | 2

                       |
## DAY

Returns the day of the month in the given timezone for the specified time. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int DAY(datetime timestamp [, string timezone])````sql
select PRICE,SIZE,
DAY(TIMESTAMP,'America/New_York') as T_DAY,
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 3
```

## DAY Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_DAY

                   |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 3

                       |
| 1

                       | 2024-01-03 14:00:24.055585176

                                                                                                     | 50.18

                                   | 2

               | 3

                       |
| 2

                       | 2024-01-03 14:00:34.459323050

                                                                                                     | 50.17

                                   | 200

             | 3

                       |
## DAY_OF_WEEK

Returns the day of the week (0 is Sunday, 1 is Monday, and so forth) in the given timezone , UTC by default, for the specified time.

Simple Syntax:`int DAY_OF_WEEK(datetime timestamp [,string timezone])````sql
select PRICE,SIZE,
DAY_OF_WEEK(TIMESTAMP,'America/New_York') as T_DAY_OF_WEEK
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## DAY Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_DAY_OF_WEEK

           |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 3

                       |
## DAYNAME

Returns the name of the weekday in the given timezone for the specified time. Timezone parameter is optional, by default local timezone is used.

Simple Syntax:`String DAYNAME(datetime timestamp [, string timezone])````sql
select PRICE,SIZE,DAYNAME(TIMESTAMP,'America/New_York') as T_DAYNAME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
LIMIT 1
```

## DAYNAME Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_DAYNAME

               |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | Wednesday

               |
## DAYOFMONTH

Returns the day of the month in the given timezone for the specified time. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int DAYOFMONTH(datetime timestamp [, string timezone])````sql
select PRICE,SIZE,
DAYOFMONTH(TIMESTAMP,'America/New_York') as T_DAYOFMONTH
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## DAYOFMONTH Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_DAYOFMONTH

            |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 3

                       |
## DAYOFWEEK

Returns the day of the week (1 is Sunday, 2 is Monday, …, 7 is Saturday) in the given timezone for the specified time. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int DAYOFWEEK(datetime timestamp [,string timezone])````sql
select PRICE,SIZE,
DAYOFWEEK(TIMESTAMP,'America/New_York') as T_DAYOFWEEK
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## DAY Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_DAYOFWEEK

             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 4

                       |
## DAYOFYEAR

Returns the day of the year in the given timezone for the specified time. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int DAYOFYEAR(datetime timestamp[, string timezone])````sql
select PRICE,SIZE,
DAYOFYEAR(TIMESTAMP,'America/New_York') as T_DAYOFYEAR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## DAYOFYEAR Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_DAYOFYEAR

             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 3

                       |
## HOUR

Returns the hour in the given timezone of the computer that is executing this function, for the specified time. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int HOUR(datetime timestamp [, string timezone])````sql
select PRICE, SIZE, HOUR(TIMESTAMP,'America/New_York') as T_HOUR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## HOUR Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_HOUR

                  |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 9

                       |
## MATCHES_TIME_FILTER

Checks whether a record matches the specified time filter criteria, including a time range and optional day patterns.
The `start_time` and `end_time` parameters have the format HH:MM:SS[.mmm].  e.g. ‘09:30:00.000’.  If the optimal parameter `timezone`  is not specified, the default timezone is `UTC`.
The optional `day_patterns` represent a comma seperated list of patterns that determines days to be propagated.  The supported pattern formats are:


* month.week.weekday (0 means any month or week, 6  means the last week of the month, weekdays start from 0 for Sunday)


* month/day (0 means any month)


* year/month/day (0 means any month or year)

Examples include:


* `5/1` - Every May 1st


* `7.1.1` - Every First Monday of July


* `11.6.4`- Every last Thursday of November


* `0.0.1,0.0.2,0.0.3,0.0.4,0.0.5` - Monday to Friday

By default timestamps exactly at the `end_time` are not considered within the filter range.   This can be change by setting `end_time_tick_matches` to `true`.

Simple Syntax:`bool MATCHES_TIME_FILTER(string start_time, string end_time [, string timezone, string day_patterns, boolean end_time_tick_matches])````sql
select PRICE,SIZE,COND,EXCHANGE FROM US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-05 00:00:00 UTC'
and MATCHES_TIME_FILTER('09:35:00.000','09:40:00.000','America/New_York') = TRUE
limit 10
```

## Returning a Specific Time Period across Multiple Days Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | COND

                    | EXCHANGE

                      |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- | ----------------------------- |
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
## MINUTE

Returns the minute of the specified time in the given timezone.timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int MINUTE(datetime timestamp[, string timezone])````sql
select PRICE, SIZE, MINUTE(TIMESTAMP,'America/New_York') as T_MINUTE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## MINUTE Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_MINUTE

                |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 0

                       |
## MONTH

Returns the month of the specified time in the given timezone.timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int MONTH(datetime timestamp[, string timezone])````sql
select PRICE, SIZE, MONTH(TIMESTAMP,'America/New_York') as T_MONTH
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## MONTH Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_MONTH

                 |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 1

                       |
## MONTHNAME

Returns the name of the month in the given timezone for the specified time. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`string MONTHNAME(datetime timestamp [, string timezone])````sql
select PRICE, SIZE, MONTHNAME(TIMESTAMP,'America/New_York') as S_MONTHNAME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## MONTHNAME Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | S_MONTHNAME

             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 2025-04-16 07:45:43.123456789

 |
## NOW

Returns the current time expressed as the number of milliseconds since 1970/01/01 UTC.

Simple Syntax:`datetime NOW()````sql
select PRICE, SIZE,
NOW() as T_NOW
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

## TODAY Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_NOW

                         |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | 2025-04-15 12:07:18.754

       |
## NSECTIME

Constructs a nanosecond-granularity timestamp from a long integer value or from a datetime.
When the argument is of long type, the input is the number of nanoseconds since epoch. When the argument is a datetime, it is converted to a nanosecond value. Thus, NSECTIME(TIMESTAMP) and NSECTIME(TIMESTAMP \* 1000000) yield the same value.

Simple Syntax:`datetime NSECTIME (long/datetime value)````sql
select PRICE,SIZE,NSECTIME(1744789543123456789) as T_NSECTIME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## NSECTIME Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_NSECTIME

                    |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 2025-04-16 07:45:43.123456789

 |
## NSECTIME_FORMAT

Converts the number of nanoseconds since 1970/01/01 GMT into the string specified by format for a specified timezone.
Timezone parameter is optional, by default GMT timezone is used.

Simple Syntax:`string NSECTIME_FORMAT (string format, datetime timestamp [,string timezone])`The format might contain any characters, but the following combinations of characters have special meanings:


* `%Y` - Year (4 digits)


* `%y` - Year (2 digits)


* `%m` - Month (2 digits)


* `%d` - Day of month (2 digits)


* `%H` - Hours (2 digits, 24-hour format)


* `%I` - Hours (2 digits, 12-hour format)


* `%M` - Minutes (2 digits)


* `%S` - Seconds (2 digits)


* `%q` - Milliseconds (3 digits)


* `%J` - Nanoseconds (9 digits)


* `%p` - AM/PM (2 characters)

```sql
select PRICE, SIZE, NSECTIME_FORMAT('%Y-%m-%dT%H:%M:%S.%J',TIMESTAMP,'America/New_York') as S_NSECTIME_FORMAT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 3
```

## NSECTIME_FORMAT Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | S_NSECTIME_FORMAT

             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 2024-01-03T09:00:11.390056347

 |
| 1

                       | 2024-01-03 14:00:24.055585176

                                                                                                     | 50.18

                                   | 2

               | 2024-01-03T09:00:24.055585176

 |
| 2

                       | 2024-01-03 14:00:34.459323050

                                                                                                     | 50.17

                                   | 200

             | 2024-01-03T09:00:34.459323050

 |
## NSECTIME_TO_LONG

Returns the specified nanosecond-granularity timestamp as a long integer value.

Simple Syntax:`long NSECTIME_TO_LONG (datetime datetime)````sql
select PRICE,SIZE, NSECTIME_TO_LONG(TIMESTAMP) as I_NSECTIME_TO_LONG
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## NSECTIME_TO_LONG Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | I_NSECTIME_TO_LONG

            |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 1704290411390056347

           |
## PARSE_NSECTIME

Parses the formatted time string, converting it to the number of nanoseconds since 1970/01/01 GMT. Timezone parameter is optional, by default UTC timezone is used.

Simple Syntax:`datetime PARSE_NSECTIME(string format,string formatted_time [,string timezone])`The format might contain any characters, but the following combinations of characters have special meanings:


* `%Y` - Year (4 digits)


* `%y` - Year (2 digits)


* `%m` - Month (2 digits)


* `%d` - Day of month (2 digits)


* `%H` - Hours (2 digits, 24-hour format)


* `%I` - Hours (2 digits, 12-hour format)


* `%M` - Minutes (2 digits)


* `%S` - Seconds (2 digits)


* `%q` - Milliseconds (3 digits)


* `%J` - Nanoseconds (9 digits)


* `%p` - AM/PM (2 characters)

```sql
select PRICE, SIZE, PARSE_NSECTIME('%Y-%m-%d %H:%M:%S.%J','2025-04-01 10:11:12.345678','America/New_York') as T_PARSE_TIME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## PARSE_NSECTIME Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_PARSE_NSECTIME

              |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 2025-04-01 14:11:12.345678

    |
## PARSE_TIME

Parses the formatted time string, converting it to the number of milliseconds since 1970/01/01 GMT. Timezone parameter is optional, by default UTC timezone is used.

Simple Syntax:`datetime PARSE_TIME(string format,string formatted_time[,string timezone])`The format might contain any characters, but the following combinations of characters have special meanings:


* `%Y` - Year (4 digits)


* `%y` - Year (2 digits)


* `%m` - Month (2 digits)


* `%d` - Day of month (2 digits)


* `%H` - Hours (2 digits, 24-hour format)


* `%I` - Hours (2 digits, 12-hour format)


* `%M` - Minutes (2 digits)


* `%S` - Seconds (2 digits)


* `%q` - Milliseconds (3 digits)


* `%J` - Nanoseconds (9 digits)


* `%p` - AM/PM (2 characters)

```sql
select PRICE, SIZE, PARSE_TIME('%Y-%m-%d %H:%M:%S.%J','2025-04-01 10:11:12.345678','America/New_York') as T_PARSE_TIME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## PARSE_TIME Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_PARSE_TIME

                  |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 2025-04-01 14:11:12.345

       |
## QUARTER

Returns the quarter of a specified time in the given timezone. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int QUARTER(datetime timestamp[, string timezone])````sql
select PRICE, SIZE, QUARTER(TIMESTAMP,'America/New_York') as T_QUARTER
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## QUARTER Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_QUARTER

                     |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 1

                             |
## SECOND

Returns the second of a specified time in the given timezone. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int SECOND(datetime timestamp[, string timezone])````sql
select PRICE, SIZE, SECOND(TIMESTAMP,'America/New_York') as T_SECOND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## SECOND Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_SECOND

                      |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 11

                            |
## TIME_FORMAT

Converts the number of milliseconds since 1970/01/01 UTC into the string specified by format for the specified timezone. timezone parameter is optional, by default UTC timezone is used.

Simple Syntax:`string TIME_FORMAT (string format, datetime timestamp [,string timezone])`The format might contain any characters, but the following combinations of characters have special meanings:


* `%Y` - Year (4 digits)


* `%y` - Year (2 digits)


* `%m` - Month (2 digits)


* `%d` - Day of month (2 digits)


* `%H` - Hours (2 digits, 24-hour format)


* `%I` - Hours (2 digits, 12-hour format)


* `%M` - Minutes (2 digits)


* `%S` - Seconds (2 digits)


* `%q` - Milliseconds (3 digits)


* `%J` - Nanoseconds (9 digits)


* `%p` - AM/PM (2 characters)

```sql
SELECT PRICE, SIZE, TIME_FORMAT('%Y-%m-%dT%H:%M:%S.%J',TIMESTAMP,'America/New_York') as S_TIME_FORMAT
FROM US_COMP_SAMPLE.TRD
WHERE SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
LIMIT 10
```

## TIME_FORMAT Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | S_TIME_FORMAT

                 |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 2024-01-03T09:00:11.390000000

 |
| 1

                       | 2024-01-03 14:00:24.055585176

                                                                                                     | 50.18

                                   | 2

               | 2024-01-03T09:00:24.055000000

 |
| 2

                       | 2024-01-03 14:00:34.459323050

                                                                                                     | 50.17

                                   | 200

             | 2024-01-03T09:00:34.459000000

 |
| 3

                       | 2024-01-03 14:00:53.553753985

                                                                                                     | 50.17

                                   | 250

             | 2024-01-03T09:00:53.553000000

 |
| 4

                       | 2024-01-03 14:02:01.214949697

                                                                                                     | 50.47

                                   | 1

               | 2024-01-03T09:02:01.214000000

 |
| 5

                       | 2024-01-03 14:02:30.637032166

                                                                                                     | 50.17

                                   | 10

              | 2024-01-03T09:02:30.637000000

 |
| 6

                       | 2024-01-03 14:02:56.469530136

                                                                                                     | 50.17

                                   | 130

             | 2024-01-03T09:02:56.469000000

 |
| 7

                       | 2024-01-03 14:03:23.521127861

                                                                                                     | 50.18

                                   | 50

              | 2024-01-03T09:03:23.521000000

 |
| 8

                       | 2024-01-03 14:03:53.535607777

                                                                                                     | 50.1702

                                 | 24

              | 2024-01-03T09:03:53.535000000

 |
| 9

                       | 2024-01-03 14:05:43.010257474

                                                                                                     | 50.19

                                   | 2

               | 2024-01-03T09:05:43.010000000

 |
## TODAY

Returns the number of milliseconds since 1970/01/01 UTC to the beginning of the current day in the specified timezone. timezone parameter is optional, by default UTC timezone is used.

Simple Syntax:`datetime TODAY([string timezone])````sql
select PRICE, SIZE,
TODAY('UTC') as T_TODAY
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

## TODAY Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_TODAY

                       |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 00:00:40.413537416

                                                                                                     | 50.56

                                   | 17

              | 2025-04-15 00:00:00

           |
## WEEK

Returns the week of a specified time in the given timezone. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`datetime WEEK(datetime timestamp [, string timezone])````sql
select PRICE, SIZE, WEEK(TIMESTAMP,'America/New_York') as T_WEEK
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## WEEK Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_WEEK

                        |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 1

                             |
## YEAR

Returns the year of a specified time in the given timezone. timezone parameter is optional, by default local timezone is used.

Simple Syntax:`int YEAR(datetime timestamp[, string timezone])````sql
select PRICE, SIZE, YEAR(TIMESTAMP,'America/New_York') as T_YEAR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

## YEAR Function Example Results

|                         | TIMESTAMP

                                                                                                                         | PRICE

                                   | SIZE

            | T_YEAR

                        |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------- | ----------------------------- |
| 0

                       | 2024-01-03 14:00:11.390056347

                                                                                                     | 50.17

                                   | 300

             | 2024

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
