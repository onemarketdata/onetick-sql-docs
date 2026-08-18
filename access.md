<a id="access-installation"></a>

# Access & Installation

SQL access is exposed through Dashboard, REST, Python, ODBC/JDBC and client tools.

<a id="dashboard-access"></a>

## Dashboard Access

SQL Access to real time and historic market data stored in [OneTick Cloud](https://www.onetick.com/cloud-services).
and is available in the [SQL Querying Examples Dashboard](https://authdash.cloud.onetick.com/web_dashboard/?dash=sql).

This includes both query execution, plus over 80 examples covering data retrieval, and SQL syntax.

<a id="rest-access"></a>

## REST Access

SQL Queries can be issued via REST to a OneTick server.  The REST query includes the parameters:

* `query_type` equal to `sql`
* `time_zone` equal to the desired time zone for the returned data. (e.g. `America\\New_York`)
* `statement` equal to the desired SQL Statement

e.g.
`https://rest.cloud.onetick.com/omdwebapi/rest/?params={"query_type":"sql","response":"csv","compression":"none","limit":"10000","all_times_are_readable":"false","show_times_as_nanos":"true","timezone":"Europe/London","statement":"SELECT * FROM LSE_SAMPLE_BARS.TRD_1M WHERE SYMBOL_NAME='VOD' and TIMESTAMP >= '2024-01-03 00:00:00 UTC' and TIMESTAMP < '2024-01-04 00:00:00 UTC' LIMIT 10"}`

REST Queries can be issued via Python by specifying the SQL `statement`, building the `params` dictionary, and executing the REST request.
[OneTick Cloud](https://www.onetick.com/cloud-services) requires the specification of a `client_id` and `client_secret` which can be retrieved from your [profile](https://authdash.cloud.onetick.com/web_dashboard/?dash=sub_profile), after registering.

They may also be stored in environment variables: `OTP_CLIENT_ID` and `OTP_CLIENT_SECRET`

```python
import requests, json, gzip

rest_url = "https://rest.cloud.onetick.com/omdwebapi/rest/"
access_token_url = "https://cloud-auth.parent.onetick.com/realms/OMD/protocol/openid-connect/token"
# Replace [client_id] and [client_secret] with values retrieved from your profile
client_id = "[client_id]"
client_secret = "[client_secret]"

def get_access_token(url, client_id, client_secret):
    response = requests.post(url,data={"grant_type": "client_credentials"}, auth=(client_id, client_secret))
    return response.json()["access_token"]

sql_statement = """
SELECT * FROM LSE_SAMPLE_BARS.TRD_1M
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
"""

params = {
    "query_type": "sql",
    "timezone" : "Europe/London",
    "response": "csv",
    "show_times_as_nanos" : "true",
    "all_times_are_readable" : "false",
    "compression":"gzip",
    "limit":"1000",
    "statement":sql_statement
    }

access_token = get_access_token(access_token_url, client_id, client_secret)
headers = {
    "Authorization": f"Bearer {access_token}",
    "Content-Type": "application/json"
}
response = requests.post(rest_url, json=params, headers=headers)
uncompressed = gzip.decompress(response.content)
print(uncompressed.decode('utf-8'))
```

<a id="python-access"></a>

## Python Access

SQL Queries can be issued via Python to a OneTick server using our Python API (`onetick.query_webapi`), returning data directly into a dataframe.
To install the python wheel:

```python
pip install -U onetick-py onetick-query-webapi
```

SQL statements are stored in a query object using  `otq.SqlQuery`, and the query is executed using `otq.run`.
[OneTick Cloud](https://www.onetick.com/cloud-services) requires the specification of a `client_id` and `client_secret` which can be retrieved from your [profile](https://authdash.cloud.onetick.com/web_dashboard/?dash=sub_profile), after registering.

They may also be stored in environment variables: `OTP_CLIENT_ID` and `OTP_CLIENT_SECRET`

```python
import datetime, requests, json
import pandas as pd
import onetick.query_webapi as otq  # See the About Tab for how to download and install from our pip server

http_address = "https://rest.cloud.onetick.com:443"
access_token_url = "https://cloud-auth.parent.onetick.com/realms/OMD/protocol/openid-connect/token"
# Replace [client id] and [client_secret] with values retrieved from your profile
client_id = "[client id]"
client_secret = "[client secret]"


sql_statement = """
SELECT * FROM LSE_SAMPLE_BARS.TRD_1M
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
"""

query = otq.SqlQuery(sql_statement=sql_statement)
query.set_merge_all_symbols_flag(True)

access_token = otq.get_access_token(access_token_url, client_id, client_secret)
result = otq.run(query,
    http_address=http_address,access_token=access_token,
    output_mode="pandas",
    timezone="UTC")

for sym in result:
    print(sym)
    print(result.output(sym).data)
```

<a id="arrow-flight"></a>

## Arrow Flight

SQL Queries can be issued via Python to a OneTick server via Arrow Flight using ADBC, returning data directly into a dataframe.
[OneTick Cloud](https://www.onetick.com/cloud-services) requires the specification of a `client_id` and `client_secret` which can be retrieved from your [profile](https://authdash.cloud.onetick.com/web_dashboard/?dash=sub_profile), after registering.

They may also be stored in environment variables: `OTP_CLIENT_ID` and `OTP_CLIENT_SECRET`

```python
import adbc_driver_flightsql.dbapi
import requests, json
import pandas as pd

flight_address = "grpc+tcp://proxy.cloud.onetick.com:32010"
access_token_url = "https://cloud-auth.parent.onetick.com/realms/OMD/protocol/openid-connect/token"
# Replace [client id] and [client_secret] with values retrieved from your profile
client_id = "[client id]"
client_secret = "[client secret]"

def get_access_token(url, client_id, client_secret):
    response = requests.post(url,data={"grant_type": "client_credentials"}, auth=(client_id, client_secret))
    return response.json()["access_token"]

sql_statement = """
SELECT * FROM LSE_SAMPLE_BARS.TRD_1M
WHERE SYMBOL_NAME='VOD'
and TIMESTAMP >= '2024-01-03 00:00:00 UTC'
and TIMESTAMP < '2024-01-04 00:00:00 UTC'
LIMIT 10
"""

# Receive and add Access Token
access_token = get_access_token(access_token_url, client_id, client_secret)
db_kwargs = {adbc_driver_flightsql.DatabaseOptions.AUTHORIZATION_HEADER.value: f"Bearer {access_token}"}

# Execute Query
with adbc_driver_flightsql.dbapi.connect(flight_address, db_kwargs=db_kwargs) as conn:
    with conn.cursor() as cursor:
        cursor.execute(sql_statement)
        arrow_table = cursor.fetch_arrow_table()
        df = arrow_table.to_pandas()
        print(df)
```
