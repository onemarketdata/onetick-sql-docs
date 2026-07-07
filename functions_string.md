# Functions - String

The following string functions are supported:

[ASCII](),
[BASE16_DECODE](),
[BASE16_ENCODE](),
[BASE64_DECODE](),
[BASE64_ENCODE](),
[CHAR](),
[CHAR_UTF8](),
[CHARACTER_LENGTH](),
[CHAR_LENGTH](),
[CHAR_LENGTH_UTF8](),
[CONCAT](),
[DECIMAL_TO_STRING](),
[INSERT](),
[INSERT_UTF8](),
[INSTR](),
[INSTR_UTF8](),
[IS_CHARACTER_PRESENT](),
[LCASE](),
[LCASE_UTF8](),
[LEFT](),
[LEFT_UTF8](),
[LENGTH](),
[LOCATE](),
[LOCATE_UTF8](),
[LOWER](),
[LOWER_UTF8](),
[LTRIM](),
[OCTET_LENGTH](),
[PARSE_TIME](),
[PARSE_NSECTIME](),
[POSITION](),
[POSITION_UTF8](),
[REGEX_EXTRACT](),
[REGEX_EXTRACT_UTF8](),
[REGEX_MATCH](),
[REGEX_MATCH_UTF8](),
[REGEX_REPLACE](),
[REGEX_REPLACE_UTF8](),
[REPEAT](),
[REPLACE](),
[RIGHT](),
[RIGHT_UTF8](),
[RTRIM](),
[SPACE](),
[STR](),
[STRCMP](),
[STRLEN](),
[STR_TO_UTF8](),
[STRING_TO_DECIMAL](),
[SUBSTR](),
[SUBSTR_UTF8](),
[SUBSTRING](),
[SUBSTRING_UTF8](),
[TOKEN](),
[TOSTRING](),
[TRIM](),
[UCASE](),
[UCASE_UTF8](),
[UPPER](),
[UPPER_UTF8](),
[URLENCODE](),
[URLDECODE](),
[UTF8_TO_STR]().

## ASCII

Returns the ASCII code of the first character in the string value.

Simple Syntax:
<br/>
`int ASCII(string value)`
<br/>
```sql
select PRICE, SIZE, EXCHANGE, ASCII(EXCHANGE) as S_ASCII
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

#### ASCII Function Example Results

|    | Timestamp                     |   PRICE |   SIZE | EXCHANGE   |   S_ASCII |
|----|-------------------------------|---------|--------|------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |     17 | Q          |        81 |
|  1 | 2024-01-03 00:00:40.413540299 | 50.57   |     33 | Q          |        81 |
|  2 | 2024-01-03 00:03:34.026455739 | 50.52   |     26 | P          |        80 |
|  3 | 2024-01-03 00:03:34.026458843 | 50.51   |    123 | P          |        80 |
|  4 | 2024-01-03 00:03:34.026459421 | 50.49   |      3 | P          |        80 |
|  5 | 2024-01-03 00:05:06.042217019 | 50.4896 |      1 | D          |        68 |
|  6 | 2024-01-03 00:07:11.737983252 | 50.4897 |      1 | D          |        68 |
|  7 | 2024-01-03 00:12:17.787582635 | 50.49   |      1 | D          |        68 |
|  8 | 2024-01-03 00:13:16.630750984 | 50.49   |      3 | P          |        80 |
|  9 | 2024-01-03 00:14:14.187418230 | 50.49   |    100 | P          |        80 |

## BASE16_DECODE

Decodes the string with Base16 algorithm in accordance with RFC 4648.

Simple Syntax:
<br/>
`string BASE16_DECODE (string value)`
<br/>
```sql
select '40465449' as EXPRESSION, BASE16_DECODE('40465449') as S_BASE16_DECODE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### BASE16_DECODE Function Example Results

|    | Timestamp                     |   EXPRESSION | S_BASE16_DECODE   |
|----|-------------------------------|--------------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 |     40465449 | @FTI              |

## BASE16_ENCODE

Encodes the string with Base16 algorithm in accordance with RFC 4648.

Simple Syntax:
<br/>
`string BASE16_ENCODE (string value)`
<br/>
```sql
select COND,BASE16_ENCODE(COND) as S_BASE16_ENCODE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### BASE16_DECODE Function Example Results

|    | Timestamp                     | COND   |   S_BASE16_ENCODE |
|----|-------------------------------|--------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |          40465449 |

## BASE64_DECODE

Decodes the string with Base64 algorithm in accordance with RFC 4648.

Simple Syntax:
<br/>
`string BASE64_DECODE (string value)`
<br/>
```sql
select 'QEZUSQ==' as EXPRESSION, BASE64_DECODE('QEZUSQ==') as S_BASE64_DECODE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### BASE64_DECODE Function Example Results

|    | Timestamp                     | EXPRESSION   | S_BASE64_DECODE   |
|----|-------------------------------|--------------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 | QEZUSQ==     | @FTI              |

## BASE64_ENCODE

Encodes the string with Base64 algorithm in accordance with RFC 4648.

Simple Syntax:
<br/>
`string BASE64_ENCODE (string value)`
<br/>
```sql
select COND,BASE64_ENCODE(COND) as S_BASE64_ENCODE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### BASE16_DECODE Function Example Results

|    | Timestamp                     | COND   | S_BASE64_ENCODE   |
|----|-------------------------------|--------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | QEZUSQ==          |

## CHAR

Returns a 1-character string with the character corresponding to the ASCII code passed as a parameter.

Simple Syntax:
<br/>
`string CHAR(int ascii_code)`
<br/>
```sql
select CHAR(68) as S_CHAR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CHAR Function Example Results

|    | Timestamp                     | S_CHAR   |
|----|-------------------------------|----------|
|  0 | 2024-01-03 00:00:40.413537416 | D        |

## CHAR_UTF8

