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

