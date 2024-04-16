
---
## # Usage of this Document

introduction to 
- preparation for using pandas-library
- pandas commands & explanations

---
## # Preparations

### # Import Module

import Pandas module

```python
import pandas as pd
```

### # Create DataFrame

create dataframe (from scratch) & pass it to pandas dataframe constructor
- **{key : value}**

```python
data = {
		'apples': [3, 2, 0, 1],
		'oranges': [0, 3, 7, 2]
}

purchases = pd.DataFrames(data)
```

As you can see - the **Index** of this dataframe was given to us on creation as the numbers (0-3).
We could also _create own index when initializing the dataframe_:

```python
purchases = pd.DataFrame(data, index=['June', 'Robert', 'Lily', 'David'])
```
---
## # Read data

It is quite simple to load data from various file formats into a dataframe. 

### # from CSV

note: a CSV doesn't have an index like our dataframes

```python
df = pd.read_csv('purchases.csv')
```

assign index:

```python
df = pd.read_csv('purchases.csv', index_col=0)
```
---
### # from JSON

is a stored python `dict` - pandas can read this easily

```python
df = pd.read_json('purchases.json')
```
---
### # from SQL Database

first: establish connection using specific python library.
Here, we use SQLite as DB.

Make sure to install `pysqlite3` / `psycopg2`.

```python
import sqlite3

con = sqlite3.connect("database.db")
```

```python
df = pd.read_sql_query("SELECT * FROM purchases", con)

# set name as index
df = df.set_index('index')
```
---
### # Convert back - CSV, JSON, SQL

```python
# CSV
df.to_csv('new_purchases.csv')
# JSON
df.to_json('new_purchases.json')
# SQL
df.to_sql('new_purchases', con)
```

----
# # Most important DataFrame Operations
## # View Data
### # .head() .tail()

print out a few rows as visual reference
- `.head()`: prints first 5 rows (per default)
- `.tail()`: prints last 5 rows (per default)

```python
df.head()
df.tail()
```
---
## # Getting Info about Data
### # .info()

provides essential information about your dataset 
- number of rows & columns
- number of non-null values
- type of data
- used-memory of df

```python
df.info()

# Output:
<class 'pandas.core.frame.DataFrame'>
Index: 1000 entries, Guardians of the Galaxy to Nine Lives
Data columns (total 11 columns):
 #   Column              Non-Null Count  Dtype  
---  ------              --------------  -----  
 0   Rank                1000 non-null   int64  
 1   Genre               1000 non-null   object 
 2   Description         1000 non-null   object
	...
dtypes: float64(3), int64(4), object(4)
memory usage: 93.8+ KB
```
---
### # .shape()

gets dimensions of a dataframe
- it returns a tuple representing the number of rows & columns

```python
df.shape()

# Output: (1000, 11)
```
---
### # .append()

returns a copy of original dataframe + appended data

```python
temp = df.append(df)
```
---
## # Handling Duplicates
### # .drop_duplicates

returns a copy of dataframe, but removing duplicates

```python
temp = temp.drop_duplicates()

temp = temp.drop_duplicates(inplace=True)
# removes duplicates, but applies this directly to df - not returning a copy

temp = temp.drop_duplicates(inplace=True, keep=first/last/False/)
# removes duplicates, but applies this directly to df - no copy - 
# keeps first(default)/last/no one
```
---
## # Column Cleanup
### # .columns

prints column names of dataset.

```python
df.columns

# Output: 
Index(['Rank', 'Genre', 'Description', 'Director', 'Actors', 'Year',
       'Runtime (Minutes)', 'Rating', 'Votes', 'Revenue (Millions)',
       'Metascore'],
      dtype='object')
```

### # .rename(_column={specific-column}_)

to rename specific columns.

```python
df.rename(columns={
        'Runtime (Minutes)': 'Runtime', 
        'Revenue (Millions)': 'Revenue_millions'
    }, inplace=True)
```


**List/Dict Comprehension**

```python
df.columns = [col.lower() for col in movies_df]
```
---
## # Working with missing Values

There are two options when dealing with null-values:
1. Get rid of rows/columns with nulls
2. Replace nulls with non-null values = "**Imputation**"

### # .isnull()

returns a df, where each cell is either True/False (depending on cell's null status).

```python
df.isnull()

# To count number of nulls in each column we use an aggregate function for summing
df.isnull().sum()
```

### .dropna()

deletes **any** row that contains a single null value.
it also returns a new df without changing the original one (`inplace=True` could be used).

```python
df.dropna()

# only drop columns with null values
df.dropna(axis=1)
```
---
## # Imputation

used to keep valuable data that have null values. 

### # .fillna()

fills null values.

```python
# extracting column from df & storing it in variable
revenue = movies_df['revenue_millions']

# filling missin values with mean value
revenue_mean = revenue.mean()
revenue.fillna(revenue_mean, inplace=True)
```
---
## # Understand Variables

### # .describe()

to get a summary of distribution of variables.

```python
df.describe

# get description of specific variable
df['genre'].describe()
```

### # .value_counts()

tell us frequency of all values in a column.

```python
df['genre'].value_counts().head(10)
```

### # .corr()

'correlation' method. 
generate relationship between each continuous variable.

```python
df.corr()
```
---
## # DataFrame slicing, selecting, extracting

### # Slicing of Columns

extract values:

```python
genre_col = df['genre']
# this returns a Series

# - to extract a column as df - you need to pass a list of column names
genre_col = df[['genre']]
```

### # Slicing of Rows

### # .loc[]/.iloc[]

`.loc[]` - **loc**ates by name
`.iloc[]` - **loc**ates by numerical **i**ndex

```python
prom = df.loc["Prometheus"]

prom = df.iloc[1]
```

---
