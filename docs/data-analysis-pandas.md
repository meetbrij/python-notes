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
```
* The code above tells us  that our data has 500 rows and 16 columns, and is stored as a pandas.core.frame.DataFrame object. **dataframes** is a primary pandas data structure.
* Dataframes are two dimensional pandas objects, the pandas equivalent of a Numpy 2D ndarray. Unlike NumPy, pandas does not use the same type for 1D and 2D arrays.
* In pandas:
  * Just like a 2D ndarray, there are two axes, however each axis of a dataframe has a specific name. The first axis is called index, and the second axis is called columns.
  * Our axis values have string labels, not just numeric locations - row labels and column labels.
  * Our dataframe contains columns with multiple dtypes: integer, float, and string.
  * If we wanted to view the first few rows of our dataframe, we can use the DataFrame.head() method, which returns the first five rows of our dataframe. The DataFrame.head() method also accepts an optional integer parameter which specifies the number of rows.
  * We can use the DataFrame.tail() method, to shows us the last rows of our dataframe. The DataFrame.tail() method accepts an optional integer parameter to specify the number of rows, defaulting to five.
  * If we wanted to get an overview of all the dtypes used in our dataframe, along with its shape and some extra information, we could use the DataFrame.info() method. Note that DataFrame.info() prints the information, rather than returning it, so we can't assign it to a variable.
