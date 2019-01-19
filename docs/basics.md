# Basic Concepts

The notes below covers basic concepts of python programming. But it is not aimed at absolute beginners. The notes below would be helpful for someone who has experience in writing code using programming language like javascript, java, c, ruby etc. To understand the notes below one should be familiar with basic programming concepts like variables, operators, loops, conditionals, arrays, lists and functions.

* To run your python code using your mac console, open your terminal, navigate to your folder where you have your python code and then run the following command.

```
$ python3 basics.py
```

* Below is a basic python program that covers important concepts. The program accepts pin from user. If the pin is valid then it asks for fruit. If the fruit input by the user is present in sample.txt file then it prints - That fruit is in the list or it prints - No such fruit found!
* For the code to run below you need to have a sample.txt file with individual names of the fruits listed on a new line.

```python
address = ["Flat Floor Street", "18", "New York"]
pins = {"Mike":1234, "Joe":1111, "Jack":2222}

print(address[0], address[1])

pin = int(input("Enter your pin: "))

def find_in_file(f):    
    myfile = open("sample.txt")
    fruits = myfile.read()
    fruits = fruits.splitlines()
    if f in fruits:
        return "That fruit is in the list."
    else:
        return "No such fruit found!"

if pin in pins.values():
    fruit = input("Enter fruit: ")
    print(find_in_file(fruit))
else:
    print("Incorrect pin!")
    print("This info can be accessed only by: ")
    for key in pins.keys():
        print(key)
```

* The `input()` function in the code above suspends the program and accepts input from the user through your command prompt / mac terminal. The value entered by the user is then stored in the variable `pin` and `fruit` which can then be used in your code for further computation. The input function will return a string output so you need to cast it to int before using it in your code.
* The dictionary object has two methods `keys()` and `values()` that can be used to extract a list of either keys or values from the dictionary. `pins.values()` will return a list of all values for pins dictionary.
* we use the `def` keyword to write custom functions in python. So in the code above `find_in_file` is a custom function which accepts one parameter.
* when a function does not have a return statement then python implicitly applies return None.
* File Object methods
  * `splitlines()` splits the lines by removing the new line characters. The function returns a list of lines in the string.
  * `close()` method of a file object flushes any unwritten information and closes the file object, after which no more writing can be done. It is a good practice to use the close() method to close a file.
  * `seek(0)` method is used to move the cursor back to the beginning after you have used the read() method on your file object.
* use the `dir(__builtins__)` command to list down all the built in functions inside python. Similarly `dir(str)` and `dir(list)` will list all the methods that can be applied on string or list objects.
* There's another datatype in Python called a tuple. It's exactly like a list  except you use round brackets instead of square brackets to define it. A tuple is not mutable which means you can't append or remove items from tuples, unlike lists. Trying to do append to a tuple would throw an error.

```python
mytuple = (1, 2, "Three")
```
* using while loop for checking password
```python
correct_pass = "python123"
name = input("Enter name: ")
surname = input("Enter surname: ")
password = input("Enter password: ")
while correct_pass != password:
  password = input("Oops! Wrong Password. Enter again: ")
print("Hi %s %s, you have logged in successfully" % (name, surname))
```


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
* To return a unique set of items from a list, we can use the `set()` function. The `set()` function returns only the unique elements in a list
* The `__str__()` method tells the python interpreter how to represent your object as a string. Whenever we try to convert the object into a string or when we want to print out that object, we can use `__str__()` method to customize the way it looks when we display the object using the `print()` function.

```python
class Dataset:
    def __init__(self, data):
        self.header = data[0]
        self.data = data[1:]

    def column(self, label):
        if label not in self.header:
            return None

        for idx, value in enumerate(self.header):
            if value == label:
                column_data = []
                for row in self.data:
                    column_data.append(row[idx])
                return column_data

    def count_unique(self, label):
        return len(set(self.column(label)))

   # the method below gets called whenever we print the instance of the object
   def __str__(self):
        data_string = self.data[:10]
        return str(data_string)

nfl_dataset = Dataset(nfl_data)
year_column = nfl_dataset.column('year')
player_column = nfl_dataset.column('player')
total_years = nfl_dataset.count_unique('year')
print(nfl_dataset)
```

* Simplifying lists by extracting the unique elements can help you find unexpected values. The result of this conversion is a set - a data type where each element is unique. A set behaves very similarly to a list. However, if you add an element to a set that it already contains, the set will ignore it. Also, the items in a set are unordered, while each item in a list has an index. Because Sets don't have indexes, the items in a set may display in a different order each time you print it. For example to create a set from a list use the code below.

