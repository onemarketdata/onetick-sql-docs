# Functions - Utilities

The following utility functions are supported:

[COMPUTE_HASH_CODE](),
[COMPUTE_HASH_CODE_STR](),
[GETHOSTNAME](),
[GETUSER](),
[GET_AUTHENTICATED_USERNAME](),
[GET_CPU_ID](),
[GET_ONETICK_RELEASE](),
[GET_ONETICK_VERSION](),
[GET_SERVER_PORT](),
[GET_THREAD_ID](),
[GET_THREAD_NAME](),
[GET_TOP_LEVEL_QUERY_ID](),
[GET_TOP_LEVEL_QUERY_NAME](),
[GET_TYPE](),
[GET_USER_ROLES](),
[SELECT_MATCHING_FIELDS](),
[UNDEFINED](),
[UUID]().

## COMPUTE_HASH_CODE

Returns the long hash code for the specified string with the hash function provided by the hash_function parameter.
Possible values of the hash_function are `DEFAULT`, `SUM_OF_BYTES`, `LOOKUP3`, `METRO_HASH_64`, `CITY_HASH_64` and `MURMUR_HASH_64`.
Default value of hash_function is `DEFAULT`.

Simple Syntax:
<br/>
`long COMPUTE_HASH_CODE(string str [, string hash_function])`
<br/>
```sql
select PRICE, SIZE, EXCHANGE,
-- Hash functions include:  DEFAULT, SUM_OF_BYTES, LOOKUP3, METRO_HASH_64, CITY_HASH_64 and MURMUR_HASH_64
COMPUTE_HASH_CODE(EXCHANGE,'DEFAULT') as N_COMPUTE_HASH_CODE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

#### COMPUTE_HASH_CODE Function Example Results

|    | Timestamp                     |   PRICE |   SIZE | EXCHANGE   |   N_COMPUTE_HASH_CODE |
|----|-------------------------------|---------|--------|------------|-----------------------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |     17 | Q          |                    81 |
|  1 | 2024-01-03 00:00:40.413540299 | 50.57   |     33 | Q          |                    81 |
|  2 | 2024-01-03 00:03:34.026455739 | 50.52   |     26 | P          |                    80 |
|  3 | 2024-01-03 00:03:34.026458843 | 50.51   |    123 | P          |                    80 |
|  4 | 2024-01-03 00:03:34.026459421 | 50.49   |      3 | P          |                    80 |
|  5 | 2024-01-03 00:05:06.042217019 | 50.4896 |      1 | D          |                    68 |
|  6 | 2024-01-03 00:07:11.737983252 | 50.4897 |      1 | D          |                    68 |
|  7 | 2024-01-03 00:12:17.787582635 | 50.49   |      1 | D          |                    68 |
|  8 | 2024-01-03 00:13:16.630750984 | 50.49   |      3 | P          |                    80 |
|  9 | 2024-01-03 00:14:14.187418230 | 50.49   |    100 | P          |                    80 |

## COMPUTE_HASH_CODE_STR

Returns as a string the hexadecimal encoded hash code for the specified string with the hash function provided by the hash_function parameter.
Possible values of the hash_function are `DEFAULT`, `SUM_OF_BYTES`, `LOOKUP3`, `METRO_HASH_64`, `CITY_HASH_64`, `MURMUR_HASH_64`, `SHA_1`, `SHA_224`, `SHA_256`, `SHA_384`, `SHA_512`.
Default value of hash_function is `DEFAULT`.
Note that trailing nulls also will be hashed in case of fixed size strings.

Simple Syntax:
<br/>
`string COMPUTE_HASH_CODE_STR(string str [, string hash_function])`
<br/>
```sql
select PRICE, SIZE, EXCHANGE,
COMPUTE_HASH_CODE_STR(EXCHANGE,'DEFAULT') as S_COMPUTE_HASH_CODE_STR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

#### COMPUTE_HASH_CODE_STR Function Example Results

|    | Timestamp                     |   PRICE |   SIZE | EXCHANGE   |   S_COMPUTE_HASH_CODE_STR |
|----|-------------------------------|---------|--------|------------|---------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |     17 | Q          |          5100000000000000 |
|  1 | 2024-01-03 00:00:40.413540299 | 50.57   |     33 | Q          |          5100000000000000 |
|  2 | 2024-01-03 00:03:34.026455739 | 50.52   |     26 | P          |          5000000000000000 |
|  3 | 2024-01-03 00:03:34.026458843 | 50.51   |    123 | P          |          5000000000000000 |
|  4 | 2024-01-03 00:03:34.026459421 | 50.49   |      3 | P          |          5000000000000000 |
|  5 | 2024-01-03 00:05:06.042217019 | 50.4896 |      1 | D          |          4400000000000000 |
|  6 | 2024-01-03 00:07:11.737983252 | 50.4897 |      1 | D          |          4400000000000000 |
|  7 | 2024-01-03 00:12:17.787582635 | 50.49   |      1 | D          |          4400000000000000 |
|  8 | 2024-01-03 00:13:16.630750984 | 50.49   |      3 | P          |          5000000000000000 |
|  9 | 2024-01-03 00:14:14.187418230 | 50.49   |    100 | P          |          5000000000000000 |

