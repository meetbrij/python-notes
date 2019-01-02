# python-notes
Notes on my python course on dataquest

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

* 