```python
unique_animals = set(["Dog", "Cat", "Hippo", "Dog", "Cat", "Dog", "Dog", "Cat"])
print(unique_animals)
```
* To add items to a set use `add()` and to remove items use `remove()`. To convert a set to a list use `list()`
* Code for finding missing (empty values) and replacing it with M

```python
gender = []
for item in legislators:
    if item[3] == "":
        item[3] = "M"
    gender.append(item[3])

    birthday = row[2]
    birth_year = birthday.split("-")[0]
    try:
        birth_year = int(birth_year)
    except Exception:
        birth_year = 0
    # extracting the birth year and adding it to an extra column in the legislators dataset
    row.append(birth_year)

gender = set(gender)
print(gender)
```

* replacing 0 as the birth year with the value from the previous row

```python
last_value = 1
for row in legislators:
    if row[7] == 0:
        row[7] = last_value
    last_value = row[7]
```

* use try and exception block to catch exceptions. In the exception clause you can either print the exception or use pass

```python
converted_years = []
for year in birth_years:
    try:
        int_year = int(year)
        converted_years.append(int_year)
    except Exception:
        pass
```

```python
converted_years = []
for year in birth_years:
    try:
        int_year = int(year)
        converted_years.append(int_year)
    except Exception as exc:
        print(type(exc))
        print(str(exc))
```

## Working with multiple Lists

* Python has an `enumerate()` function that allows us to have two variables in the body of a for loop -- an index, and the value.

```python
animals = ["Dog", "Tiger", "SuperLion", "Cow", "Panda"]
viciousness = [1, 5, 10, 10, 1]

for i, animal in enumerate(animals):
    print("Animal")
    print(animal)
    print("Viciousness")
    print(viciousness[i])
```

* A short way to write for loops. Below is the code the calculate the length of each animal in animals and then store that length in animal_lengths.

```python
animals = ["Dog", "Tiger", "SuperLion", "Cow", "Panda"]
animal_lengths = []
for animal in animals:
    animal_lengths.append(len(animal))
```
the above code can also be written as
```python
animal_lengths = [len(animal) for animal in animals]
```
More examples of list comprehension
```python
apple_prices = [100, 101, 102, 105]
apple_prices_doubled = [item*2 for item in apple_prices]
apple_prices_lowered = [item-100 for item in apple_prices]
```

* code for looping through a dataset and creating a dictionary object to list unique first names with counts for dates beyond 1940

```python
name_counts = {}
for row in legislators:
    gender = row[3]
    year = row[7]
    if gender == "F" and year > 1940:
        name = row[1]
        if name in name_counts:
            name_counts[name] = name_counts[name] + 1
        else:
            name_counts[name] = 1

# extracting names with count = 2
top_female_names = []
for name, count in name_counts.items():
    if count == 2:
        top_female_names.append(name)
```
* Working with None - The None object indicates that the variable has no value. Rather than using the normal double equals sign (==) to check whether a value equals `None`, we use the `variable is None` syntax.
* When a value could potentially be `None`, and we want to compare it to another value, we should always include code that checks whether it actually is `None` first.
```python
max_value is None or i > max_value
```
*  We can use the items() method to iterate through keys and values at the same time.
```python
plant_types = {"orchid": "flower", "cedar": "tree", "maple": "tree"}
for kkey, vvalue in plant_types.items():
    print(kkey)
    print(vvalue)
```
* Code for finding the highest male first name after year 1940
```python
top_male_names = []

#finding the count for individual names after year 1940
male_name_counts = {}
for row in legislators:
    if row[3] == 'M' and row[7] >1940:
        name = row[1]
        if name in male_name_counts:
            male_name_counts[name] = male_name_counts[name] + 1
        else:
            male_name_counts[name] = 1

# finding the highest count for the male name
highest_male_count = None
for name in male_name_counts:
    count = male_name_counts[name]
    if (highest_male_count is None or count > highest_male_count):
        highest_male_count = count

# finding male names with the highest count
for name, count in male_name_counts.items():
    if count == highest_male_count:
        top_male_names.append(name)
```
## Variable Scoping
* The global scope is whatever happens outside of a function. Anything that happens inside a function is in a local scope. There's only one global scope, but each function creates its own local scope.
* Our code creates a local scope when it calls a function, and destroys it when the function finishes running. Calling the same function twice will create two separate local scopes.
*  If we use a variable in a local scope that isn't defined there, the Python interpreter will look for it in the global scope. If it doesn't find the variable there, it will check whether the variable is a built-in function name.
* There are limits to how much we can work with global scope variables inside a local scope. While we can access the value of a global scope variable inside a local scope, we can't change the value of that variable. It will throw error.
* Global variables are variables that are available across all scopes. We can access and change the value of a global variable inside any global scope or local scope.

