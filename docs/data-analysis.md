# Data Analysis and Visualization

## Introduction to Numpy

* NumPy and pandas provide a powerful toolset for working with data in Python. They allow us to write code quickly without sacrificing performance. 
* NumPy uses vectorization to make your performance faster.
* When we use a for-loop to iterate through a list python interpretor will go through each and every item in the list. So if our list has 1000 rows then it would take 1000 processor cycles to process 1000 rows of data.
* Vectorization takes advantage of a processor feature called Single Instruction Multiple Data (SIMD) to process data faster. SIMD allows a processor to perform the same operation, on multiple data points, in a single processor cycle.
* Vectorized operations might process as little as two and as many as as hundreds of operations per processor cycle, depending on the capabilities of the processor and the size of each data point. So instead of processing individual rows it can process multiple (say 100) rows in one go thus reducing the number of processing cycles. 
* NumPy and pandas takes care of SIMD and processor cycles for you.
* ndarray stands for 'n-dimensional array'. N-dimensional refers to the fact that ndarrays can have one or more dimensions.
  * One-dimensional arrays are commonly known as array, list, vector or sequence
  * Two-dimensional arrays are commonly known as matrix, table, list of lists, spreadsheets
  * Three-dimensional arrays are known as multi dimensional arrays or panels
* Similar to using lists of lists, we use numbers to specify the location of elements of our data that we want to work with. Just like with lists, we call these numbers index values
* Unlike with Python lists, every value in an ndarray must be of the same types.
* If we wanted to see the how many rows and columns are in our ndarray, we can use the ndarray.shape
* If we just want to select a number of rows from an ndarray, we can use slicing, just like we would with a list of lists.
```python
import csv
import numpy as np

# import nyc_taxi.csv as a list of lists
f = open("nyc_taxis.csv", "r")
taxi_list = list(csv.reader(f))
taxi_list = taxi_list[1:]

# convert all values to floats
converted_taxi_list = []
for row in taxi_list:
    converted_row = []
    for item in row:
        converted_row.append(float(item))
    converted_taxi_list.append(converted_row)

# start writing your code below this comment
taxi = np.array(converted_taxi_list)
print(taxi.shape)
taxi_ten = taxi[:10]
print(taxi_ten)

# selecting rows
row_0 = taxi[0]
rows_391_to_500 = taxi[391:501]
row_21_column_5 = taxi[21,5]

# selecting columns

```
* 
