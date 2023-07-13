# IBM_DataAnalysis
## Introduction:
The following code implements a Data Acquisition, Pre-processing, and Data Analysis Pipeline using Python. It utilizes an API provided by AlphaVantage to acquire stock data for IBM. The pipeline includes steps for data pre-processing, loading the pre-processed data into an SQLite database, and performing data analysis tasks based on the acquired data. The code demonstrates examples of data analysis tasks and visualizations using the stored data.

## Code:
**Step 1: Data Acquisition**
The code begins by acquiring stock data for IBM from the AlphaVantage API using a provided URL. The received data is then pre-processed, converting it into a pandas DataFrame for easier manipulation. This includes renaming columns, converting data types, and formatting timestamps.

**Step 2: Data Pre-processing**
The pre-processed data is loaded into an SQLite database for storage and further analysis. A connection to the SQLite database is established using SQLAlchemy, and the DataFrame is stored in the specified database table using the `to_sql` method.

**Step 3: Loading into a database (SQLite)**
For data analysis, the code includes several examples. The average closing price is calculated by executing an SQL query and provides insight into the overall price trend. Similarly, the maximum trading volume in a single 5-minute interval is determined using another SQL query.

**Step 4: Data Analysis**
The code also includes visualizations to aid in understanding the data. The closing prices over time are plotted as a line graph, offering a visual representation of the stock's performance. The volume traded over time is displayed in another line graph, indicating periods of high and low trading activity. Additionally, a histogram is created to illustrate the distribution of closing prices, allowing for an understanding of their frequency and spread.

## Conclusion:
The provided code showcases the implementation of a Data Acquisition, Pre-processing, and Data Analysis Pipeline. It demonstrates how to acquire data from an API, pre-process it, load it into an SQLite database, and perform data analysis tasks using SQL queries. Visualizations are used to gain insights and provide a graphical representation of the analyzed data. This pipeline can be extended with additional data analysis tasks and visualizations based on specific questions and interests.
