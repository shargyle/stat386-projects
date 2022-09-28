---
layout: post
title:  "Pull Data from API into MySQL Database with Python"
author: Seth Argyle
description: This tutorial shows how to use python to pull data from an API and store it in tables in a MySQL database.
image: /assets/images/database.jpg
---

Have you ever wanted to store API data into your own database? If so, here's a quick tutorial on how to do it using Python. (Note: I use MySQL as my preffered database, but any other dialect of SQL will work with this tutorial as well--with a few tweaks that I'll mention.)


### Required Packages
To get started, import the following packages into your Python script:

```
import requests
import json
import pandas as pd
from sqlalchemy import create_engine
import pymysql
```

### API Call
For this tutorial, I'll be pulling in BTC crypto data from polygon.io API. Whatever data/API you choose, make sure you read the corresponding documentation so as to understand what data you're getting and how it's structured.

The requests and json packages enable the user to import data from an API. All you need is a url to specify the source and any parameters needed to describe the desired data. (polygon.io provides this url, just copy-paste. You'll need a personal API key as well.)

The following code pulls data from the API and stores it in a pandas dataframe called 'df':
```
### url components
security = 'BTCUSD'
url = 'https://api.polygon.io/v2/aggs/ticker/X:' + security + '/range/1/minute/2022-09-27/2022-09-27?adjusted=true&sort=asc&limit=1500&apiKey=' + api_key

### call API, store as pandas df
response = requests.request('GET', url)
data = json.loads(response.text)
df = pd.DataFrame(data['results'])
```

Oftentimes, the naming conventions pulled from the API are vague. Here, I simply change the column names to be more intuitive:
```
### rename columns
df = df.rename(columns={'v':'trading_volume', 'vw':'volume_weighted_avg_price', 'o':'open_price', 'c':'close_price', 'h':'high', 'l':'low', 't':'unix_timestamp_msec', 'n':'num_transactions'})
```

`pandas`'s `to_datetime()` method enables the user to convert a timestamp from unix to readable datetimes. Here, I add a column to my dataframe called `date_timestamp` that includes a datetime version of the unix timestamp (in Msec).
```
### add column: unix timestamp --> datetime
df['date_timestamp'] = pd.to_datetime(df['unix_timestamp_msec'], unit='ms')
```

### Store pandas dataframe as table in MySQL database