## Regular Expressions
* A regular expression (regex) is a sequence of characters that describes a search pattern. We can use regular expressions to search for and extract data.
* Use the character "." to indicate that any character can be put in its place
* Use the caret symbol ("^") to match the beginning of a string. So accordingly "^a" will match all strings that start with "a".
* Use the dollar sign ("$") to match the end of a string. So accordingly "a$" will match all strings that end with "a".
* In Python, we use the re module to work with regular expressions.
* One useful function the module provides is re.search. With `re.search(regex, string)`, we can check whether string is a match for regex. If it is, the expression will return a match object. If it isn't, it will return None.
* We use square brackets in a regex to indicate that any character within them can fill the space. For example, the regex "[bcr]at" would match the substrings "bat", "cat", and "rat". the regex "[rR]ose" would match the substring "rose" and "Rose"
* In regular expressions, escaping a character means indicating that you don't want the character to do anything special, and that the interpreter should treat it just like any other character. We use the backslash ("\") to escape characters in a regex.
* Suppose we wanted to match all of the strings that end with a period. If we used `".$"`, it would match all strings that contain any character, because "." has that special meaning. Instead, we need to escape the "." with a backslash, so our regex becomes `"\.$"`
```python
serious_count = 0
for row in posts:
    if re.search("\[[Ss]erious\]", row[0]) is not None:
        serious_count += 1
```
* To combine regular expressions, we use the "|" character. For example, "cat|dog" would match "catfish" and "hotdog"
+ The re module provides a sub() function that takes the following parameters (in order):
  * pattern: The regex to match
  * repl: The string that should replace the substring matches
  * string: The string containing the pattern we want to search
* `"[0-9]"` will match any character that falls between 0 and 9
* `"[a-z]"` would match any lowercase letter
* `"[0-9][0-9][0-9][0-9]"` would match four consecutive integers
* Looping through strings to determine whether each string contains a year between 1000 and 2999.
```python
import re
year_strings = []
for string in strings:
    if re.search("[1-2][0-9][0-9][0-9]", string) is not None:
        year_strings.append(string)
```
* We can use curly brackets ("{" and "}") to indicate that a pattern should repeat. To match any four-digit number, for example, we could repeat the pattern "[0-9]" four times by writing `"[0-9]{4}"`.
```python
import re
year_strings = []
for string in strings:
    if re.search("[1-2][0-9]{3}", string) is not None:
        year_strings.append(string)
```
* The re module contains a findall() function that returns a list of substrings matching the regex.
```python
import re
years = re.findall("[1-2][0-9]{3}", years_string)
```

## Working with Dates module
* The time module deals primarily with Unix timestamps. A Unix timestamp is a floating point value with no explicit mention of day, month, or year. This value represents the number of seconds that have passed since the "epoch", or the first second of the year 1970.
* To retrieve the current Unix timestamp, we use the time.time() function
* We can convert a timestamp to a more human-readable form using the time.gmtime() function. This function takes a timestamp as an argument, and returns an instance of the struct_time class. struct_time instances have attributes that represent the current time in other ways
  * tm_year: The year of the timestamp
  * tm_mon: The month of the timestamp (1-12)
  * tm_mday: The day in the month of the timestamp (1-31)
  * tm_hour: The hour of the timestamp (0-23)
  * tm_min: The minute of the timestamp (0-59)
```python
current_time = time.time()
current_struct_time = time.gmtime(current_time)
current_year = current_struct_time.tm_year
current_hour = current_struct_time.tm_hour
```
* The time module always results in a UTC time. UTC stands for Coordinated Universal Time. This is the accepted time standard within the programming community. It corresponds to the mean solar time at 0° longitude, or Greenwich Mean Time, except that it doesn't follow daylight saving time.
* The datetime module offers better support for working extensively with dates.
```python
nye_2018 = datetime.datetime(year=2018, month=12, day=31, hour=12, minute=59, second=59)
```
* We can return the current utc time as a datetime instance using the datetime.utcnow() function.
* We can use the datetime.strftime() method to specify how we'd like the string output to be formatted.
```python
march3 = datetime.datetime(year = 2010, month = 3, day = 3)
pretty_march3 = march3.strftime("%b %d, %Y")
print(pretty_march3) # prints: Mar 03, 2010
mystery_date_formatted_string = mystery_date.strftime("%I:%M%p on %A %B %d, %Y")
print(mystery_date_formatted_string) # prints: 12:00AM on Thursday December 31, 2015
```

