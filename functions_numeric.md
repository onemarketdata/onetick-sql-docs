# Functions - Numeric

The following numeric functions are supported:

[ABS](),
[ATOF](),
[ATOL](),
[CEIL](),
[CEILING](),
[CONVERT_32ND_TO_DOUBLE](),
[CONVERT_DOUBLE_TO_32ND](),
[DECIMAL](),
[DECIMAL_COMPARE](),
[DECIMAL_INFINITY](),
[DECIMAL_NAN](),
[DECIMAL_PI](),
[DECIMAL_TO_STRING](),
[DIV](),
[DOUBLE_COMPARE](),
[EXP](),
[FLOOR](),
[FRAND](),
[GCD](),
[INFINITY](),
[LOG](),
[LOG_10](),
[MOD](),
[NAN](),
[PI](),
[POWER](),
[RAND](),
[REPLACE_NAN](),
[ROUND](),
[ROUND_DOUBLE](),
[ROUND_DECIMAL](),
[SIGN](),
[SQRT](),
[STRING_TO_DECIMAL](),
[TOSTRING](),
[TRUNCATE]()

## ABS

Returns the absolute value of the `number`.

Simple Syntax:
<br/>
`double/decimal ABS (double/decimal number)`
<br/>
```sql
select PRICE, (PRICE-50.5) as REL_PRICE, ABS(PRICE - 50.5) as N_ABS
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### ABS Function Example Results

|    | Timestamp                     |   PRICE |   REL_PRICE |   N_ABS |
|----|-------------------------------|---------|-------------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |        0.06 |    0.06 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |        0.07 |    0.07 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |        0.02 |    0.02 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |        0.01 |    0.01 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |       -0.01 |    0.01 |

## ATOF

Converts the floating point number from the string representation into double. It supports scientific notation. Special value nan is also supported.

Simple Syntax:
<br/>
`double ATOF(string value)`
<br/>
```sql
select PRICE, SIZE, ATOF('1.23') as N_ATOF,
ATOF('nan') as N_ATOF2
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### ABS Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_VALUE | N_VALUE2   |
|----|-------------------------------|---------|--------|-----------|------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |      1.23 |            |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |      1.23 |            |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |      1.23 |            |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |      1.23 |            |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |      1.23 |            |

## ATOL

Converts the integer number from the string representation into a long integer.

Simple Syntax:
<br/>
`long ATOL(string value)`
<br/>
```sql
select PRICE, SIZE, ATOL('1.23') as N_ATOL,
ATOL('nan') as N_ATOL2
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### ATOL Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_ATOL |   N_ATOL2 |
|----|-------------------------------|---------|--------|----------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |        1 |         0 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |        1 |         0 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |        1 |         0 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |        1 |         0 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |        1 |         0 |

## CEIL

Returns a long integer value representing the smallest integer that is greater than or equal to the number.

Simple Syntax:
<br/>
`long CEIL(double/decimal number)`
<br/>
```sql
select PRICE, SIZE, CEIL(PRICE) as N_CEIL
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### CEIL Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_CEIL |
|----|-------------------------------|---------|--------|----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |       51 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |       51 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |       51 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |       51 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |       51 |

## CEILING

Returns a long integer value representing the smallest integer that is greater than or equal to the number.

Simple Syntax:
<br/>
`long CEILING(double/decimal number)`
<br/>
```sql
select PRICE, SIZE, CEILING(PRICE) as N_CEILING
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### CEILING Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_CEILING |
|----|-------------------------------|---------|--------|-------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |          51 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |          51 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |          51 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |          51 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |          51 |

## CONVERT_32ND_TO_DOUBLE

Converts 32nds format `32nds_string` to double. If throw_if_not_convertible flag is set to true, the function will throw an exception on error, otherwise it will return NaN. throw_if_not_convertible parameter is optional, with default value true.

Simple Syntax:
<br/>
`double CONVERT_32NDS_TO_DOUBLE (string 32nds_string [, boolean throw_if_not_convertible])`
<br/>
```sql
select '99-30' as EXPRESSION, CONVERT_32NDS_TO_DOUBLE('99-30') AS N_32NDS
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CONVERT_32NDS_TO_DOUBLE Function Example Results

|    | Timestamp                     | EXPRESSION   |   N_32NDS |
|----|-------------------------------|--------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | 99-30        |   99.9375 |

## CONVERT_DOUBLE_TO_32ND