Returns a 1-character string with the character corresponding to the UTF8 code passed as a parameter.

Simple Syntax:
<br/>
`string CHAR_UTF8(int utf8_code)`
<br/>
```sql
select CHAR_UTF8(8364) as S_CHAR_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CHAR_UTF8 Function Example Results

|    | Timestamp                     | S_CHAR_UTF8   |
|----|-------------------------------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 | €             |

## CHARACTER_LENGTH

Returns the length of the string. If a byte with value 0 is present in the string, its position (0-based) is returned.

Simple Syntax:
<br/>
`int CHARACTER_LENGTH(string value)`
<br/>
```sql
select 'OneTick SQL' as EXPRESSION, CHARACTER_LENGTH('OneTick SQL') as N_CHARACTER_LENGTH
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CHARACTER_LENGTH Function Example Results

|    | Timestamp                     | EXPRESSION   |   N_CHARACTER_LENGTH |
|----|-------------------------------|--------------|----------------------|
|  0 | 2024-01-03 00:00:40.413537416 | OneTick SQL  |                   11 |

## CHAR_LENGTH

Returns the length of the string. If a byte with value 0 is present in the string, its position (0-based) is returned.

Simple Syntax:
<br/>
`int CHAR_LENGTH(string value)`
<br/>
```sql
select 'OneTick SQL' as EXPRESSION, CHAR_LENGTH('OneTick SQL') as N_CHAR_LENGTH
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CHAR_LENGTH Function Example Results

|    | Timestamp                     | EXPRESSION   |   N_CHAR_LENGTH |
|----|-------------------------------|--------------|-----------------|
|  0 | 2024-01-03 00:00:40.413537416 | OneTick SQL  |              11 |

## CHAR_LENGTH_UTF8

Returns the length of the UTF8 string. If a byte with value 0 is present in the string, its position (0-based) is returned.

Simple Syntax:
<br/>
`int CHAR_LENGTH_UTF8(string value)`
<br/>
```sql
select CHAR_LENGTH_UTF8('OneTick SQL with Accénts €') as N_CHAR_LENGTH_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CHAR_LENGTH_UTF8 Function Example Results

|    | Timestamp                     |   N_CHAR_LENGTH_UTF8 |
|----|-------------------------------|----------------------|
|  0 | 2024-01-03 00:00:40.413537416 |                   26 |

## CONCAT

Returns a string that is the result of concatenating value2 to value1.

Simple Syntax:
<br/>
`string CONCAT(string value1, string value2)`
<br/>
```sql
select EXCHANGE, COND, CONCAT(EXCHANGE,COND) as S_CONCAT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CONCAT Function Example Results

|    | Timestamp                     | EXCHANGE   | COND   | S_CONCAT              |
|----|-------------------------------|------------|--------|-----------------------|
|  0 | 2024-01-03 00:00:40.413537416 | Q          | @FTI   | [Q@FTI](mailto:Q@FTI) |

## DECIMAL_TO_STRING

Converts decimal number into a string. Precision, defaulting to 8, specifies the number of decimal digits after the decimal point.

Simple Syntax:
<br/>
`string DECIMAL_TO_STRING(decimal number, integer precision)`
<br/>
```sql
select DECIMAL_TO_STRING(DECIMAL_PI(),15) as S_DECIMAL_TO_STRING
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CONCAT Function Example Results

|    | Timestamp                     |   S_DECIMAL_TO_STRING |
|----|-------------------------------|-----------------------|
|  0 | 2024-01-03 00:00:40.413537416 |               3.14159 |

## INSERT

Returns a string where `length` characters have been deleted from `value1`, beginning at `start`, and where `value2` has been inserted into string, beginning at `start`.

Simple Syntax:
<br/>
`string INSERT(string value1, int start, int length, string value2)`
<br/>
```sql
select COND, INSERT(COND,2,0,'X') as S_INSERT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### INSERT Function Example Results

|    | Timestamp                     | COND   | S_INSERT   |
|----|-------------------------------|--------|------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @XFTI      |

## INSERT_UTF8

Returns a UTF8 string where `length` characters have been deleted from `value1`, beginning at `start`, and where `value2` has been inserted into string, beginning at `start`.

Simple Syntax:
<br/>
`string INSERT_UTF8(string value1, int start, int length, string value2)`
<br/>
```sql
select COND, INSERT_UTF8(COND,2,0,'€') as S_INSERT_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### INSERT_UTF8 Function Example Results

|    | Timestamp                     | COND   | S_INSERT_UTF8   |
|----|-------------------------------|--------|-----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @€FTI           |

## INSTR

If a `substring` is found in the `value`, returns the index of the first character of the substring in the value (0-based). Otherwise, returns -1.

Simple Syntax:
<br/>
`int INSTR(string value,string substring)`
<br/>
```sql
select COND, INSTR(COND,'T') as N_INSTR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### INSTR Function Example Results

|    | Timestamp                     | COND   |   N_INSTR |
|----|-------------------------------|--------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |         2 |

## INSTR_UTF8

If a `substring` is found in the UTF8 string `value`, returns the index of the first character of the substring in the value (0-based). Otherwise, returns -1.

Simple Syntax:
<br/>
`int INSTR_UTF8(string value,string substring)`
<br/>
```sql
select COND, INSTR_UTF8(COND,'€') as N_INSTR_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### INSTR_UTF8 Function Example Results

|    | Timestamp                     | COND   |   N_INSTR_UTF8 |
|----|-------------------------------|--------|----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |             -1 |

## IS_CHARACTER_PRESENT

Returns true if the value of the field specified by `field` contains at least one character from the character set specified by `characters`. Otherwise, returns false.

Simple Syntax:
<br/>
`bool IS_CHARACTER_PRESENT(string field,string characters)`
<br/>
```sql
select PRICE,SIZE, COND, IS_CHARACTER_PRESENT(COND,'IBCGHLMNPQRVWZ479') as S_IS_CHARACTER_PRESENT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 10
```

