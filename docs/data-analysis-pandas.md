# Data Analysis with pandas

## Introduction to pandas

* Limitations of NumPy
  * Lack of support for column names
  * Support for only one type of data per ndarray
  * Lack of built-in methods for common analysis patterns
* The **pandas** library provides solutions to all of the pain points above and more. Pandas is an extension of NumPy as the underlying code for pandas uses the NumPy library extensively
* The two core pandas types are dataframes and series
```python
import pandas as pd
f500 = pd.read_csv("f500.csv", index_col=0)
f500.index.name = None
f500_type = type(f500)
f500_shape = f500.shape

# printing information about the  data types of columns
print(f500.dtypes)

# fetching head, tail and info
f500_head= f500.head(6)
f500_tail = f500.tail(8)
f500.info()
```
### Dataframe in pandas
![dataframe](https://github.com/meetbrij/python-notes/blob/master/docs/images/df_anatomy_static.svg "")

* The code above tells us  that our data has 500 rows and 16 columns, and is stored as a pandas.core.frame.DataFrame object. **dataframes** is a primary pandas data structure.
* Dataframes are two dimensional pandas objects, the pandas equivalent of a Numpy 2D ndarray. Unlike NumPy, pandas does not use the same type for 1D and 2D arrays.
* In pandas:
  * Just like a 2D ndarray, there are two axes, however each axis of a dataframe has a specific name. The first axis is called index, and the second axis is called columns.
  * Our axis values have string labels, not just numeric locations - row labels and column labels.
  * Our dataframe contains columns with multiple dtypes: integer, float, and string.
  * If we wanted to view the first few rows of our dataframe, we can use the DataFrame.head() method, which returns the first five rows of our dataframe. The DataFrame.head() method also accepts an optional integer parameter which specifies the number of rows.
  * We can use the DataFrame.tail() method, to shows us the last rows of our dataframe. The DataFrame.tail() method accepts an optional integer parameter to specify the number of rows, defaulting to five.
  * If we wanted to get an overview of all the dtypes used in our dataframe, along with its shape and some extra information, we could use the DataFrame.info() method. Note that DataFrame.info() prints the information, rather than returning it, so we can't assign it to a variable.
  * DataFrame.info() method show us the number of entries in our index (representing the number of rows), a list of each column with their dtype and the number of non-null values, as well as a summary of the different dtypes and memory usage.
* We use the DataFrame.loc[] method for selecting data using column and row labels
* Selecting a single column returns a pandas series. The new series has the same index axis labels as the original dataframe. Examples of selecting rows and columns in dataframes. 
```python
# selecting data in the rank column
f500.loc[:,"rank"]

# selecting data in the country and rank column
f500.loc[:,["country", "rank"]]

# selecting column data by specifying range
# returns all of the columns from the first up until and including the last column in our slice
f500.loc[:,"rank":"profits"]

# more code samples
industries = f500.loc[:, "industry"]
previous = f500.loc[:, ["rank", "previous_rank", "years_on_global_500_list"]]
financial_data = f500.loc[:, "revenues": "profit_change"]
```
* Shortcuts for accessing columns
  * Instead of `df.loc[:,"col1"]` you can use `df["col1"]` to select columns. This works for single columns and lists of
  columns but not for column slices. 
  * Instead of `df.loc[:,"col1"]` you can use `df.col1.` This shortcut does not work for labels that contain spaces or special characters.
```python
# using shortcuts to select columns from dataframes
countries = f500["country"]
revenues_years = f500[["revenues", "years_on_global_500_list"]]
ceo_to_sector = f500.loc[:, "ceo":"sector"]
```
* When you select just one column of a dataframe, you get a new pandas type: a **series object**. Series is the pandas type for one-dimensional objects. Anytime you see a 1D pandas object, it will be a series, and anytime you see a 2D pandas object, it will be a dataframe.
* You can think of a dataframe as being a collection of series objects
* Because a series has only one axis, its axis labels are either the index axis or column axis labels, depending on whether it is representing a row or a column from the original dataframe
* If we make a 2D selection from a dataframe for multiple columns or rows then it will return a dataframe object with the selected columns or rows.
```python
ceos = f500["ceo"]
# examples of series selection
walmart = ceos["Walmart"]
apple_to_samsung = ceos["Apple":"Samsung Electronics"]
oil_companies = ceos[["Exxon Mobil", "BP", "Chevron"]]

# more examples of row and column selection
drink_companies = f500.loc[["Anheuser-Busch InBev","Coca-Cola","Heineken Holding"],:]
big_movers = f500.loc[["Aviva", "HP", "JD.com", "BHP Billiton"],["rank", "previous_rank"]]
middle_companies = f500.loc["Tata Motors":"Nationwide","rank":"country"]
```
* The Series.describe() method returns some descriptive statistics on the data contained within a specific pandas series
* If we use describe() on a column that contains non-numeric values, we get some different statistics.
* DataFrame objects also have a DataFrame.describe() method that returns these same statistics for every column. 
* By default, DataFrame.describe() will return statistics for only numeric columns. If we wanted to get just the object columns, we need to use the `include=['O']` parameter.
```python
revs = f500["revenues"]
print(revs.describe())

revs = f500["revenues"]
print(revs.describe())
profits_desc = f500["profits"].describe()
revenue_and_employees_desc = f500.loc[:,["revenues","employees"]].describe()
all_desc = f500.describe(include=['O'])
```