Converts double number to 32nds format string. If throw_if_not_convertible flag is set is to true, function will throw an exception if it can not convert double to a 32nds string, otherwise it will return a string whose double implementation is close to the given number. throw_if_not_convertible parameter is optional, with default value true.

Simple Syntax:
<br/>
`string CONVERT_DOUBLE_TO_32NDS (double number [, boolean throw_if_not_convertible])`
<br/>
```sql
select CONVERT_DOUBLE_TO_32NDS(99.5) AS S_32NDS
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### CONVERT_DOUBLE_TO_32NDS Function Example Results

|    | Timestamp                     | S_32NDS   |
|----|-------------------------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 | 99-16     |

## DECIMAL

Constructs a decimal number from a long/double `number` (usually a literal).

Simple Syntax:
<br/>
`decimal DECIMAL(long/double number)`
<br/>
```sql
select PRICE, SIZE, DECIMAL(PRICE) as D_PRICE, DECIMAL(SIZE) as D_SIZE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### DECIMAL Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   D_PRICE |   D_SIZE |
|----|-------------------------------|---------|--------|-----------|----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |     50.56 |       17 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |     50.57 |       33 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |     50.52 |       26 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |     50.51 |      123 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |     50.49 |        3 |

## DECIMAL_COMPARE

Returns 0 if `number1` = `number2`, 1 if `number1` > `number2`, and -1 if `number1` < `number2`.
`number1` is considered to be equal to `number2` if both of them are NaN or ABS(`number1` - `number2`) / (ABS(`number1`) + ABS(`number2`)) < `eps`.
`eps` represents a relative difference (percentage) between the two numbers, not an absolute difference.

Simple Syntax:
<br/>
`integer DECIMAL_COMPARE(decimal number1, decimal number2, decimal eps)`
<br/>
```sql
select PRICE,SIZE, DECIMAL_COMPARE(DECIMAL(PRICE), DECIMAL(SIZE),DECIMAL(0.0001)) as D_COMPARE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### DECIMAL_COMPARE Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   D_COMPARE |
|----|-------------------------------|---------|--------|-------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |           1 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |           1 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |           1 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |          -1 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |           1 |

## DECIMAL_INFINITY

Returns decimal positive infinity.

Simple Syntax:
<br/>
`decimal DECIMAL_INFINITY()`
<br/>
```sql
select DECIMAL_INFINITY() as D_INFINITY
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### DECIMAL_INFINITY Function Example Results

|    | Timestamp                     |   D_INFINITY |
|----|-------------------------------|--------------|
|  0 | 2024-01-03 00:00:40.413537416 |          inf |

## DECIMAL_NAN

Returns decimal NaN.

Simple Syntax:
<br/>
`decimal DECIMAL_NAN()`
<br/>
```sql
select DECIMAL_NAN() as D_NAN
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### DECIMAL_NAN Function Example Results

|    | Timestamp                     | D_NAN   |
|----|-------------------------------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |         |

## DECIMAL_PI

Returns decimal PI.

Simple Syntax:
<br/>
`decimal DECIMAL_PI()`
<br/>
```sql
select DECIMAL_PI() as D_PI
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### DECIMAL_PI Function Example Results

|    | Timestamp                     |    D_PI |
|----|-------------------------------|---------|
|  0 | 2024-01-03 00:00:40.413537416 | 3.14159 |

## DECIMAL_TO_STRING

Converts decimal `number` into a string. `precision`, defaulting to 8, specifies the number of decimal digits after the decimal point.

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

#### DECIMAL_TO_STRING Function Example Results

|    | Timestamp                     |   S_DECIMAL_TO_STRING |
|----|-------------------------------|-----------------------|
|  0 | 2024-01-03 00:00:40.413537416 |               3.14159 |

## DIV

Computes the quotient by dividing number1 by number2.