#### IS_CHARACTER_PRESENT Function Example Results

|    | Timestamp                     |    Time |   PRICE | SIZE   |   COND | S_IS_CHARACTER_PRESENT   |
|----|-------------------------------|---------|---------|--------|--------|--------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |      17 | @FTI   |      1 |                          |
|  1 | 2024-01-03 00:00:40.413540299 | 50.57   |      33 | @FTI   |      1 |                          |
|  2 | 2024-01-03 00:03:34.026455739 | 50.52   |      26 | @ TI   |      1 |                          |
|  3 | 2024-01-03 00:03:34.026458843 | 50.51   |     123 | @ T    |      0 |                          |
|  4 | 2024-01-03 00:03:34.026459421 | 50.49   |       3 | @ TI   |      1 |                          |
|  5 | 2024-01-03 00:05:06.042217019 | 50.4896 |       1 | @ TI   |      1 |                          |
|  6 | 2024-01-03 00:07:11.737983252 | 50.4897 |       1 | @ TI   |      1 |                          |
|  7 | 2024-01-03 00:12:17.787582635 | 50.49   |       1 | @ TI   |      1 |                          |
|  8 | 2024-01-03 00:13:16.630750984 | 50.49   |       3 | @ TI   |      1 |                          |
|  9 | 2024-01-03 00:14:14.187418230 | 50.49   |     100 | @ T    |      0 |                          |
```sql
select PRICE,SIZE, COND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and IS_CHARACTER_PRESENT(COND,'IBCGHLMNPQRVWZ479') = TRUE
limit 10
```

#### IS_CHARACTER_PRESENT Function as a where clause include filter Example Results

|    | Timestamp                     |   PRICE |   SIZE | COND   |
|----|-------------------------------|---------|--------|--------|
|  0 | 2024-01-03 00:00:40.413537416 | 50.56   |     17 | @FTI   |
|  1 | 2024-01-03 00:00:40.413540299 | 50.57   |     33 | @FTI   |
|  2 | 2024-01-03 00:03:34.026455739 | 50.52   |     26 | @ TI   |
|  3 | 2024-01-03 00:03:34.026459421 | 50.49   |      3 | @ TI   |
|  4 | 2024-01-03 00:05:06.042217019 | 50.4896 |      1 | @ TI   |
|  5 | 2024-01-03 00:07:11.737983252 | 50.4897 |      1 | @ TI   |
|  6 | 2024-01-03 00:12:17.787582635 | 50.49   |      1 | @ TI   |
|  7 | 2024-01-03 00:13:16.630750984 | 50.49   |      3 | @ TI   |
|  8 | 2024-01-03 00:20:42.328826087 | 50.49   |     45 | @ TI   |
|  9 | 2024-01-03 00:20:42.328826226 | 50.49   |     25 | @ TI   |
```sql
select PRICE,SIZE, COND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
and IS_CHARACTER_PRESENT(COND,'IBCGHLMNPQRVWZ479') = FALSE
limit 10
```

#### IS_CHARACTER_PRESENT Function as a where clause include filter Example Results

|    | Timestamp                     |   PRICE |   SIZE | COND   |
|----|-------------------------------|---------|--------|--------|
|  0 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 | @ T    |
|  1 | 2024-01-03 00:14:14.187418230 |   50.49 |    100 | @ T    |
|  2 | 2024-01-03 00:47:09.486020152 |   50.49 |    100 | @FT    |
|  3 | 2024-01-03 00:47:58.373425468 |   50.48 |    200 | @ T    |
|  4 | 2024-01-03 00:50:55.662956337 |   50.47 |    100 | @ T    |
|  5 | 2024-01-03 00:50:55.662956474 |   50.47 |    100 | @ T    |
|  6 | 2024-01-03 00:54:39.124665866 |   50.47 |    272 | @FT    |
|  7 | 2024-01-03 00:58:00.488692602 |   50.49 |    155 | @FT    |
|  8 | 2024-01-03 11:09:22.666731671 |   50.19 |    181 | @FT    |
|  9 | 2024-01-03 11:09:22.671341239 |   50.2  |    199 | @ T    |

## LCASE

Converts a string to lower case.

Simple Syntax:
<br/>
`string LCASE(string value)`
<br/>
```sql
select COND, LCASE(COND) as S_LCASE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LCASE Function Example Results

|    | Timestamp                     | COND   | S_LCASE   |
|----|-------------------------------|--------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @fti      |

## LCASE_UTF8

Converts a UTF8 string to lower case.

Simple Syntax:
<br/>
`string LCASE_UTF8(string value)`
<br/>
```sql
select COND, LCASE_UTF8(COND) as S_LCASE_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LCASE_UTF8 Function Example Results

|    | Timestamp                     | COND   | S_LCASE_UTF8   |
|----|-------------------------------|--------|----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @fti           |

## LEFT

Returns the leftmost count of characters from a string value.

Simple Syntax:
<br/>
`string LEFT(string value, int count)`
<br/>
```sql
select COND, LEFT(COND,1) as S_LEFT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LEFT Function Example Results

|    | Timestamp                     | COND   | S_LEFT   |
|----|-------------------------------|--------|----------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @        |

## LEFT_UTF8

Returns the leftmost count of characters from a UTF8 string value.

Simple Syntax:
<br/>
`string LEFT_UTF8(string value, int count)`
<br/>
```sql
select COND, LEFT_UTF8(COND,1) as S_LEFT_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LEFT_UTF8 Function Example Results

|    | Timestamp                     | COND   | S_LEFT_UTF8   |
|----|-------------------------------|--------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @             |

## LENGTH

Returns the length of the string. If a byte with value 0 is present in the string, its position (0-based) is returned.

