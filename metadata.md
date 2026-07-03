# Metadata

A series of simple examples are provided showing how to retrieve metadata about each database or table.

## Database Listing

The list of Databases can be retrieved through the following SQL query.
The `SYMBOL_NAME` where clause needs to be specified.

```sql
select * from OTQ_CHAIN."SHOW_DB_LIST"
where SYMBOL_NAME = 'DB_INFO::'
```

## Database Authorized Listing

The list of authorised Databases, including restrictions around time ranges can be retrieved through the following SQL query.
The `SYMBOL_NAME` where clause needs to be specified.

```sql
SELECT * FROM OTQ_CHAIN."ACCESS_INFO(INFO_TYPE='DATABASES')"
where SYMBOL_NAME = 'DB_INFO::'
```

## Database Symbol Listing

Databases are partitioned by symbol, and must be queried by `SYMBOL_NAME`.
The list of available symbols for a specified database can change by day, so the query must specify in the `WHERE` clause


* The Database as the `SYMBOL_NAME` field, suffixed with `::`


* The Time window with the `TIMESTAMP` field.

```sql
SELECT * FROM OTQ_CHAIN."FIND_DB_SYMBOLS(PATTERN='%')"
where SYMBOL_NAME = 'LSE_SAMPLE::'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
```

## Table Listing

The list of tables for a specified database can be retrieved through the following SQL query.
The Database as the `SYMBOL_NAME` field, suffixed with `::`

```sql
select * from OTQ_CHAIN."DB/SHOW_TICK_TYPES"
where SYMBOL_NAME = 'LSE_SAMPLE::'
```

## Field Listing

The list of fields for a specified database can be retrieved through the following SQL query.
Each Symbol may potentially have a different schema so the symbol should also be specified along with the database.


* The Database and symbol as the `SYMBOL_NAME` field


* The Table after `SHOW_TICK_DESCRIPTOR;`

```sql
SELECT * FROM OTQ_CHAIN."SHOW_TICK_DESCRIPTOR;TRD"
where SYMBOL_NAME = 'LSE_SAMPLE::VOD'
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