## Other userful code bits

```python
# loading files names in python from your current directory
import os
os.listdir()

# working with geocoding
from geopy.geocoders import ArcGIS
nom = ArcGIS()

# forward geocoding
nom.geocode("308 Telok Kurau Road, Singapore 423858)
```
* Geocoding is the process of converting an address to a coordinate. Python has various libraries to support the same.
* The [Geocoder library](https://geocoder.readthedocs.io/api.html) supports the following - forward geocoding, reverse geocoding, house address and IP address

## Jupyter Shortcuts
* For running Jupyter Notebook using command line change to your project directory and run 'jupyter notebook'. It will open a Jupyter session in your browser.
* `Shift + Enter` to insert a new cell
* `Ctrl + Enter' to run your active cell
* Press `M` to enter markdowd mode
* Press 'Y' to code
* Use the up and down arrow keys to navigate between cells
* Press 'D' twice to delete a cell (but not in edit mode)
* Press 'CMD + SHIFT + P' to open the command palate for other commands
* Type your command followed by ? to see help about that command `pd.read_csv?`

## Working with command line

* The __name__ variable in Python scripts is automatically set to the name of the module. If the module is being run from the command line, the __name__ variable will be __main__ by default. Checking the __name__ variable allows us to tell whether a script is running from the command line or not
* The best way to install packages is to use the command line and a program called pip. In order to install a package with pip, we just use pip install. pip install requests will install the requests package, which developers use to interact with websites and APIs
* Using Virtual environments, or virtualenvs, each project we write can have its own version of Python, along with its own packages. This way, we don't need to worry that upgrading the version of a package will affect other projects on the system and cause them to stop working
* We can create a new virtualenv with the virtualenv command. We need to install the virtualenv package first in order to access this command
* Typing `virtualenv main` will create a virtualenv named `main`. It will create a folder in the current directory called main that will hold all of the packages we install into the virtual environment
* By default, virtualenv will use the python executable when it makes a new virtualenv, which means that it has the same version of Python as the system. If you want to use python3 for our virtualenv you can pass the -p flag to the virtualenv command, which will allow us to change the Python interpreter that virtualenv uses
* In this case, we can type `virtualenv -p /usr/bin/python3 python3` to use Python 3 instead of Python 2
* Once we've created a virtualenv, we can activate it using source python3/bin/activate (this assumes that the virtualenv is called python3, and that the folder for the virtualenv is in our current directory)
```python
# creating a virtual environment called script and then activating it
virtualenv -p /usr/bin/python3 script
source script/bin/activate
```
* Use `python -V` to verify that Python 3 is the current Python version after activating the virtualenv.
* Use `pip freeze` to check which packages are installed, and their versions.
* We can also pass command line options into Python scripts. We can retrieve them from inside the script through the sys package. Once we import the sys package, the argv list will allow us to retrieve the positional arguments passed into the script. 
* To switch off the virtualenv, we deactivate it with the `deactivate` command. This command will automatically shut down the current virtualenv

## Git Basics

* There are a few distributed version control systems, including Git, Mercurial and Bazzar.
* The first step in using Git is to initialize a folder as a repository. 
* A repository (or "repo") tracks multiple versions of the files in the folder, enabling collaboration.
* We can initialize a repository by typing `git init` inside the folder we want to use for our project.
* Initializing a Git repository will create a folder called .git inside the repository folder.
* The typical Git workflow involves adding files, making changes, and then storing a checkpoint (or "snapshot") of those changes. These checkpoints are called commits.
* To check the status of the repo use `git status`
* Files that are **staged** are marked for inclusion in the next commit, but hasn't been committed yet
* Use `git add .` to add the modified files to the staging area
* A commit stores a snapshot of the files in the repository at a certain point in time. By building a history of these snapshots, we can rewind to an earlier point in time, or merge someone else's changes with our own.
* Before we place a file in the staging area, we can use `git diff` to see all of the line differences between the current and previous version.
* We can pull up a repository's commit history using the `git log` command. This command will show us a list of all of the commits to the repository, in descending order by creation date.