Simple Syntax:
<br/>
`int LENGTH(string value)`
<br/>
```sql
select 'OneTick SQL' as EXPRESSION, LENGTH('OneTick SQL') as N_LENGTH
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LENGTH Function Example Results

|    | Timestamp                     | EXPRESSION   |   N_LENGTH |
|----|-------------------------------|--------------|------------|
|  0 | 2024-01-03 00:00:40.413537416 | OneTick SQL  |         11 |

## LOCATE

Returns the starting position of the first occurrence of `value1` within `value2`.
The search for the first occurrence of `value1` begins with the character position indicated by `start` in `value2`.
Default value of optional parameter `start` is 1.

Simple Syntax:
<br/>
`int LOCATE(string value1, string value2[, int start])`
<br/>
```sql
select COND, LOCATE('I',COND) as N_LOCATE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LOCATE Function Example Results

|    | Timestamp                     | COND   |   N_LOCATE |
|----|-------------------------------|--------|------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |          4 |

## LOCATE_UTF8

Returns the starting position of the first occurrence of `value1` within `value2`.
The search for the first occurrence of `value1` begins with the character position indicated by `start` in `value2`.
Default value of optional parameter `start` is 1.

Simple Syntax:
<br/>
`int LOCATE_UTF8(string value1, string value2[, int start])`
<br/>
```sql
select COND, LOCATE_UTF8('I',COND) as N_LOCATE_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LOCATE_UTF8 Function Example Results

|    | Timestamp                     | COND   |   N_LOCATE_UTF8 |
|----|-------------------------------|--------|-----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |               4 |

## LOWER

Converts a string to lower case.

Simple Syntax:
<br/>
`string LOWER(string value)`
<br/>
```sql
select COND, LOWER(COND) as S_LOWER
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LOWER Function Example Results

|    | Timestamp                     | COND   | S_LOWER   |
|----|-------------------------------|--------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @fti      |

## LOWER_UTF8

Converts a UTF8 string to lower case.

Simple Syntax:
<br/>
`string LOWER_UTF8(string value)`
<br/>
```sql
select COND, LOWER_UTF8(COND) as S_LOWER_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LOWER_UTF8 Function Example Results

|    | Timestamp                     | COND   | S_LOWER_UTF8   |
|----|-------------------------------|--------|----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @fti           |

## LTRIM

Removes the leading white spaces from a string. See also RTRIM and TRIM.

Simple Syntax:
<br/>
`string LTRIM(string value)`
<br/>
```sql
select ' ' + COND as EXPRESSION, LTRIM(' ' + COND) as S_LTRIM
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### LTRIM Function Example Results

|    | Timestamp                     | EXPRESSION   | S_LTRIM   |
|----|-------------------------------|--------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI         | @FTI      |

## OCTET_LENGTH

Returns the length of the string. If a byte with value 0 is present in the string, its position (0-based) is returned.

Simple Syntax:
<br/>
`int OCTET_LENGTH(string value)`
<br/>
```sql
select 'OneTick SQL' as EXPRESSION, OCTET_LENGTH('OneTick SQL') as N_OCTET_LENGTH
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### OCTET_LENGTH Function Example Results

|    | Timestamp                     | EXPRESSION   |   N_OCTET_LENGTH |
|----|-------------------------------|--------------|------------------|
|  0 | 2024-01-03 00:00:40.413537416 | OneTick SQL  |               11 |

## PARSE_NSECTIME

Parses the formatted time string, converting it to the number of nanoseconds since 1970/01/01 GMT. Timezone parameter is optional, by default UTC timezone is used.

Simple Syntax:
<br/>
`datetime PARSE_NSECTIME(string format,string formatted_time [,string timezone])`
<br/>

The format might contain any characters, but the following combinations of characters have special meanings:

- `%Y` - Year (4 digits)
- `%y` - Year (2 digits)
- `%m` - Month (2 digits)
- `%d` - Day of month (2 digits)
- `%H` - Hours (2 digits, 24-hour format)
- `%I` - Hours (2 digits, 12-hour format)
- `%M` - Minutes (2 digits)
- `%S` - Seconds (2 digits)
- `%q` - Milliseconds (3 digits)
- `%J` - Nanoseconds (9 digits)
- `%p` - AM/PM (2 characters)

```sql
select PRICE, SIZE, PARSE_NSECTIME('%Y-%m-%d %H:%M:%S.%J','2025-04-01 10:11:12.345678','America/New_York') as T_PARSE_TIME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

#### PARSE_NSECTIME Function Example Results

|    | TIMESTAMP                     |   PRICE |   SIZE | T_PARSE_NSECTIME           |
|----|-------------------------------|---------|--------|----------------------------|
|  0 | 2024-01-03 14:00:11.390056347 |   50.17 |    300 | 2025-04-01 14:11:12.345678 |

## PARSE_TIME

Parses the formatted time string, converting it to the number of milliseconds since 1970/01/01 GMT. Timezone parameter is optional, by default UTC timezone is used.

Simple Syntax:
<br/>
`datetime PARSE_TIME(string format,string formatted_time[,string timezone])`
<br/>

The format might contain any characters, but the following combinations of characters have special meanings:

- `%Y` - Year (4 digits)
- `%y` - Year (2 digits)
- `%m` - Month (2 digits)
- `%d` - Day of month (2 digits)
- `%H` - Hours (2 digits, 24-hour format)
- `%I` - Hours (2 digits, 12-hour format)
- `%M` - Minutes (2 digits)
- `%S` - Seconds (2 digits)
- `%q` - Milliseconds (3 digits)
- `%J` - Nanoseconds (9 digits)
- `%p` - AM/PM (2 characters)

```sql
select PRICE, SIZE, PARSE_TIME('%Y-%m-%d %H:%M:%S.%J','2025-04-01 10:11:12.345678','America/New_York') as T_PARSE_TIME
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 14:00:00 UTC'
and TIMESTAMP < '2024-01-04 15:00:00 UTC'
limit 1
```

