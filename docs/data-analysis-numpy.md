# Data Analysis with NumPy

## Introduction to NumPy

* NumPy and pandas provide a powerful toolset for working with data in Python. They allow us to write code quickly without sacrificing performance.
* NumPy uses vectorization to make your performance faster.
* NumPy makes it quick and easy to make selections of our data, and includes a number of functions and methods that make it easy to calculate statistics across the different axes (or dimensions).
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
cols = [1,4,7]
columns_1_4_7 = taxi[:,cols]
row_99_columns_5_to_8 = taxi[99, 5:9]
rows_100_to_200_column_14 = taxi[100:201, 14]

# calculating miles per hour using vector math
trip_distance_miles = taxi[:,7]
trip_length_seconds = taxi[:,8]

trip_length_hours = trip_length_seconds / 3600 #3600 seconds is one hour
trip_mph = trip_distance_miles / trip_length_hours

# calculating mean, max, min
mph_min = trip_mph.min()
mph_max = trip_mph.max()
mph_mean = trip_mph.mean()

```
* Adding two columns (or vectors) together is called vector math. When we're performing vector math on two one-dimensional vectors, both vectors must have the same shape.
* Code for adding 2 columns (vectors) of data
```python
# convert the list of lists to an ndarray
my_numbers = np.array(my_numbers)

# select each of the columns - the result
# of each will be a 1D ndarray
col1 = my_numbers[:,0]
col2 = my_numbers[:,1]

# add the two columns
sums = col1 + col2
```
* If we wanted to find the maximum value of each row then we need to use the axis parameter, and specify a value of 1, which indicates we want to calculate values for each row. And ff we want to find the maximum value of each column, we use an axis value of 0
```python
ndarray.max(axis=1) # finding max in a row
ndarray.max(axis=0) # finding max in a column

# Let's say that we wanted to do some validation, and check that the total_amount column is accurate.
# We want to perform a check of whether the first 4 of these columns sums to the 5th column
# we'll compare against the first 5 rows only
taxi_first_five = taxi[:5]
# select these columns: fare_amount, fees_amount, tolls_amount, tip_amount
fare_components = taxi_first_five[:,9:13]
# select the total_amount column
fare_totals = taxi_first_five[:,13]

# sum the component columns
fare_sums = fare_components.sum(axis=1)

# compare the summed columns to the fare_totals
print(fare_totals.round())
print(fare_sums)
```
*  To add rows and columns to an ndarray we're going to use the numpy.concatenate() function. This function accepts:
   * A list of ndarrays as the first, unnamed parameter.
   * An integer for the axis parameter, where 0 will add rows and 1 will add columns.
* The numpy.concatenate() function requires that each array have the same shape, excepting the dimension corresponding to axis. In order to adjust the shape of our array, we can use the numpy.expand_dims() function. We'll start by passing axis=0 because we want to convert our 1D array into a 2D array representing a row.
```python
ones = []
ones[0] = [1, 1, 1]
ones[1] = [1, 1, 1]
zeros = [0, 0, 0]
print(ones.shape) # (2,3)
print(zeros.shape) # (3,)

# adding zeros as row to ones
zeros_2d = np.expand_dims(zeros,axis=0)
print(zeros_2d) # [[0, 0, 0]]
print(zeros.shape) # (1, 3)
combined = np.concatenate([ones,zeros_2d],axis=0)
print(combined)

# adding zeros as column to ones
zeros_2d = np.expand_dims(zeros,axis=1)
print(zeros_2d) # [[0], [0], [0]]
print(zeros_2d.shape) # (3,1)
combined = np.concatenate([ones,zeros_2d],axis=1)
print(combined)

# adding the trip_mph column to the end of taxi table
trip_mph_2d = np.expand_dims(trip_mph,axis=1)
taxi = np.concatenate([taxi,trip_mph_2d],axis=1)
print(taxi)
```
* To sort the array  we'll use the numpy.argsort() function. The numpy.argsort() function returns the indices which would sort an array.
```python
colors = np.array(['red', 'green', 'black', 'yellow', 'blue', 'orange', 'pink'])
sorted_order = np.argsort(colors)
sorted_colors = colors[sorted_order]
```
* Sorting a 2D array by its last column
```python
int_square = []
int_square[0] = [5, 2, 8, 3, 4]
int_square[1] = [2, 8, 6, 2, 5]
int_square[2] = [1, 6, 2, 7, 7]
int_square[3] = [0, 7, 7, 4, 5]
int_square[4] = [5, 7, 1, 1, 2]
last_column = int_square[:,4]
print(last_column) # [4 5 7 5 2]

sorted_order = np.argsort(last_column)
print(sorted_order) # [4 0 1 3 2]

last_column_sorted = last_column[sorted_order]
print(last_column_sorted) # [2 4 5 5 7]

int_square_sorted = int_square[sorted_order]
print(int_square_sorted)

