# Adelin Database

In many cases, we don't need a complex database infrastructure for the applications we want to build. In such situations, Adelin is very suitable for creating a simple and manageable database.


## Features

- Written pure python
- Save and load from file
- You can automatically add a date and an ID number for each entered data.
- The saved information, along with the Base64-encoded version in JSON format, is stored in .adl format.This way, it prevents ordinary users from tampering with and manipulating the JSON file.
- Each saved piece of information is stored in .adl format under a folder with a name determined by you.


## Install

```bash
pip install adelin
```

## Usage


### Init Data

```python
from adelin import Makedata

# As a preliminary setup, we enter the row keys.
fruits = MakeData("Name","Price_USD","Units_KG","Color")

# Later on, we specify the name of the section under which we want to collect our data in our inherited object, and then we enter the values corresponding to the row keys.
# "Fruit" is our main heading.
fruits("Fruit", "Apple", 5, 200, "Red")
# Let's enter a new piece of data.
fruits("Fruit", "Apple", 4, 150, "Green")

# Now, the prepared data is ready to be saved.

```

### Save and Read Data

```python
# We complete the saving process by using the `save_db` method, where we first specify the folder name we want to save and then the file name.
# This way, we intend to establish a more organized recording system to prevent future complexity.
# The saved file is located under the "FRUITS" folder and is recorded in the "product.adl" file.
fruits.save_db("FRUITS","product")

# Reading operations can also be easily performed by using the `read_db` method, where you specify the folder and file you want to read from, following the same approach.
print(fruits.read_db("FRUITS","product"))
# Here is the result
```
```json
{'Fruit': [{'Name': 'Apple', 'Price_USD': 5, 'Units_KG': 200, 'Color': 'Red'}, {'Name': 'Apple', 'Price_USD': 4, 'Units_KG': 150, 'Color': 'Green'}]}
```

### Extra Features      
Sometimes, we may want to add an ID number and a date to the data we want to enter.
Let's now create the same example by adding an ID number and a Date.

```python

fruits = MakeData("Name","Price_USD","Units_KG","Color", id=True, date=True)
fruits("Fruit", "Apple", 5, 200, "Red")
fruits("Fruit", "Apple", 4, 150, "Green")
fruits.save_db("FRUITS","product")    
print(fruits.read_db("FRUITS","product"))



```
Here is the result:
```json
{'Fruit': [{'Name': 'Apple', 'Price_USD': 5, 'Units_KG': 200, 'Color': 'Red', 'Id': 'da8bcabb', 'Date': '26/09/2023'}, {'Name': 'Apple', 'Price_USD': 4, 'Units_KG': 150, 'Color': 'Green', 'Id': 'da8cc555', 'Date': '26/09/2023'}]}
```

Sometimes, we may prefer all entered headings to be in uppercase.

```python
fruits = MakeData("Name","Price_USD","Units_KG","Color", column_up=True)
fruits("Fruit", "Apple", 5, 200, "Red")
fruits("Fruit", "Apple", 4, 150, "Green")
fruits.save_db("FRUITS","product")    
print(fruits.read_db("FRUITS","product"))
```
```json
{'FRUIT': [{'Name': 'Apple', 'Price_USD': 5, 'Units_KG': 200, 'Color': 'Red'}, {'Name': 'Apple', 'Price_USD': 4, 'Units_KG': 150, 'Color': 'Green'}]}
```

### Delete Data
In this current version, only data with entered ID numbers can be deleted, but this functionality can be further enhanced.

```python
# The `del_with_id` method will delete the information of the data with the specified "xxx" ID number when provided with the folder name and the file name where the data with the "xxx" ID number is located.
fruits.del_with_id("FRUITS","product","c43c6881")
```
### Fetch Data from File
You can obtain the data collected under each heading in a list format by entering the desired row values.

```python
foods = MakeData("Name","Price_USD","Units_KG","Color", id=True, date=True, column_up=True)

foods("Fruit", "Apple", 5, 200, "Red")
foods("Fruit", "Apple", 4, 150, "Green")
foods.save_db("Fruits","product")

foods("xxVegetable", "cucumber", 2, 300, "Green")
foods("xxVegetable", "tomato", 1, 350," Red")
foods.save_db("Vegetables","salad")

foods("Eggs", "quail egg", 0.5, 750, "patchy brown")
foods("Eggs", "chicken egg", 0.1, 1800, "White")
foods("Eggs", "chicken egg", 0.1, 3200, "Brown")
foods.save_db("Eggs","eggs")

print(foods.read_db("Eggs","eggs"))

```
Result:
```json
{'EGGS': [{'Name': 'quail egg', 'Price_USD': 0.5, 'Units_KG': 750, 'Color': 'patchy brown', 'Id': '60799066', 'Date': '26/09/2023'}, {'Name': 'chicken egg', 'Price_USD': 0.1, 'Units_KG': 1800, 'Color': 'White', 'Id': '60799067', 'Date': '26/09/2023'}, {'Name': 'chicken egg', 'Price_USD': 0.1, 'Units_KG': 3200, 'Color': 'Brown', 'Id': '6079b783', 'Date': '26/09/2023'}]}
```

```python
print(foods.fetchdata("Vegetables","salad","xxVegetable","Name","Id"))
# result ['cucumber', 'd48fa160', 'tomato', 'd48fc737']
```