#### PARSE_TIME Function Example Results

|    | TIMESTAMP                     |   PRICE |   SIZE | T_PARSE_TIME            |
|----|-------------------------------|---------|--------|-------------------------|
|  0 | 2024-01-03 14:00:11.390056347 |   50.17 |    300 | 2025-04-01 14:11:12.345 |

## POSITION

Returns the position of `value1` in `value2`. The lowest valid value of position is 1. If `value1` was not found in `value2`, 0 is returned.

Simple Syntax:
<br/>
`int POSITION(string value1, string value2)`
<br/>
```sql
select COND, POSITION('I',COND) as N_POSITION
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### POSITION Function Example Results

|    | TIMESTAMP                     | COND   |   N_POSITION |
|----|-------------------------------|--------|--------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |            4 |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   |            4 |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   |            4 |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    |            0 |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   |            4 |

## POSITION_UTF8

Returns the position of `value1` in `value2`. The lowest valid value of position is 1. If `value1` was not found in `value2`, 0 is returned.

Simple Syntax:
<br/>
`int POSITION_UTF8(string value1, string value2)`
<br/>
```sql
select COND, POSITION_UTF8('I',COND) as N_POSITION_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### POSITION_UTF8 Function Example Results

|    | TIMESTAMP                     | COND   |   N_POSITION_UTF8 |
|----|-------------------------------|--------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |                 4 |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   |                 4 |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   |                 4 |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    |                 0 |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   |                 4 |

## REGEX_EXTRACT

Matches the `text` against the regular expression specified by the `pattern` parameter. The expressions are specified via the POSIX extended regular expression syntax.
If the match was successful, the function returns a string consisting of the first match and/or its parenthesized groups and constant string literals.
The `rewrite` parameter specifies the format of the returned string.
Within the rewrite string, you can use backslash-escaped digits (`\1` to `\9`) to insert text matching the corresponding parenthesized group from the regular expression.
`\0` in the rewrite refers to the entire matching text.
Additionally, you can use the `\u` and `\l` modifiers within the rewrite string to convert the case of the text that matches the corresponding parenthesized group (
for example, `\u1` converts `\1` to uppercase and `\l2` converts `\2` to lowercase).
If the `caseless` flag is set to true, matching is case-insensitive. The default value of optional parameter caseless is false.

Simple Syntax:
<br/>
`string REGEX_EXTRACT(string text, string pattern, string rewrite[, Boolean caseless])`
<br/>
```sql
select COND, REGEX_EXTRACT(COND, '[F-I]','\\0') as S_REGEX_EXTRACT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REGEX_EXTRACT Function Example Results

|    | TIMESTAMP                     | COND   | S_REGEX_EXTRACT   |
|----|-------------------------------|--------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | F                 |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | F                 |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | I                 |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    |                   |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | I                 |

## REGEX_EXTRACT_UTF8

Matches the UTF8 `text` against the regular expression specified by the `pattern` parameter. The expressions are specified via the POSIX extended regular expression syntax.
If the match was successful, the function returns a string consisting of the first match and/or its parenthesized groups and constant string literals.
The `rewrite` parameter specifies the format of the returned string.
Within the rewrite string, you can use backslash-escaped digits (`\1` to `\9`) to insert text matching the corresponding parenthesized group from the regular expression.
`\0` in the rewrite refers to the entire matching text.
Additionally, you can use the `\u` and `\l` modifiers within the rewrite string to convert the case of the text that matches the corresponding parenthesized group (
for example, `\u1` converts `\1` to uppercase and `\l2` converts `\2` to lowercase).
If the `caseless` flag is set to true, matching is case-insensitive. The default value of optional parameter caseless is false.

Simple Syntax:
<br/>
`string REGEX_EXTRACT_UTF8(string text, string pattern, string rewrite[, Boolean caseless])`
<br/>
```sql
select COND, REGEX_EXTRACT_UTF8(COND, '[F-I]','\\0') as S_REGEX_EXTRACT_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REGEX_EXTRACT_UTF8 Function Example Results

|    | TIMESTAMP                     | COND   | S_REGEX_EXTRACT_UTF8   |
|----|-------------------------------|--------|------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | F                      |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | F                      |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | I                      |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    |                        |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | I                      |

## REGEX_MATCH

Matches the `text` against the regular expression specified by the `pattern` parameter. The expressions are specified via the POSIX extended regular expression syntax.
If the match was successful, the function returns true; otherwise, it returns false.
If the `caseless` flag is set to true, matching is case-insensitive. The default value of optional parameter caseless is false.

Simple Syntax:
<br/>
`bool REGEX_MATCH(string text, string pattern [, Boolean caseless])`
<br/>
```sql
select COND, REGEX_MATCH(COND, '[F-I]') as N_REGEX_MATCH
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REGEX_MATCH Function Example Results

|    | TIMESTAMP                     | COND   |   N_REGEX_MATCH |
|----|-------------------------------|--------|-----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |               1 |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   |               1 |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   |               1 |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    |               0 |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   |               1 |

## REGEX_MATCH_UTF8

Matches the UTF8 `text` against the regular expression specified by the `pattern` parameter. The expressions are specified via the POSIX extended regular expression syntax.
If the match was successful, the function returns true; otherwise, it returns false.
If the `caseless` flag is set to true, matching is case-insensitive. The default value of optional parameter caseless is false.

Simple Syntax:
<br/>
`bool REGEX_MATCH_UTF8(string text, string pattern [, Boolean caseless])`
<br/>
```sql
select COND, REGEX_MATCH_UTF8(COND, '[F-I]') as N_REGEX_MATCH_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REGEX_MATCH_UTF8 Function Example Results

