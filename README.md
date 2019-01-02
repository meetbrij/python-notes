# python-notes
Notes on my python course on dataquest

## Modules and Classes

* When we import a module this way, we add all of the objects and functions within that module to the global namespace. A __namespace__ is a dictionary that contains all of the names we can refer to in our code. Before running our code, the Python interpreter adds all of the objects and functions that are available by default (print(), list(), etc.) to the global namespace.
* When we create variables or define our own functions, these are also added to the same namespace.
* If we import `function1()` from my_module and then define our own `function1()`, then the reference to the original `function1()` we imported will get overrwriten in the namespace.
* We can work with CSV files more easily through the csv module. This module has a `reader()` function that takes a file object as its argument, and returns an object that represents our data.
* code for reading a csv file

```python
import csv
f = open("nfl.csv", 'r')
csvreader = csv.reader(f)
nfl = list(csvreader)
```
* Counting number of wins for a team

```python
import csv

f = open("nfl.csv", 'r')
nfl = list(csv.reader(f))

# Define your function here.
def nfl_wins(team):
    total_wins = 0
    for row in nfl:
        if row[2] == team:
            total_wins = total_wins + 1
    return total_wins

cowboys_wins = nfl_wins("Dallas Cowboys")
falcons_wins = nfl_wins("Atlanta Falcons")
```

* A class bundles up logically grouped functions and variables (called attributes) that we can use anywhere in our code. The reason to use classes is similar to modules but instead of requiring multiple files for different groupings, we can add multiple classes to a single file. This promotes code abstraction which helps us by not having to repeatedly write the same code over and over again.
* Python uses this `self` variable to refer to the created object so you can interact with the instance data. If you didn't have `self`, then the class wouldn't know where to store the internal data you wanted to keep. By convention, `self` is used to define the instance even though it's possible to name it whatever you want. It is highly recommended to use `self` because any project that is built using Python will also give it that name.
* Sample code for creating a class and instantiating it

```python
class Dataset:
    def __init__(self):
        self.type = "csv"
        
dataset = Dataset()
print(dataset.type)
```

```python
class Dataset:
    def __init__(self, data):
        self.type = "csv"
        self.data = data
    
    def print_data(self, num_rows):
        print(self.data[:num_rows])
        
f = open("nfl.csv", 'r')
nfl_data = list(csv.reader(f))

nfl_dataset = Dataset(nfl_data)
nfl_dataset.print_data(25)
dataset_data = nfl_dataset.data
```
* Code for grabbing column data for a given header using `enumerate()`. To grab the column data, all we need to do is search through the headers, find the index of the given label, and then loop through the rest of the data returning the value of the index every iteration.

```python
class Dataset:
    def __init__(self, data):
        self.header = data[0]
        self.data = data[1:]
        
    # Add your method here.
    def column(self, label):
        for idx, value in enumerate(self.header):
            if value == label:
                column_data = []
                for row in self.data:
                    column_data.append(row[idx])
                return column_data

nfl_dataset = Dataset(nfl_data)
year_column = nfl_dataset.column('year')
player_column = nfl_dataset.column('player')
```