## GETHOSTNAME

Returns the name of the host where this function runs.

Simple Syntax:
<br/>
`string GETHOSTNAME()`
<br/>
```sql
select GETHOSTNAME() as S_GETHOSTNAME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GETHOSTNAME Function Example Results

|    | Timestamp                     |   S_GETHOSTNAME |    |                              |
|----|-------------------------------|-----------------|----|------------------------------|
|  0 | 2024-01-03 00:00:40.413537416 |           50.56 | 17 | ip-10-36-13-161.ec2.internal |

## GETUSER

Returns the name of the user who is executing the query.

Simple Syntax:
<br/>
`string string GETUSER()`
<br/>
```sql
select GETUSER() as S_GETUSER
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GETUSER Function Example Results

|    | Timestamp                     | S_GETUSER        |
|----|-------------------------------|------------------|
|  0 | 2024-01-03 00:00:40.413537416 | ccf_omd_ps_app_1 |

## GET_AUTHENTICATED_USERNAME

Returns the authenticated login name of the user who is executing the query.

Simple Syntax:
<br/>
`string string GET_AUTHENTICATED_USERNAME()`
<br/>
```sql
select GET_AUTHENTICATED_USERNAME() as S_GET_AUTHENTICATED_USERNAME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_AUTHENTICATED_USERNAME Function Example Results

|    | Timestamp                     | S_GET_AUTHENTICATED_USERNAME   |
|----|-------------------------------|--------------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | ccf_omd_ps_app_1               |

## GET_CPU_ID

Returns the ID of the CPU core that is executing this function.

Simple Syntax:
<br/>
`int GET_CPU_ID()`
<br/>
```sql
select GET_CPU_ID() as N_GET_CPU_ID
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_CPU_ID Function Example Results

|    | Timestamp                     |   N_GET_CPU_ID |
|----|-------------------------------|----------------|
|  0 | 2024-01-03 00:00:40.413537416 |             11 |

## GET_ONETICK_RELEASE

Returns the build name of OneTick, for example, BUILD_rel_20250227_initial.

Simple Syntax:
<br/>
`string GET_ONETICK_RELEASE()`
<br/>
```sql
select GET_ONETICK_RELEASE() as S_GET_ONETICK_RELEASE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_ONETICK_RELEASE Function Example Results

|    | Timestamp                     | S_GET_ONETICK_RELEASE      |
|----|-------------------------------|----------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | BUILD_rel_20250227_update4 |

## GET_ONETICK_VERSION

Returns the OneTick version (build number) represented as YYYYMMDDHHmmSS.

Simple Syntax:
<br/>
`long GET_ONETICK_VERSION()`
<br/>
```sql
select GET_ONETICK_VERSION() as N_GET_ONETICK_VERSION
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_ONETICK_VERSION Function Example Results

|    | Timestamp                     |   N_GET_ONETICK_VERSION |
|----|-------------------------------|-------------------------|
|  0 | 2024-01-03 00:00:40.413537416 |          20250227120000 |

## GET_SERVER_PORT

Returns the port number of the  server, where this function is executing.

Simple Syntax:
<br/>
`int GET_SERVER_PORT()`
<br/>
```sql
select GET_SERVER_PORT() as N_GET_SERVER_PORT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_SERVER_PORT Function Example Results

|    | Timestamp                     |   N_GET_SERVER_PORT |
|----|-------------------------------|---------------------|
|  0 | 2024-01-03 00:00:40.413537416 |               40001 |

## GET_THREAD_ID

Returns the ID of the thread that is executing this function.

Simple Syntax:
<br/>
`int GET_THREAD_ID()`
<br/>
```sql
select GET_THREAD_ID() as N_GET_THREAD_ID
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_THREAD_ID Function Example Results

|    | Timestamp                     |   N_GET_THREAD_ID |
|----|-------------------------------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 |   139894414652992 |

## GET_THREAD_NAME

Returns the name of the thread that is executing this function.