|    | TIMESTAMP                     | COND   |   N_REGEX_MATCH_UTF8 |
|----|-------------------------------|--------|----------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   |                    1 |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   |                    1 |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   |                    1 |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    |                    0 |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   |                    1 |

## REGEX_REPLACE

Matches the `text` against the regular expression specified by the `pattern` parameter. The expressions are specified via the POSIX extended regular expression syntax.

If the `global` flag is set to true, the function replaces all the matches of the regular expression in the `text` with the rewrite. Otherwise, only the first match is replaced.
The default value of optional parameter `global` is false.

Within the `rewrite` string, you can use backslash-escaped digits (`\1` to `\9`) to insert text matching the corresponding parenthesized group from the regular expression.
`\0` in the rewrite refers to the entire matching text.

If the `caseless` flag is set to true, matching is case-insensitive. The default value of optional parameter `caseless` is false.

Simple Syntax:
<br/>
`string REGEX_REPLACE(string text, string pattern, string rewrite [, Boolean global], Boolean caseless])`
<br/>
```sql
select COND, REGEX_REPLACE(COND, '[F-I]','X') as S_REGEX_REPLACE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REGEX_REPLACE Function Example Results

|    | TIMESTAMP                     | COND   | S_REGEX_REPLACE   |
|----|-------------------------------|--------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @XTI              |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | @XTI              |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | @ TX              |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    | @ T               |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | @ TX              |

## REGEX_REPLACE_UTF8

Matches the  UTF8 `text` against the regular expression specified by the `pattern` parameter. The expressions are specified via the POSIX extended regular expression syntax.

If the `global` flag is set to true, the function replaces all the matches of the regular expression in the `text` with the rewrite. Otherwise, only the first match is replaced.
The default value of optional parameter `global` is false.

Within the `rewrite` string, you can use backslash-escaped digits (`\1` to `\9`) to insert text matching the corresponding parenthesized group from the regular expression.
`\0` in the rewrite refers to the entire matching text.

If the `caseless` flag is set to true, matching is case-insensitive. The default value of optional parameter `caseless` is false.

Simple Syntax:
<br/>
`string REGEX_REPLACE_UTF8(string text, string pattern, string rewrite [, Boolean global], Boolean caseless])`
<br/>
```sql
select COND, REGEX_REPLACE_UTF8(COND, '[F-I]','X') as S_REGEX_REPLACE_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REGEX_REPLACE_UTF8 Function Example Results

|    | TIMESTAMP                     | COND   | S_REGEX_REPLACE_UTF8   |
|----|-------------------------------|--------|------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @XTI                   |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | @XTI                   |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | @ TX                   |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    | @ T                    |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | @ TX                   |

## REPEAT

Returns a string composed of the `value` repeated `count` times.

Simple Syntax:
<br/>
`string REPEAT(string value, int count)`
<br/>
```sql
select REPEAT('ONETICK',5) as S_REPEAT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### REPEAT Function Example Results

|    | TIMESTAMP                     | S_REPEAT                            |
|----|-------------------------------|-------------------------------------|
|  0 | 2024-01-03 00:00:40.413537416 | ONETICKONETICKONETICKONETICKONETICK |

## REPLACE

Search `value1` for occurrences of `value2`, and replace with `value3`.

Simple Syntax:
<br/>
`string REPLACE(string value1, string value2, string value3)`
<br/>
```sql
select COND, REPLACE(COND,'T','TRADING') as S_REPLACE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REPEAT Function Example Results

|    | TIMESTAMP                     | S_REPLACE   |            |
|----|-------------------------------|-------------|------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI        | @FTRADINGI |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI        | @FTRADINGI |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI        | @ TRADINGI |
|  3 | 2024-01-03 00:03:34.026458843 | @ T         | @ TRADING  |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI        | @ TRADINGI |

## RIGHT

Returns the rightmost `count` of characters from `value`.

Simple Syntax:
<br/>
`string RIGHT(string value, int count))`
<br/>
```sql
select COND, RIGHT(COND,2) as S_RIGHT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### RIGHT Function Example Results

|    | TIMESTAMP                     | S_RIGHT   |    |
|----|-------------------------------|-----------|----|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI      | TI |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI      | TI |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI      | TI |
|  3 | 2024-01-03 00:03:34.026458843 | @ T       | T  |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI      | TI |

## RIGHT_UTF8

Returns the rightmost `count` of characters from UTF8 string `value`.

Simple Syntax:
<br/>
`string RIGHT_UTF8(string value, int count))`
<br/>
```sql
select COND, RIGHT_UTF8(COND,2) as S_RIGHT_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### RIGHT_UTF8 Function Example Results

|    | TIMESTAMP                     | S_RIGHT_UTF8   |    |
|----|-------------------------------|----------------|----|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI           | TI |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI           | TI |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI           | TI |
|  3 | 2024-01-03 00:03:34.026458843 | @ T            | T  |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI           | TI |

## RTRIM

Removes the trailing  white spaces from a string. See also LTRIM and TRIM.

Simple Syntax:
<br/>
`string RTRIM(string value)`
<br/>
```sql
select  COND + ' ' as EXPRESSION, RTRIM(COND + ' ') as S_RTRIM
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### RTRIM Function Example Results

|    | Timestamp                     | EXPRESSION   | S_RTRIM   |
|----|-------------------------------|--------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI         | @FTI      |

## SPACE

Returns a string consisting of `count` spaces.

Simple Syntax:
<br/>
`string SPACE(int count)`
<br/>
```sql
select SPACE(10) as S_SPACE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### SPACE Function Example Results

|    | Timestamp                     | S_SPACE   |
|----|-------------------------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 |           |

## STR

Converts `number` to string with given `length` and `precision`.
The specified `length` should be greater than or equal to the part of the number before the decimal point plus the number’s sign (if any).
Default value for `length` is 10, for `precision` 6.

