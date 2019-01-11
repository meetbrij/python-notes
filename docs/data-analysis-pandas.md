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

# find the median value for the revenues and profits columns
medians = f500[["revenues", "profits"]].median(axis=0)
# we could also use .median(axis="index")
print(medians)

# using series value counts
f500["sector"].value_counts().head()
```
* The Series.value_counts() method displays each unique non-null value from a series, with a count of the number of times that value is used.
```python
top5_countries = f500["country"].value_counts().head()
top5_previous_rank = f500["previous_rank"].value_counts().head()

# finding the maximum value from the numeric columns only
max_f500 = f500.max(numeric_only=True)
```
* When we select a column by label and use assignment, we assign the value to every item in that column.
* If we assign a value using a index or column label that does not exist, pandas will create a new row or column in our dataframe.
```python
# adding a new revenues_b column to the f500 dataframe
f500["revenues_b"] = f500["revenues"] / 1000

# modifying a specific cell in the dataframe
f500.loc["Dow Chemical", "ceo"] = "Jim Fitterling"
```
* Code for using boolean indexing with our Fortune 500 dataset to find out which are the 5 most common countries for companies belonging to the 'Motor Vehicles and Parts' industry.
```python
# finding rows with value 'Motor Vehicles and Parts'
motor_bool = f500["industry"] == "Motor Vehicles and Parts"

# using boolean series to select countries which are true 
motor_countries = f500.loc[motor_bool, "country"]

# filtering the top 5 countries for the 'Motor Vehicles and Parts' industry
top5_motor_countries = motor_countries.value_counts().head()

# filtering the top 5 companies in South Korea
kr_bool = f500["country"] == "South Korea"
top_5_kr = f500[kr_bool].head()

# changing the 'Motor Vehicles & Parts' values in the sector column to 'Motor Vehicles and Parts'
f500.loc[f500["sector"] == "Motor Vehicles & Parts", "sector"] = "Motor Vehicles and Parts"

# replacing the values in previous_rank column from 0 to np.nan
# using dropna=False to stop the Series.value_counts() method from excluding null values when it makes its calculation
import numpy as np
prev_rank_before = f500["previous_rank"].value_counts(dropna=False).head()
f500.loc[f500["previous_rank"] == 0, "previous_rank"] = np.nan
prev_rank_after = f500["previous_rank"].value_counts(dropna=False).head()

# identifying the 3 most common countries in the dataframe
top_3_countries = f500["country"].value_counts().head(3)

# counting 5 common cities head quartered in USA
cities_usa = f500.hq_location[f500["country"] == "USA"].value_counts().head()

# counting 3 common sectors for companies headquartered in China
sector_china = f500.sector[f500["country"] == "China"].value_counts().head(3)

# finding mean average number of employees headquartered in Japan
mean_employees_japan = f500.employees[f500["country"] == "Japan"].mean()
```
* Because pandas uses NumPy objects behind the scenes to store the data, the integer positions we used to select data can also be used. To select data by integer position using pandas we use the `Dataframe.iloc[]` method and the `Series.iloc[]` method.
  * While using slicing, with `loc[]`, the ending slice is included.
  * While using slicing, with `iloc[]`, the ending slice is not included.
* Table below for selecting integer by position
![dataframe](https://github.com/meetbrij/python-notes/blob/master/docs/images/integer-selection-pandas.png "")
* While loading csv file
  * The `index_col` parameter specifies which column to use as the row labels. We use a value of 0 to specify that we want to use the first column.
  * Next we use DataFrame.index to access the index axes attribute, and then we use index.name to access the name of the index axes. By setting this to None we remove the name
* Always think carefully and deliberately about whether you want to select by label or integer position, and use `DataFrame.loc[]` or `DataFrame.iloc[]` accordingly.
* Boolean Mask methods
  * Series.str.startswith()
  * Series.str.endswith()
  * Series.str.isnull()
  * Series.str.notnull()
  * Series.str.contains()

```python
import pandas as pd
import numpy as np

f500 = pd.read_csv("f500.csv", index_col=0)
f500.index.name = None
f500.loc[f500["previous_rank"] == 0, "previous_rank"] = np.nan