# sorting the taxi table by last column trip_mph
last_column = taxi[:,15]
sorted_order = np.argsort(last_column)
last_column_sorted = last_column[sorted_order]
taxi_sorted = taxi[sorted_order]
print(taxi_sorted)
```
## Boolean Indexing

* The numpy.genfromtxt() function reads a text file into a NumPy ndarray. While it has over 20 parameters, for most cases you need only two.
```python
import numpy as np
# np.genfromtxt(filename,delimiter)
taxi = np.genfromtxt('nyc_taxis.csv', delimiter=',')
print(taxi)
taxi = taxi[1:] # removing header row
```
* Using boolean arrays to index and select data from ndarrays. A boolean array, as the name suggests is an array full of boolean values. Boolean arrays are sometimes called boolean vectors or boolean masks.
* When we perform a boolean operation between an ndarray and a scalar, the operation is applied to each value in the array resulting in a new ndarray of boolean values.
```python
print(np.array([2,4,6,8]) < 5) # [True True False False]
color = np.array(["blue", "blue", "red", "blue"])
color_bool = (color == "blue")
```
* The last aspect is to understand how to index (or select) using boolean arrays. This is known as boolean indexing. To index using our new boolean array, we simply insert it in the square brackets, just like we would do with our other selection techniques. The boolean array acts as a filter, and the values that correspond to True become part of the resultant ndarray, where the the values that correspond to False are removed.
* Using boolean indexing to find out the number of rides in our data set (taxi) for January, February and March.
```python
# extracting the pickup_month column from taxi ndarray
pickup_month = taxi[:,1]

# extracting a boolean array for the month of jan, feb and march
january_bool = pickup_month == 1
february_bool = pickup_month == 2
march_bool = pickup_month == 3

# Using boolean array for filtering out the months
january = pickup_month[january_bool]
february = pickup_month[february_bool]
march = pickup_month[march_bool]

# find out the number of rides for the respective months
january_rides = january.shape[0]
february_rides = february.shape[0]
march_rides = march.shape[0]
```
* Using Boolean indexing to sort and view the trips that had very large average speeds.
```python
# calculate the average speed
trip_mph = taxi[:,7] / (taxi[:,8] / 3600)

# create a boolean array for trips with average speeds greater than 20,000 mph
trip_mph_bool = trip_mph > 20000

# use the boolean array to select the rows for those trips, and the pickup_location_code,
# dropoff_location_code, trip_distance, and trip_length columns
trips_over_20000_mph = taxi[trip_mph_bool,5:9]

print(trips_over_20000_mph)

# extracting rows with hightest value of tip amount - tips over 50
tip_amount = taxi[:,12]
tip_bool = tip_amount > 50
top_tips = taxi[tip_bool, 5:14]
```
* Modifying ndarrays
```python
# this creates a copy of our taxi ndarray
taxi_modified = taxi.copy()

# changing the value at column index 5 and row index 28214 to 1
taxi_modified[28214, 5] = 1

# changing the value at column index 0 for all rows to 16
taxi_modified[:,0] = 16

# changing the value at column index 7 and row index 1800, 1801 to the mean of column index 7
taxi_modified[1800:1802,7] = taxi_modified[:,7].mean()
```
* The pattern `c[c[:,1] > 2, 1]` in the code below, where a 1D boolean array is used to specify assignment in the row dimension and an index value is used to specify which column the array applies to is a common way of using Boolean indexing.
```python
a = np.array([1, 2, 3, 4, 5])
a[a > 2] = 9
print(a) # [ 1 2 99 99 99]

# with multi-d arrays
b = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])

b[b > 4] = 99
print(b)
# [[ 1  2  3]
#  [ 4 99 99]
#  [99 99 99]]

c = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])
c[c[:,1] > 2, 1] = 99
print(c)
# [[ 1  2  3]
#  [ 4 99  6]
#  [ 7 99  9]]
```
* Code for finding out which airport is the most popular destination in our data set
```python
# Selecting rows where the dropoff_location_code value is JFK
jfk = (taxi[taxi[:,6] == 2])
jfk_count = jfk.shape[0]

# Selecting rows where the dropoff_location_code value is Laguardia
laguardia = (taxi[taxi[:,6] == 3])
laguardia_count = laguardia.shape[0]

# Selecting rows where the dropoff_location_code value is Newark
newark = (taxi[taxi[:,6] == 5])
newark_count = newark.shape[0]

trip_mph = taxi[:,7] / (taxi[:,8] / 3600)

# creating cleaned_taxi containing only rows
# for which the values of trip_mph are less than 100.
cleaned_taxi = taxi[trip_mph<100]

mean_distance = cleaned_taxi[:,7].mean()
mean_length = cleaned_taxi[:,8].mean()
mean_total_amount = cleaned_taxi[:,13].mean()

# Calculating mean of the trip_mph excluding  
# values greater than or equal to 100,
mean_mph = trip_mph[trip_mph < 100].mean()
```

### Additional methods
* Use hsplit and vsplit to split numpy dataframes
```python
import numpy as np
taxi = np.genfromtxt('nyc_taxis.csv', delimiter=',')[1:]

# splitting dataframe horizontally in 5 columns
lst = np.hsplit(taxi, 5)

# splitting dataframe vertically in 4 rows
lst = np.vsplit(taxi, 4)
```

### Working with Images in Python

Python can also do image processing like Photoshop. You can use the image processing capabilities of Python to detect faces from photos from images as well as moving objects from vidoes. It stores and reads images using arrays of numbers. We can use numpy to work with images in Python. Images have pixels stored in 2 dimensions or 2 dimensional array. 

We can use OpenCV image processing library to work with images in Python. OpenCV is also referred to as cv2 in Python.

```python
# loading image as 2D array in python
import cv2

# reading an image in python
# the second parameter accepts two values 0 and 1
# 0 means you accept the image in grayscale
# the img_gray would be a 2 dimensional array
img_gray = cv2.imread("smallgray.png", 0)

# 1 means you accept image in RGB mode
# the img_rgb would be a 3 dimensional array
# one dimension for red, one for green and one for blue
img_rgb = cv2.imread("smallgray.png", 1)

# writing or creating an image using an numpy array
cv2.imwrite("new_image.png", img_rgb)

```