Simple Syntax:
<br/>
`string STR(double number, integer length, integer double_precision)`
<br/>
```sql
select PRICE, STR(PRICE,10,2) as S_STR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### STR Function Example Results

|    | Timestamp                     |   PRICE |   S_STR |
|----|-------------------------------|---------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |   50.56 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |   50.57 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |   50.52 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |   50.51 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |   50.49 |

## STRCMP

Compares the strings `value1` and `value2`. Returns 0 if they are equal, -1 if `value1` is lexicographically smaller than `value2`, 1 otherwise.

Simple Syntax:
<br/>
`int STRCMP(string value1, string value2)`
<br/>
```sql
select STRCMP('hallo','HELLO') as N_STRCMP
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### STR Function Example Results

|    | Timestamp                     |   N_STRCMP |
|----|-------------------------------|------------|
|  0 | 2024-01-03 00:00:40.413537416 |          1 |

## STRLEN

Returns the length of the string. If a byte with value 0 is present in the string, its position (0-based) is returned.

Simple Syntax:
<br/>
`int STRLEN(string value)`
<br/>
```sql
select 'OneTick SQL' as EXPRESSION, STRLEN('OneTick SQL') as N_STRLEN
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### STRLEN Function Example Results

|    | Timestamp                     | EXPRESSION   |   N_STRLEN |
|----|-------------------------------|--------------|------------|
|  0 | 2024-01-03 00:00:40.413537416 | OneTick SQL  |         11 |

## STR_TO_UTF8

Returns the specified string re-encoded from `encoding` to UTF-8.

Simple Syntax:
<br/>
`string STR_TO_UTF8(string encoding, string value)`
<br/>
```sql
select COND, STR_TO_UTF8('ASCII',COND) as S_STR_TO_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### STR_TO_UTF8 Function Example Results

|    | Timestamp                     | COND   | S_STR_TO_UTF8   |
|----|-------------------------------|--------|-----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | @FTI            |

## STRING_TO_DECIMAL

Converts the floating point number from the `string` representation into decimal. Valid input examples would be “3.14”, “.0314E2”, “NAN”, “INF”.

Simple Syntax:
<br/>
`decimal STRING_TO_DECIMAL(string)`
<br/>
```sql
select STRING_TO_DECIMAL('12345678.90123456789') as N_STRING_TO_DECIMAL
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### STRING_TO_DECIMAL Function Example Results

|    | Timestamp                     |   N_STRING_TO_DECIMAL |
|----|-------------------------------|-----------------------|
|  0 | 2024-01-03 00:00:40.413537416 |           1.23457e+07 |

## SUBSTR

For a positive `start_index` returns `num_bytes` of the input string `value`, starting from the position specified by `start_index` (0-based).
For a negative start_index, the position is counted from the end of the field.
If the `num_bytes` parameter is omitted, returns a part of the input string starting at `start_index`.

Simple Syntax:
<br/>
`string SUBSTR(string value,int start_index,int num_bytes)`
<br/>
```sql
select COND, SUBSTR(COND,1,2) as S_SUBSTR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### SUBSTR Function Example Results

|    | Timestamp                     | COND   | S_SUBSTR   |
|----|-------------------------------|--------|------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | FT         |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | FT         |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | T          |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    | T          |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | T          |

## SUBSTR_UTF8

For a positive `start_index` returns `num_bytes` of the input UTF8 string `value`, starting from the position specified by `start_index` (0-based).
For a negative start_index, the position is counted from the end of the field.
If the `num_bytes` parameter is omitted, returns a part of the input string starting at `start_index`.

Simple Syntax:
<br/>
`string SUBSTR_UTF8(string value,int start_index,int num_bytes)`
<br/>
```sql
select COND, SUBSTR_UTF8(COND,1,2) as S_SUBSTR_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### SUBSTR_UTF8 Function Example Results

|    | Timestamp                     | COND   | S_SUBSTR_UTF8   |
|----|-------------------------------|--------|-----------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | FT              |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | FT              |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | T               |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    | T               |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | T               |

## SUBSTRING

For a positive `start_index` returns `num_bytes` of the input string `value`, starting from the position specified by `start_index` (0-based).
For a negative start_index, the position is counted from the end of the field.
If the `num_bytes` parameter is omitted, returns a part of the input string starting at `start_index`.

Simple Syntax:
<br/>
`string SUBSTRING(string value,int start_index,int num_bytes)`
<br/>
```sql
select COND, SUBSTRING(COND,1,2) as S_SUBSTRING
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### SUBSTRING Function Example Results

|    | Timestamp                     | COND   | S_SUBSTRING   |
|----|-------------------------------|--------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | FT            |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | FT            |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | T             |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    | T             |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | T             |

## SUBSTRING_UTF8

For a positive `start_index` returns `num_bytes` of the input UTF8 string `value`, starting from the position specified by `start_index` (0-based).
For a negative start_index, the position is counted from the end of the field.
If the `num_bytes` parameter is omitted, returns a part of the input string starting at `start_index`.

Simple Syntax:
<br/>
`string SUBSTRING_UTF8(string value,int start_index,int num_bytes)`
<br/>
```sql
select COND, SUBSTRING_UTF8(COND,1,2) as S_SUBSTRING_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### SUBSTRING_UTF8 Function Example Results

|    | Timestamp                     | COND   | S_SUBSTRING_UTF8   |
|----|-------------------------------|--------|--------------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | FT                 |
|  1 | 2024-01-03 00:00:40.413540299 | @FTI   | FT                 |
|  2 | 2024-01-03 00:03:34.026455739 | @ TI   | T                  |
|  3 | 2024-01-03 00:03:34.026458843 | @ T    | T                  |
|  4 | 2024-01-03 00:03:34.026459421 | @ TI   | T                  |

## TOKEN

Splits the `value` into tokens based on the `delimiter` and returns Nth token, where N=\`\`token_index\`\` (0-based), for a positive `token_index`.
For a negative `token_index`, it counts from the end instead of the beginning.

Simple Syntax:
<br/>
`string TOKEN(string value,int token_index,char delimiter)`
<br/>
```sql
select 'A:B:C:D' as EXPRESSION,
TOKEN('A:B:C:D',0,':') as S_TOKEN,
TOKEN('A:B:C:D',1,':') as S_TOKEN2,
TOKEN('A:B:C:D',-1,':') as S_TOKEN3,
TOKEN('A:B:C:D',10,':') as S_TOKEN4,
TOKEN('A:B:C:D',1,'|') as S_TOKEN5
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### TOKEN Function Example Results