Simple Syntax:
<br/>
`int DIV(integer number1, integer number2)`
<br/>
```sql
select PRICE,SIZE,DIV(PRICE,SIZE) N_DIV
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### DIV Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_DIV |
|----|-------------------------------|---------|--------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |       2 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |       1 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |       1 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |       0 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |      16 |

## DOUBLE_COMPARE

Returns 0 if `number1` = `number2`, 1 if `number1` > `number2`, and -1 if `number1` < `number2`.
For the purposes of this function, `number1` is considered to be equal to `number2` if both of them are NaN or ABS(`number1` - `number2`) / (ABS(`number1`) + ABS(`number2`)) < `eps`.
`eps` represents a relative difference (percentage) between the two numbers, not an absolute difference.

Simple Syntax:
<br/>
`double DOUBLE_COMPARE(double number1, double number2, double eps)`
<br/>
```sql
select PRICE,SIZE, DOUBLE_COMPARE(PRICE, SIZE,0.0001) as N_COMPARE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### DOUBLE_COMPARE Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_COMPARE |
|----|-------------------------------|---------|--------|-------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |           1 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |           1 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |           1 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |          -1 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |           1 |

## EXP

Computes the natural exponential of the number.

Simple Syntax:
<br/>
`double/decimal EXP(double/decimal number)`
<br/>
```sql
select PRICE,SIZE, EXP(SIZE) as N_EXP
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### EXP Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |              | N_EXP   |
|----|-------------------------------|---------|--------|--------------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |  2.4155e+07  |         |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |  2.14644e+14 |         |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |  1.9573e+11  |         |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |  2.61952e+53 |         |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 | 20.0855      |         |

## FLOOR

Returns a long integer value representing the largest integer that is less than or equal to the number.

Simple Syntax:
<br/>
`long FLOOR(double/decimal number)`
<br/>
```sql
select PRICE,FLOOR(PRICE) as N_FLOOR
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### FLOOR Function Example Results

|    | Timestamp                     |   PRICE |   N_FLOOR |
|----|-------------------------------|---------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |        50 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |        50 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |        50 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |        50 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |        50 |

## FRAND

Returns a pseudo-random value in the range between min and max. If parameter seed not specified, the function produces different values each time a query is invoked. If specified, the function produces the same sequence of values each time a query is invoked.
Accepts at most 3 parameters. Meaning of different number of parameters is as follows.

- 0 parameter -> min=0, max=1, no seed
- 1 parameter -> min=0, max=1, seed=1st param
- 2 parameters -> min=1st param, max=2nd param, no seed
- 0 parameters -> min=1st param, max=2nd param, seed=3rd param

Simple Syntax:
<br/>
`double FRAND(double min, double max, integer seed)`
<br/>
```sql
select FRAND(0.1,5.7) as N_FRAND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### FRAND Function Example Results

|    | Timestamp                     |   N_FRAND |
|----|-------------------------------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 |  0.478635 |

## GCD

Computes the greatest common divisor.

Simple Syntax:
<br/>
`int GCD(integer number1, integer number2)`
<br/>
```sql
select GCD(45,30) as N_GCD
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### GCD Function Example Results

|    | Timestamp                     |   N_GCD |
|----|-------------------------------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |      15 |

## INFINITY

Returns positive infinity. Comparison of INFINITY() to positive infinite value returns true.

Simple Syntax:
<br/>
`double INFINITY()`
<br/>
```sql
select INFINITY() as N_INFINITY
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### INFINITY Function Example Results

|    | Timestamp                     |   N_INFINITY |
|----|-------------------------------|--------------|
|  0 | 2024-01-03 00:00:40.413537416 |          inf |

## LOG

Computes the natural logarithm of a `number`.

Simple Syntax:
<br/>
`double/decimal LOG(double/decimal number)`
<br/>
```sql
select PRICE,SIZE, LOG(SIZE) as N_LOG
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### LOG Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_LOG |
|----|-------------------------------|---------|--------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 | 2.83321 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 | 3.49651 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 | 3.2581  |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 | 4.81218 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 | 1.09861 |

## LOG_10

Computes the base-10 logarithm of a `number`.

Simple Syntax:
<br/>
`double/decimal LOG10(double/decimal number)`
<br/>
```sql
select PRICE,SIZE, LOG10(SIZE) as N_LOG10
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### LOG10 Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_LOG10 |
|----|-------------------------------|---------|--------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |  1.23045  |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |  1.51851  |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |  1.41497  |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |  2.08991  |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |  0.477121 |

## MOD

Computes the remainder from dividing `number1` by `number2`.

Simple Syntax:
<br/>
`long MOD(long number1, long number2)`
<br/>
```sql
select PRICE,SIZE, MOD(PRICE,SIZE) as N_MOD
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### MOD Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_MOD |
|----|-------------------------------|---------|--------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |      16 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |      17 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |      24 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |      50 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |       2 |

## NAN

Returns NaN. Comparison of NAN() to NaN returns true.

