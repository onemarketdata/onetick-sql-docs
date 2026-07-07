# Interest Rate Data Retrieval

A series of simple examples are provided showing how to retrieve interest rate data from the RATES database.
The RATES database contains daily and statistical interest rate information for various rate indices and instruments.

## Interest Rate Tables

The RATES database provides data through the following tables:

* `RATES.DAY` - Daily interest rates table containing closing rates and statistics
* `RATES.STAT` - Statistical records for interest rates with symbol information and metadata

## Retrieval of All Interest Rate Symbols

Daily Data is retrieved by specifying:

* Database & STAT Table (separated by a period, e.g. `RATES.STAT`).
* Symbol selection using `SYMBOL_NAME` to return all available rates
* Timestamp range to filter for a specific date

The STAT table returns all available interest rate symbols and their associated names for a specific date.

```sql
SELECT *                                  -- Return all fields from STAT Records
FROM RATES.STAT                           -- Daily interest rates table
WHERE
  SYMBOL_NAME LIKE '%'                    -- Return All Rates
  AND TIMESTAMP >= '2024-01-03 00:00:00'  -- Start date: January 3, 2024
  AND TIMESTAMP < '2024-01-04 00:00:00'   -- End date: January 4, 2024
```

## Retrieval of Specific Interest Rate Data

Daily Data is retrieved by specifying:

* Database & DAY Table (separated by a period, e.g. `RATES.DAY`).
* Symbol using the field `SYMBOL_NAME`, with a specific interest rate identifier
* Timestamp range to filter for the desired time period

Interest rate symbols are typically named with the rate index followed by an underscore and the rate descriptor.
For example, `SONIA_RATE` represents the Sterling Overnight Index Average interest rate.

```sql
SELECT
  CLOSE                                   -- Closing interest rate value for the day
FROM RATES.DAY                            -- Daily interest rates table
WHERE
  SYMBOL_NAME = 'SONIA_RATE'              -- Filter for SONIA rate specifically
  AND TIMESTAMP >= '2024-01-01 00:00:00'  -- Start date: January 1, 2024
  AND TIMESTAMP < TODAY()                 -- End date: up to today (exclusive)
LIMIT 100000                              -- Limit results to 100,000 rows to avoid excessive data retrieval
```
