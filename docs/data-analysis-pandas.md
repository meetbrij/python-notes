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

### Data Cleaning Basics

* Data scientists commonly spend over half their time cleaning data, so knowing how to clean 'messy' data is an extremely important skill.
* A short history of encodings: 
  * Computers, at their lowest levels, can only understand binary - 0 and 1. Encodings are systems for representing characters in binary. From the early days of computers, the standard for representing text was called ASCII. The ASCII standard specified a set of 128 standard characters - these were letters, numbers and punctuation marks that were used for the English language. For instance, the letter `a` in ASCII is represented as `01100001` in binary.
  * ASCII was very useful for the basic letters and numbers in the english language, but as the popularity of computers spread worldwide, different computer manufacturers created new encodings to encode different characters, like the greek character `α` or the Japanese character `も`. Because there wasn't one standard for encodings, there were problems when you tried to read files saved using one character set with a computer that used a different character set.
  * As a result, there ended up being dozens of different encodings being used throughout the early days of the web. Ideally, the person sending you a file would tell you what encoding it is in, however in reality that rarely happened (and still doesn't). Trying to work out what encoding a file was in was relatively difficult, and you often had to guess.
  * In the last 15 years the number of encodings being commonly used has decreased. This means that the vast majority of files are encoded in one of 2-3 formats.
  * UTF-8 has grown rapidly to be the predominant encoding being used. Because of this trend, the best thing to do if your file has an unknown encoding is to try the most common encodings. The most common encodings are, in order:
    *  UTF-8 (the default for Python)
    *  Latin-1 (also known as ISO-8895-1)
    *  Windows-1251
  * To specify a encoding when reading a CSV file with pandas, simply use the encoding argument within the pandas.read_csv() function, specifying the encoding as a string:
  
```python
# UTF-8 is the default, so you don't need to specify the encoding as UTF-8 
df = pd.read_csv("filename.csv", encoding="UTF-8")
```
* When we view the output of the DataFrame.info() we see that every column is represented as the object type, indicating that they are represented by strings and not numbers.
* Steps for cleaning column labels
  * Remove any whitespace from the start and end of the labels
  * Replace spaces with underscores
  * Remove special characters 
  * Make all labels lowercase
  * Shorten any long column names
* We can access the column axis of a dataframe using the DataFrame.columns attribute. This returns an index object, a special type of NumPy ndarray, with the labels of each column
```python
print(laptops.columns)
```
* We can create a function that uses Python string methods to clean our column labels, and then use list comprehension to apply that function to each label.

```python
def clean_col(col):
    # removing spaces from the start and end of the string
    col = col.strip()
    # removing parenthesis
    col = col.replace("(","")
    col = col.replace(")","")
    
    # replacing long text with abbreviation
    col = col.replace("Operating System","os")
    
    # replacing spaces with underscore
    col = col.replace(" ","_")
    
    # converting column names to lowercase
    col = col.lower()
    return col

laptops.columns = [clean_col(c) for c in laptops.columns]
print(laptops.columns)
```
Whenever we're converting text to numeric data, we can follow this data cleaning workflow:

![cleaningworkflow](https://github.com/meetbrij/python-notes/blob/master/docs/images/cleaning_workflow.svg "")

* The first stage is to explore the data. One of the best ways to do this is to use the Series.unique() method to view all of the unique values in a column.
* Our next stage is to identify patterns and special cases.
* The next stage is to remove the non-digit characters using the `Series.str.replace()` method.
* To can convert (or cast) the column to a numeric dtype we use the `Series.astype()` method. We can use either `int` or `float` as the parameter for the method to convert the column to the respective type.
* Final step is to rename the column. This is an optional step, and can be useful if the non-digit values contains information that helps us understand the data. We can use the `DataFrame.rename()` method to rename specific axis labels using a dictionary with the keys as the old label name, and the values as the new label name. 
* Parts of South America, Africa and Europe use commas instead of periods (decimal commas) for numbers. In this case, we'll have to replace the comman with a period.
* The `Series.str.split()` method accepts an argument n, which controls the maximum number of splits allowed. By using n=1, the method will make a single split on the first whitespace.
* When we want to extract value from the end of the string and not the beginning of the string, we use the `Series.str.rsplit()` method allows to split from the end of the string instead of the front.
* When we want to change multiple values in a column we use `Series.map()`. The most common way to use `Series.map()` is with a dictionary. The keys of our dictionary are the original values in our series, and the corresponding values are what they're updated to. One important thing to remember with `Series.map()` is that if a value from your series doesn't exist as a key in your dictionary, it will convert that value to NaN.
* In pandas null values will be indicated by either NaN or None. To identify which values are missing you can use: the `DataFrame.info()` method and the `DataFrame.isnull()` method. The DataFrame.info() method will print information about the dataframe, including the number of non-null values in each column. n contrast, DataFrame.isnull() returns a boolean dataframe with True and False indications for every value in the dataframe, and then we can use DataFrame.sum() to give us a count of the True values
* We have a few options for how we can handle missing values:
  * Remove any rows that have missing values.
  * Remove any columns that have missing values.
  * Fill the missing values with some other value.
  * Leave the missing values as is.
* The first two options, removing columns and/or rows with missing values is often used when preparing data for machine learning, as machine learning algorithms are unable to be trained on data that includes null values. The methods that we use to remove rows and columns with null values is the `DataFrame.dropna(`) method. As a result, removing columns and rows is commonly known as **dropping**.
* To remove a column, you can use the DataFrame.drop() method along with the axis: `laptops = laptops.drop('storage', axis=1)`

```python
laptops["screen_size"] = laptops["screen_size"].str.replace('"','').astype(float)
laptops.rename({"screen_size": "screen_size_inches"}, axis=1, inplace=True)

print(laptops["ram"].unique())
laptops["ram"] = laptops["ram"].str.replace('GB','').astype(int)
laptops.rename({"ram": "ram_gb"}, axis=1, inplace=True)
dtypes = laptops.dtypes

laptops["weight"] = (laptops["weight"]
                        .str.replace("kgs","")
                        .str.replace("kg", "")
                        .astype(float)
                    )
laptops.rename({"weight": "weight_kg"}, axis=1, inplace=True)

laptops["price_euros"] = (laptops["price_euros"]
                        .str.replace(",",".")
                        .astype(float)
                    )

weight_describe = laptops["weight_kg"].describe()
price_describe = laptops["price_euros"].describe()

# code for extracting the gpu and cpu manufacturer
laptops["gpu_manufacturer"] = (laptops["gpu"]
                                    .str.split(n=1,expand=True)
                                    .iloc[:,0]
                               )

laptops["cpu_manufacturer"] = (laptops["cpu"]
                                    .str.split(n=1,expand=True)
                                    .iloc[:,0]
                               )
                               
# splitting the screen column from end to extract the resolution
screen_res = laptops["screen"].str.rsplit(n=1, expand=True)
# giving the columns string labels makes them easier to work with
screen_res.columns = ["A", "B"]
# for rows where the value of column "B" is null, fill in the
# value found in column "A" for that row
screen_res.loc[screen_res["B"].isnull(), "B"] = screen_res["A"]
print(screen_res.iloc[:10])

# assigning the extracted column to a new screen_resolution column
laptops["screen_resolution"] = screen_res["B"]
print(laptops["screen_resolution"].unique().shape)
print(laptops["screen_resolution"].unique())

# extracting cpu speed
laptops["cpu_speed_ghz"] = (laptops["cpu"]
                            .str.replace("GHz","")
                            .str.rsplit(n=1,expand=True)
                            .iloc[:,1]
                            .astype(float)
                            )
                            
# using max and dict to fix column values
mapping_dict = {
    'Android': 'Android',
    'Chrome OS': 'Chrome OS',
    'Linux': 'Linux',
    'Mac OS': 'macOS',
    'No OS': 'No OS',
    'Windows': 'Windows',
    'macOS': 'macOS'
}
laptops["os"] = laptops["os"].map(mapping_dict)

# getting a count of null values in your dataframe
print(laptops.isnull().sum())

# dropping rows with null values
laptops_no_null_rows = laptops.dropna(axis=0)

# dropping columns with null values
laptops_no_null_cols = laptops.dropna(axis=1)

# replacing missing values
value_counts_before = laptops.loc[laptops["os_version"].isnull(), "os"].value_counts()
laptops.loc[laptops["os"] == "macOS", "os_version"] = "X"
laptops.loc[laptops["os"] == "No OS", "os_version"] = "Version Unknown"
value_counts_after = laptops.loc[laptops["os_version"].isnull(), "os"].value_counts()

```
* Assignment: splitting the storage column in 4 - storage_1_capacity_gb, storage_1_type, storage_2_capacity_gb, storage_2_type
```python
counts_before = laptops["storage"].value_counts()
laptops["storage"] = laptops["storage"].str.replace("+","")

# combining flash storage
laptops["storage"] = laptops["storage"].str.replace("Flash Storage","Flash_Storage")
laptops["storage"] = laptops["storage"].str.replace("GB","")

# converting TB to GB
laptops["storage"] = laptops["storage"].str.replace("TB","000")
storage = laptops["storage"].str.split(expand=True)
laptops["storage_1_capacity_gb"] = storage.iloc[:,0].astype(float)
laptops["storage_1_type"] = storage.iloc[:,1]
laptops["storage_1_type"] = laptops["storage_1_type"].str.replace("Flash_Storage","Flash Storage")
laptops["storage_2_capacity_gb"] = storage.iloc[:,2].astype(float)
laptops["storage_2_type"] = storage.iloc[:,3]
laptops["storage_2_type"] = laptops["storage_2_type"].str.replace("Flash_Storage","Flash Storage")
```
* The above code can also be written as
```python
# replace 'TB' with 000 and rm 'GB'
laptops["storage"] = laptops["storage"].str.replace('GB','').str.replace('TB','000')

# split out into two columns for storage
laptops[["storage_1", "storage_2"]] = laptops["storage"].str.split("+", expand=True)

for s in ["storage_1", "storage_2"]:
    s_capacity = s + "_capacity_gb"
    s_type = s + "_type"
    # create new cols for capacity and type
    laptops[[s_capacity, s_type]] = laptops[s].str.split(n=1,expand=True)
    # make capacity numeric (can't be int because of missing values)
    laptops[s_capacity] = laptops[s_capacity].astype(float)
    # strip extra white space
    laptops[s_type] = laptops[s_type].str.strip()

# remove unneeded columns
laptops.drop(["storage", "storage_1", "storage_2"], axis=1, inplace=True)
```
* Re-ordering columns and saving the output to CSV
```python
laptops_dtypes = laptops.dtypes

# expected order of columns
cols = ['manufacturer', 'model_name', 'category', 'screen_size_inches',
        'screen', 'cpu', 'cpu_manufacturer', 'screen_resolution', 'cpu_speed_ghz', 'ram_gb',
        'storage_1_type', 'storage_1_capacity_gb', 'storage_2_type',
        'storage_2_capacity_gb', 'gpu', 'gpu_manufacturer', 'os',
        'os_version', 'weight_kg', 'price_euros']

# reordering columns using the cols array
laptops = laptops[cols]

# saving data to CSV file
laptops.to_csv('laptops_cleaned.csv',index=False)

#loading the new CSV
laptops_cleaned = pd.read_csv('laptops_cleaned.csv')
laptops_cleaned_dtypes = laptops_cleaned.dtypes
```
* Analyzing Data further to answer the following questions:
* Are laptops made by Apple more expensive than those by other manufacturers?
* What is the best value laptop with a screen size of 15" or more?
* Which laptop has the most storage space?

### Guided Project: Exploring Ebay Car Sale Data
* The aim of this project is to clean the data and analyze the included used car listings. 
* 