Simple Syntax:
<br/>
`double NAN()`
<br/>
```sql
select NAN() as N_NAN
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### NAN Function Example Results

|    | Timestamp                     | PRICE   | SIZE   | N_NAN   |
|----|-------------------------------|---------|--------|---------|
|  0 | 2024-01-03 00:00:40.413537416 |         |        |         |

## PI

Returns PI = 3.14159265358979.

Simple Syntax:
<br/>
`double PI()`
<br/>
```sql
select PI() as N_PI
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 1
```

#### PI Function Example Results

|    | Timestamp                     |   PRICE | SIZE   | N_PI   |
|----|-------------------------------|---------|--------|--------|
|  0 | 2024-01-03 00:00:40.413537416 | 3.14159 |        |        |

## POWER

Computes the `base` to the power of the `exponent`.

Simple Syntax:
<br/>
`double/decimal POWER(double/decimal base, double/decimal exponent)`
<br/>
```sql
select PRICE, SIZE, POWER(SIZE,2) as N_POWER
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### POWER Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_POWER |
|----|-------------------------------|---------|--------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |       289 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |      1089 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |       676 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |     15129 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |         9 |

## RAND

Returns a pseudo-random value in the range between `min` and `max`. The parameter seed is optional.

Simple Syntax:
<br/>
`double RAND (long min, long max,integer seed)`
<br/>
```sql
select PRICE, SIZE, RAND(0,9) as N_RAND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### RAND Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_RAND |
|----|-------------------------------|---------|--------|----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |  8.50082 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |  6.74546 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |  5.26713 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |  7.66881 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |  7.96396 |

## REPLACE_NAN

Returns `number2` if `number1` is NaN.

Simple Syntax:
<br/>
`double/decimal REPLACE_NAN(double/decimal number1, double/decimal number2)`
<br/>
```sql
select PRICE, SIZE, REPLACE_NAN(PRICE,0) as N_REPLACE_NAN
from CME_SAMPLE.TRD
where SYMBOL_NAME='GC\Z24'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### REPLACE_NAN Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_REPLACE_NAN |
|----|-------------------------------|---------|--------|-----------------|
|  0 | 2024-01-03 00:10:35.379937645 |  2157.1 |      1 |          2157.1 |
|  1 | 2024-01-03 00:24:51.617408695 |  2158.2 |      1 |          2158.2 |
|  2 | 2024-01-03 00:32:00.925415781 |         |      1 |             0   |
|  3 | 2024-01-03 01:14:00.628316297 |  2159.1 |      1 |          2159.1 |
|  4 | 2024-01-03 01:16:22.723165427 |         |      1 |             0   |

## ROUND

Returns a long integer value representing the integer that is the absolute value of the difference of the `number`, and this integer is smallest.
If there are two such integers, it returns the greater one (i.e., ROUND(1.5) = 2 and ROUND(-1.5) = -1).

Simple Syntax:
<br/>
`long ROUND(double/decimal number)`
<br/>
```sql
select PRICE, SIZE, ROUND(PRICE) as N_ROUND
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### ROUND Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_ROUND |
|----|-------------------------------|---------|--------|-----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |        51 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |        51 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |        51 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |        51 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |        50 |

## ROUND_DECIMAL

Returns the same `number` rounded in the specified `precision` with specified `rounding_method`.  Rounding method parameter is optional (by default rounding method = UPWARD).
There are four rounding methods: `UPWARD`, `DOWNWARD`, `TOWARDS_ZERO` `AWAY_FROM_ZERO`

Simple Syntax:
<br/>
`decimal ROUND_DECIMAL(decimal number, integer precision [, string rounding_method])`
<br/>
```sql
select PRICE, SIZE, ROUND_DECIMAL(PRICE,1) as N_ROUND_DECIMAL
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### ROUND_DECIMAL Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_ROUND_DECIMAL |
|----|-------------------------------|---------|--------|-------------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |              50.6 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |              50.6 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |              50.5 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |              50.5 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |              50.5 |

## ROUND_DOUBLE

Returns the same `number` rounded to the specified number of decimal digits (`precision`) after the decimal point with specified rounding method. `rounding_method` parameter is optional. The precision should be an integer from [-12, 12].