Simple Syntax:
<br/>
`string GET_THREAD_NAME()`
<br/>
```sql
select GET_THREAD_NAME() as S_GET_THREAD_NAME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_THREAD_NAME Function Example Results

|    | Timestamp                     | S_GET_THREAD_NAME   |
|----|-------------------------------|---------------------|
|  0 | 2024-01-03 00:00:40.413537416 | OMD_PS.785          |

## GET_TOP_LEVEL_QUERY_ID

Returns the unique ID of this query.

Simple Syntax:
<br/>
`string GET_TOP_LEVEL_QUERY_ID()`
<br/>
```sql
select GET_TOP_LEVEL_QUERY_ID() as S_GET_TOP_LEVEL_QUERY_ID
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_TOP_LEVEL_QUERY_ID Function Example Results

|    | Timestamp                     | S_GET_TOP_LEVEL_QUERY_ID                       |
|----|-------------------------------|------------------------------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | ip-10-36-13-227.96777.20250416093102.899.21221 |

## GET_TOP_LEVEL_QUERY_NAME

Returns the unique ID of this query.

Simple Syntax:
<br/>
`string GET_TOP_LEVEL_QUERY_NAME()`
<br/>
```sql
select GET_TOP_LEVEL_QUERY_NAME() as S_GET_TOP_LEVEL_QUERY_NAME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_TOP_LEVEL_QUERY_NAME Function Example Results

|    | Timestamp                     | S_GET_TOP_LEVEL_QUERY_NAME   |
|----|-------------------------------|------------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | File1                        |

## GET_TYPE

Returns the type name of the field name.  Possible return values are byte, string, short, int, float, uint, long, double, msectime, and nsectime.

Simple Syntax:
<br/>
`string GET_TYPE(field_name)`
<br/>
```sql
select PRICE, GET_TYPE(PRICE) as S_GET_TYPE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_TYPE Function Example Results

|    | Timestamp                     |   PRICE | S_GET_TYPE   |
|----|-------------------------------|---------|--------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 | double       |

## GET_USER_ROLES

Returns a comma-separated list of the roles of the user who is executing the query.

Simple Syntax:
<br/>
`string GET_USER_ROLES()`
<br/>
```sql
select GET_USER_ROLES() as S_GET_USER_ROLES
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GET_TOP_LEVEL_QUERY_NAME Function Example Results

|    | Timestamp                     | S_GET_USER_ROLES   |
|----|-------------------------------|--------------------|
|  0 | 2024-01-03 00:00:40.413537416 | OMD_PS             |

## SELECT_MATCHING_FIELDS

Returns a comma-separated list of schema field names filtered by name (the `regex` parameter) and optionally by type (the `field_types` parameter).
A field is included in the resulting comma-separated field list if its name in the schema fully matches the regex parameter.
If field_types parameter is specified, then the type of the field is among those listed in the field_types parameter.

regex - A regular expression to match field names with.

The following field  types are supported: `byte`, `string`, `varstring`, `short`, `int`, `uint`, `float`, `timeoffset`, `long`, `ulong`, `double`, `msectime`, `nsectime`, `decimal128`.

Simple Syntax:
<br/>
`string SELECT_MATCHING_FIELDS(string regex[, string field_types])`
<br/>
```sql
select SELECT_MATCHING_FIELDS('.*PRICE','double') as S_SELECT_MATCHING_FIELDS
from US_COMP_SAMPLE.QTE
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### SELECT_MATCHING_FIELDS Function Example Results

|    | Timestamp                     | S_SELECT_MATCHING_FIELDS   |
|----|-------------------------------|----------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | BID_PRICE                  |

## UNDEFINED

Returns false if a field with a given name was declared, and true otherwise. The parameter name is a string and must be quoted.

Simple Syntax:
<br/>
`bool UNDEFINED(string name)`
<br/>
```sql
select PRICE,
UNDEFINED('PRICE')  as N_UNDEFINED,
UNDEFINED('PRICE_D')  as N_UNDEFINED2
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### UNDEFINED Function Example Results

|    | Timestamp                     | PRICE   |   N_UNDEFINED |   N_UNDEFINED2 |
|----|-------------------------------|---------|---------------|----------------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |             0 |              1 |
|    |                               |         |               |                |
|  0 | 2024-01-03 00:00:40.413537416 | OMD_PS  |               |                |

## UUID

Returns a unique identifier in form: [mac_address].[timestamp].[process_id].[seq_num], where:

- `mac_address` - is the mac address of the machine
- `timestamp` - the current time expressed as the number of milliseconds since the UNIX epoch
- `process_id` - process ID
- `seq_num` - is a unique to a process integer that is incremented per call to this function

Simple Syntax:
<br/>
`string UUID()`
<br/>
```sql
select UUID() as S_UUID
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### UUID Function Example Results

|    | Timestamp                     | S_UUID                              |
|----|-------------------------------|-------------------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | 12F0E7B3CE6B.1744796538466.344146.1 |
|    |                               |                                     |
|    |                               |                                     |
|    |                               |                                     |
|  0 | 2024-01-03 00:00:40.413537416 | OMD_PS                              |