# selecting a single row at index 3
fourth_row = f500.iloc[3]

# selecting the first column
first_column = f500.iloc[:,0]

# selecting a slice of row
second_to_sixth_rows = f500[1:5]

# selecting fifth row
fifth_row = f500.iloc[4]

# selecting the first 3 rows
first_three_rows = f500[0:3]

# selecting the first and seventh row for first to fifth columns
first_seventh_row_slice = f500.iloc[[0,6],0:5]

# sorting employees and finding top 5
sorted_emp = f500.sort_values("employees", ascending=False)
top5_emp = sorted_emp.iloc[0:5]

# selecting companies head quartered in USA
usa_hqs = f500.loc[f500["country"] == "USA", "hq_location"]
print(usa_hqs.head())

# creating boolean mask using Series.str.endswith() to find companies head quartered in California (CA)
usa = f500.loc[f500["country"] == "USA"]
is_california = usa["hq_location"].str.endswith("CA")
print(is_california.head())

# using the boolean mask for filtering values
california = usa[is_california]
print(california.iloc[:5,:3])

# using Series.str.isnull() to find companies with missing values
rev_change_null = f500[f500["revenue_change"].isnull()]
print(rev_change_null[["company","country","sector"]])

# selecting all rows from f500 that have a non null value in the previous_rank column
previously_ranked = f500[f500["previous_rank"].notnull()]

# finding rank change by subtracting the previous_rank column from the rank column
rank_change = previously_ranked["rank"] - previously_ranked["previous_rank"]

# appending the rank_change column to f500 dataframe
# for labels that does not have value in rank_change dataframe it will be populated as NaN
f500["rank_change"] = rank_change

# finding companies which have over 265 B in revenue and headquartered in China
cols = ["company", "revenues", "country"]
f500_sel = f500[cols]
over_265 = f500_sel["revenues"] > 265000
china = f500["country"] == "China"
combined = over_265 & china
final_cols = ["country", "revenue"]
result = f500_sel.loc[combined, final_cols]

# companies with revenue > 100B and negative profits
over_100 = f500["revenues"] > 100000
profits = f500["profits"] < 0
combined = over_100 & profits
big_rev_neg_profit = f500[combined]

# companies headquartered outside USA in the technology sector 
tech_sel = f500["sector"] == "Technology"
nonusa_hqs = f500["country"] == "USA"
tech_outside_usa = f500[tech_sel & ~nonusa_hqs].head() 

```
### Loops in Pandas

* When you attempt to loop over a dataframe in pandas, it returns the column index labels, rather than the rows as we might expect. There are pandas methods to help loop over dataframes.
* One scenario where it is useful to use loops with pandas is when we are performing aggregation. 
* Aggregation is where we apply a statistical operation to groups of our data. 
* Below is the code to work out what the average revenue was for each country in the data set
```python
# Create an empty dictionary to store the results
avg_rev_by_country = {}

# Create an array of unique countries
countries = f500["country"].unique()

# Use a for loop to iterate over the countries
for c in countries:
    # Use boolean comparison to select only rows that
    # correspond to a specific country
    selected_rows = f500[f500["country"] == c]
    
    # Calculate the mean average revenue for just those rows
    mean = selected_rows["revenues"].mean()
    
    # Assign the mean value to the dictionary, using the
    # country name as the key
    avg_rev_by_country[c] = mean
    
# finding top employers by country
top_employer_by_country = {}

for c in countries:
    selected_rows = f500[f500["country"] == c]
    sorted_rows = selected_rows.sort_values(by=['employees'], ascending=False)
    
    top_employer = sorted_rows.iloc[0]
    employer_name = top_employer["company"]
    
    top_employer_by_country[c] = employer_name

# finding companies with the highest ROA in each sector
f500["roa"] = f500["profits"] / f500["assets"]

top_roa_by_sector = {}
for sector in f500["sector"].unique():
    is_sector = f500["sector"] == sector
    sector_companies = f500.loc[is_sector]
    top_company = sector_companies.sort_values("roa",ascending=False).iloc[0]
    company_name = top_company["company"]
    top_roa_by_sector[sector] = company_name
```