Simple Syntax:
<br/>
`double ROUND_DOUBLE(double number, integer precision [, string rounding_method])`
<br/>
```sql
select PRICE, SIZE, ROUND_DOUBLE(PRICE,1) as N_ROUND_DOUBLE
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### ROUND_DOUBLE Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_ROUND_DOUBLE |
|----|-------------------------------|---------|--------|------------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |             50.6 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |             50.6 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |             50.5 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |             50.5 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |             50.5 |

## SIGN

Computes the sign of the number: -1 for Negative, 0 for Zero, 1 for Positive.

Simple Syntax:
<br/>
`int SIGN(double/decimal number)`
<br/>
```sql
select PRICE, SIZE, (SIZE-33) as SIZEM33, SIGN(SIZE-33) as N_SIGN
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### SIGN Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   SIZEM33 |   N_SIGN |
|----|-------------------------------|---------|--------|-----------|----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |       -16 |       -1 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |         0 |        0 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |        -7 |       -1 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |        90 |        1 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |       -30 |       -1 |

## SQRT

Computes the square root of the number.

Simple Syntax:
<br/>
`double SQRT(double/decimal number)`
<br/>
```sql
select PRICE, SIZE, SQRT(SIZE) as N_SQRT
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### SQRT Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_SQRT |
|----|-------------------------------|---------|--------|----------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |  4.12311 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |  5.74456 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |  5.09902 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 | 11.0905  |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |  1.73205 |

## STRING_TO_DECIMAL

Converts the floating point number from the string representation into decimal. Valid input examples would be “3.14”, “.0314E2”, “NAN”, “INF”.

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

## TOSTRING

Converts the `number` into a string in specified base representation.
base\` should be one of 2, 8, 10, 16 or 36; assuming 10, if not specified.
`double_precision` specifies the number of digits after the decimal point and should be specified only if the base is 10.
If the `number` is not a double, double_precision is ignored.

Simple Syntax:
<br/>
`string TOSTRING (double number, integer base, integer double_precision)`
<br/>
```sql
select PRICE, SIZE, TOSTRING(PRICE) as S_TOSTRING,
TOSTRING(PRICE,10,2) as S_TOSTRING2,
TOSTRING(PRICE,10,1) as S_TOSTRING3
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### TOSTRING Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   S_TOSTRING |   S_TOSTRING2 |   S_TOSTRING3 |
|----|-------------------------------|---------|--------|--------------|---------------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 |   50.56 |     17 |        50.56 |         50.56 |          50.6 |
|  1 | 2024-01-03 00:00:40.413540299 |   50.57 |     33 |        50.57 |         50.57 |          50.6 |
|  2 | 2024-01-03 00:03:34.026455739 |   50.52 |     26 |        50.52 |         50.52 |          50.5 |
|  3 | 2024-01-03 00:03:34.026458843 |   50.51 |    123 |        50.51 |         50.51 |          50.5 |
|  4 | 2024-01-03 00:03:34.026459421 |   50.49 |      3 |        50.49 |         50.49 |          50.5 |

## TRUNCATE

Returns the `number` truncated to `precision` places right of the decimal point. If `precision` is negative, the `number` is truncated to `precision` places to the left of the decimal point.

Simple Syntax:
<br/>
`double/decimal TRUNCATE(double/decimal number, int precision)`
<br/>
```sql
select PI(), TRUNCATE(PI(),3) as N_TRUNCATE,
TRUNCATE(PI(),2) as N_TRUNCATE2,
TRUNCATE(PI(),1) as N_TRUNCATE3
from US_COMP_SAMPLE.TRD
where SYMBOL_NAME='CSCO'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
limit 5
```

#### TRUNCATE Function Example Results

|    | Timestamp                     |   PRICE |   SIZE |   N_TRUNCATE |   N_TRUNCATE2 | N_TRUNCATE3   |
|----|-------------------------------|---------|--------|--------------|---------------|---------------|
|  0 | 2024-01-03 00:00:40.413537416 | 3.14159 |  3.141 |         3.14 |           3.1 |               |
|  1 | 2024-01-03 00:00:40.413540299 | 3.14159 |  3.141 |         3.14 |           3.1 |               |
|  2 | 2024-01-03 00:03:34.026455739 | 3.14159 |  3.141 |         3.14 |           3.1 |               |
|  3 | 2024-01-03 00:03:34.026458843 | 3.14159 |  3.141 |         3.14 |           3.1 |               |
|  4 | 2024-01-03 00:03:34.026459421 | 3.14159 |  3.141 |         3.14 |           3.1 |               |
