# IBM_DataAnalysis
## Introduction:
The following code implements a Data Acquisition, Pre-processing, and Data Analysis Pipeline using Python. It utilizes an API provided by AlphaVantage to acquire stock data for IBM. The pipeline includes steps for data pre-processing, loading the pre-processed data into an SQLite database, and performing data analysis tasks based on the acquired data. The code demonstrates examples of data analysis tasks and visualizations using the stored data.

## Code:
> Necessary Libraries Imported
> ```python
> import requests
> import pandas as pd
> import matplotlib.pyplot as plt
> import seaborn as sns
> from sqlalchemy import create_engine
> ```

**Step 1: Data Acquisition**

The code begins by acquiring stock data for IBM from the AlphaVantage API using a provided URL. 
```python
url = 'https://www.alphavantage.co/query?function=TIME_SERIES_INTRADAY&symbol=IBM&interval=5min&outputsize=full&apikey=AFIY6TARCA5RULA7'
response = requests.get(url)
data = response.json()
```

The received data is then pre-processed, converting it into a pandas DataFrame for easier manipulation. This includes renaming columns, converting data types, and formatting timestamps.
```python
# Convert the nested JSON data to a pandas DataFrame
df = pd.DataFrame(data['Time Series (5min)']).T

# Rename columns for clarity
df.columns = ['Open', 'High', 'Low', 'Close', 'Volume']

# Convert string values to appropriate data types
df = df.astype({'Open': float, 'High': float, 'Low': float, 'Close': float, 'Volume': int})

# Convert the index (timestamps) to datetime format
df.index = pd.to_datetime(df.index)
```

**Step 2: Data Pre-processing**

The pre-processed data is loaded into an SQLite database for storage and further analysis. A connection to the SQLite database is established using SQLAlchemy, and the DataFrame is stored in the specified database table using the `to_sql` method.
```python
# Establish a connection to the SQLite database
engine = create_engine('sqlite:///stock_data.db')

# Store the DataFrame in the database table
table_name = 'stock_data'
df.to_sql(table_name, engine, if_exists='replace')
```

**Step 3: Loading into a database (SQLite)**

For data analysis, the code includes several examples. The average closing price is calculated by executing an SQL query and provides insight into the overall price trend. Similarly, the maximum trading volume in a single 5-minute interval is determined using another SQL query.
```python
# Calculate the average closing price over the entire dataset
average_closing_price_query = f"SELECT AVG(Close) FROM {table_name}"
average_closing_price = engine.execute(average_closing_price_query).scalar()
print("Average Closing Price: ", average_closing_price)

# Calculate the maximum volume traded in a single 5-minute interval
max_volume_query = f"SELECT MAX(Volume) FROM {table_name}"
max_volume = engine.execute(max_volume_query).scalar()
print("Maximum Volume: ", max_volume)
```

**Step 4: Data Analysis**

The code also includes visualizations to aid in understanding the data. The closing prices over time are plotted as a line graph, offering a visual representation of the stock's performance. The volume traded over time is displayed in another line graph, indicating periods of high and low trading activity. Additionally, a histogram is created to illustrate the distribution of closing prices, allowing for an understanding of their frequency and spread.
```python
# Visualize the closing prices over time
closing_prices_query = f"SELECT [index], Close FROM {table_name}"
closing_prices = pd.read_sql(closing_prices_query, engine, parse_dates=['index'])
plt.figure(figsize=(12, 6))
plt.plot(closing_prices['index'], closing_prices['Close'])
plt.title('Closing Prices Over Time')
plt.xlabel('Date')
plt.ylabel('Closing Price')
plt.grid(True)
plt.show()
Inline-style: 
![alt text](https://drive.google.com/file/d/19OxuOg11eqqaOEbksMTayx7qLtygr5k-/view?usp=drive_link "Logo Title Text 1")

# Visualize the volume traded over time
volume_query = f"SELECT [index], Volume FROM {table_name}"
volume = pd.read_sql(volume_query, engine, parse_dates=['index'])
plt.figure(figsize=(12, 6))
plt.plot(volume['index'], volume['Volume'])
plt.title('Volume Traded Over Time')
plt.xlabel('Date')
plt.ylabel('Volume')
plt.grid(True)
plt.show()

# Visualize the distribution of closing prices
plt.figure(figsize=(10, 6))
sns.histplot(closing_prices['Close'], bins=30, kde=True)
plt.title('Distribution of Closing Prices')
plt.xlabel('Closing Price')
plt.ylabel('Frequency')
plt.grid(True)
plt.show()
```

## Conclusion:
The provided code showcases the implementation of a Data Acquisition, Pre-processing, and Data Analysis Pipeline. It demonstrates how to acquire data from an API, pre-process it, load it into an SQLite database, and perform data analysis tasks using SQL queries. Visualizations are used to gain insights and provide a graphical representation of the analyzed data. This pipeline can be extended with additional data analysis tasks and visualizations based on specific questions and interests.
