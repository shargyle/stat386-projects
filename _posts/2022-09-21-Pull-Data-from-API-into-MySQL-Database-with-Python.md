---
layout: post
title:  "Pull Data from API into MySQL Database with Python"
author: Seth Argyle
description: This tutorial shows how to use Python to pull data from an API and store it in tables in a MySQL database.
image: /assets/images/database.jpg
---

Data is flooding the world. With it, we can gain insights and improve decision making. Although lots of data is available online through what is called an API (Application Programming Interface), data science is much more easily accomplished when using data stored in SQL databases.

In this tutorial, you will learn how to use Python to pull data from an API and store it in an SQL table by understanding the following:
- what Python packages are required.
- how to make an API call and store the resulting data into a [pandas](https://pandas.pydata.org/docs/index.html) dataframe.
- how to write a pandas dataframe to a table in an SQL database.

### Required Packages
To get started, import the following packages into your Python script:
```
import requests
import json
import pandas as pd
from sqlalchemy import create_engine
```

### API Call
For this tutorial, I'll be pulling in BTC crypto data from polygon.io API. Whatever data/API you choose, make sure you read the corresponding documentation so as to understand what data you're getting and how it's structured.

The [requests](https://pypi.org/project/requests/) and [json](https://docs.python.org/3/library/json.html) packages enable the user to import data from an API. All you need is a url to specify the source and any parameters needed to describe the desired data. (polygon.io provides this url, just copy-paste. You'll need a personal API key as well.)

The following code pulls data from the API and stores it in a pandas dataframe called `df`:
```
security = 'BTCUSD'
url = 'https://api.polygon.io/v2/aggs/ticker/X:' + security + '/range/1/minute/2022-09-27/2022-09-27?adjusted=true&sort=asc&limit=1500&apiKey=' + api_key
```
```
response = requests.request('GET', url)
data = json.loads(response.text)
df = pd.DataFrame(data['results'])
```

Oftentimes, the naming conventions pulled from the API are vague. Here, I simply change the column names to be more intuitive:
```
df = df.rename(columns={'v':'trading_volume', 'vw':'volume_weighted_avg_price', 'o':'open_price', 'c':'close_price', 'h':'high', 'l':'low', 't':'unix_timestamp_msec', 'n':'num_transactions'})
```

pandas `to_datetime()` function enables the user to convert a timestamp from unix to readable datetimes. Here, I add a column to my dataframe called `date_timestamp` that includes a datetime version of the unix timestamp (in Msec).
```
df['date_timestamp'] = pd.to_datetime(df['unix_timestamp_msec'], unit='ms')
```

### Store pandas dataframe as table in MySQL database
Now onto writing the pandas dataframe to a table in an SQL database. The `create_engine()` function from the sqlalchemy package and pandas dataframe `to_sql()` method can be used to create a connection with an existing SQL database and then write the dataframe as a table.
```
mysql_engine = create_engine('mysql+pymysql://root:' + password + '@localhost:3306/securities')
db_connection = mysql_engine.connect()
```
```
df.to_sql(security, db_connection, if_exists='replace');
db_connection.close()
```
The `if_exisis` argument can take one of three options: `fail` (which is the default), `replace`, or `append`.

Just like reading and writing to a file, make sure to close the database when done.

That's it!

### Additional Resources
Here are some of the resources that I used to learn about this topic:
- [Pandas DataFrames - Writing To And Reading From MySQL Table](https://pythontic.com/pandas/serialization/mysql#:~:text=Create%20a%20dataframe%20by%20calling,data%20from%20the%20pandas%20dataframe.)
- [SQLAlchemy Engine Configuration](https://docs.sqlalchemy.org/en/14/core/engines.html)

[Here](https://polygon.io/docs/crypto/getting-started) is a link to polygon.io's documentation used for the API call in my script.