|    | Timestamp                     | EXPRESSION   | S_TOKEN   | S_TOKEN2   | S_TOKEN3   | S_TOKEN4   | S_TOKEN5   |
|----|-------------------------------|--------------|-----------|------------|------------|------------|------------|
|  0 | 2024-01-03 00:00:40.413537416 | A:B:C:D      | A         | B          | D          |            |            |

## TOSTRING

Converts the `number` into a string in specified `base` representation. `Base` should be one of 2, 8, 10, 16 or 36; assuming 10, if not specified.
`double_precision` specifies the number of digits after the decimal point and should be specified only if the base is 10. If the number is not a double, `double_precision` is ignored.

Simple Syntax:
<br/>
`string TOSTRING (double number, integer base, integer double_precision)`
<br/>
```sql
select PRICE, TOSTRING(PRICE) as S_TOSTRING,
TOSTRING(PRICE,10,2) as S_TOSTRING2,
TOSTRING(PRICE,10,1) as S_TOSTRING3
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### TOSTRING Function Example Results

|    | Timestamp                     |   PRICE |   S_TOSTRING |   S_TOSTRING2 |   S_TOSTRING3 |
|----|-------------------------------|---------|--------------|---------------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |        50.56 |         50.56 |          50.6 |

## TRIM

Removes white spaces from both sides of the string. See also LTRIM and RTRIM.

Simple Syntax:
<br/>
`string TRIM(string value)`
<br/>
```sql
select ' ' + COND + ' ' as EXPRESSION, TRIM(' ' + COND + ' ') as S_TRIM
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### TRIM Function Example Results

|    | Timestamp                     | EXPRESSION   | S_TRIM   |
|----|-------------------------------|--------------|----------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI         | @FTI     |

## UCASE

Converts a string `value` to upper case.

Simple Syntax:
<br/>
`string UCASE(string value))`
<br/>
```sql
select 'Ucase' as EXPRESSION, UCASE('Ucase') as S_UCASE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### UCASE Function Example Results

|    | Timestamp                     | EXPRESSION   | S_UCASE   |
|----|-------------------------------|--------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | Ucase        | UCASE     |

## UCASE_UTF8

Converts a UTF8 string to upper case.

Simple Syntax:
<br/>
`string UCASE_UTF8(string value))`
<br/>
```sql
select 'Ucasé€' as EXPRESSION, UCASE_UTF8('Ucasé€') as S_UCASE_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### UCASE_UTF8 Function Example Results

|    | Timestamp                     | EXPRESSION   | S_UCASE_UTF8   |
|----|-------------------------------|--------------|----------------|
|  0 | 2024-01-03 00:00:40.413537416 | Ucasé€       | UCASÉ€         |

## UPPER

Converts a string `value` to upper case.

Simple Syntax:
<br/>
`string UPPER(string value))`
<br/>
```sql
select 'Upper' as EXPRESSION, UPPER('Upper') as S_UPPER
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### UPPER Function Example Results

|    | Timestamp                     | EXPRESSION   | S_UPPER   |
|----|-------------------------------|--------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | Upper        | UPPER     |

## UPPER_UTF8

Converts a UTF8 string `value` to upper case.

Simple Syntax:
<br/>
`string UPPER_UTF8(string value))`
<br/>
```sql
select 'Upper€' as EXPRESSION, UPPER_UTF8('Upper€') as S_UPPER_UTF8
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### UPPER_UTF8 Function Example Results

|    | Timestamp                     | EXPRESSION   | S_UPPER_UTF8   |
|----|-------------------------------|--------------|----------------|
|  0 | 2024-01-03 00:00:40.413537416 | Upper€       | UPPER€         |

## URLENCODE

Modifies the string `value` to be URI compliant, in accordance with RFC 3986.

Simple Syntax:
<br/>
`string URLENCODE(string value)`
<br/>
```sql
select COND, URLENCODE(COND) as S_URLENCODE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### URLENCODE Function Example Results

|    | Timestamp                     | COND   | S_URLENCODE   |
|----|-------------------------------|--------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | %40FTI        |

## URLDECODE

Inverse of URLENCODE.

Simple Syntax:
<br/>
`string URLDECODE(string value)`
<br/>
```sql
select COND, '%40%20T%20' as EXPRESSION, URLDECODE('%40%20T%20') as S_URLDECODE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### URLDECODE Function Example Results

|    | Timestamp                     | COND   | EXPRESSION   | S_URLDECODE   |
|----|-------------------------------|--------|--------------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 | @FTI   | %40%20T%20   | @ T           |

## UTF8_TO_STR

Returns the specified string `value` re-encoded from UTF-8 to `encoding`.

Simple Syntax:
<br/>
`string UTF8_TO_STR(string encoding, string value)`
<br/>
```sql
select '¿Cómo estás?' as EXPRESSION, UTF8_TO_STR('ASCII','¿Cómo estás?') as S_UTF8_TO_STR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### UTF8_TO_STR Function Example Results

|    | Timestamp                     | EXPRESSION   | S_UTF8_TO_STR   |
|----|-------------------------------|--------------|-----------------|
|  0 | 2024-01-03 00:00:40.413537416 | ¿Cómo estás? | Cmo ests?       |
