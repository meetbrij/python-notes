# Data Analysis and Visualization

## Introduction to Numpy

* NumPy and pandas provide a powerful toolset for working with data in Python. They allow us to write code quickly without sacrificing performance. 
* NumPy uses vectorization to make your performance faster.
* When we use a for-loop to iterate through a list python interpretor will go through each and every item in the list. So if our list has 1000 rows then it would take 1000 processor cycles to process 1000 rows of data.
* Vectorization takes advantage of a processor feature called Single Instruction Multiple Data (SIMD) to process data faster. SIMD allows a processor to perform the same operation, on multiple data points, in a single processor cycle.
* Vectorized operations might process as little as two and as many as as hundreds of operations per processor cycle, depending on the capabilities of the processor and the size of each data point. So instead of processing individual rows it can process multiple (say 100) rows in one go thus reducing the number of processing cycles. 
* NumPy and pandas takes care of SIMD and processor cycles for you.
* 
