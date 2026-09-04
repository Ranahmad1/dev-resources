# Python Cheatsheet

## Variables & Data Types
```python
# No need to declare type
name = "Ahmad"          # str
age = 20                # int
price = 99.9            # float
is_active = True        # bool
nothing = None          # None

# Type checking & conversion
type(age)               # <class 'int'>
str(42)                 # "42"
int("42")               # 42
float("3.14")           # 3.14
bool(0)                 # False
```

## Strings
```python
s = "Hello, World!"

len(s)                  # 13
s.upper()               # "HELLO, WORLD!"
s.lower()               # "hello, world!"
s.strip()               # Remove whitespace
s.replace("World", "Python")  # Replace
s.split(", ")           # ["Hello", "World!"]
s.startswith("Hello")  # True
s.endswith("!")         # True
s[0:5]                  # "Hello" (slicing)
s[::-1]                 # Reverse string

# f-strings (best way)
name = "Rana"
print(f"Hello, {name}!")       # Hello, Rana!
print(f"2 + 2 = {2 + 2}")      # 2 + 2 = 4
print(f"{price:.2f}")           # 2 decimal places

# Multiline string
text = """Line 1
Line 2
Line 3"""
```

## Lists
```python
fruits = ["apple", "banana", "mango"]

fruits[0]              # "apple"
fruits[-1]             # "mango" (last)
fruits[1:]             # ["banana", "mango"]
len(fruits)            # 3

fruits.append("grape")         # Add to end
fruits.insert(1, "kiwi")       # Insert at index
fruits.remove("banana")        # Remove by value
fruits.pop()                   # Remove last
fruits.pop(0)                  # Remove by index
fruits.sort()                  # Sort ascending
fruits.reverse()               # Reverse
fruits.index("mango")          # Find index
"apple" in fruits              # True

# List comprehension
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
```

## Dictionaries
```python
person = {
    "name": "Ahmad",
    "age": 20,
    "city": "Faisalabad"
}

person["name"]                 # "Ahmad"
person.get("email", "N/A")     # Safe access with default
person["email"] = "a@b.com"   # Add/update
del person["city"]             # Delete key

person.keys()                  # dict_keys(['name', 'age'])
person.values()                # dict_values([...])
person.items()                 # key-value pairs

# Check key
"name" in person               # True

# Loop
for key, value in person.items():
    print(f"{key}: {value}")

# Dict comprehension
squares = {x: x**2 for x in range(5)}
```

## Tuples & Sets
```python
# Tuple (immutable)
coords = (10, 20)
x, y = coords              # Unpack

# Set (unique values, no order)
tags = {"python", "web", "python"}  # {"python", "web"}
tags.add("django")
tags.remove("web")
set1 & set2                # Intersection
set1 | set2                # Union
set1 - set2                # Difference
```

## Conditions & Loops
```python
# If-elif-else
if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teen")
else:
    print("Child")

# Ternary
status = "Active" if is_active else "Inactive"

# For loop
for i in range(5):         # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 10, 2):  # 1, 3, 5, 7, 9
    print(i)

for fruit in fruits:
    print(fruit)

for i, fruit in enumerate(fruits):  # with index
    print(i, fruit)

# While
count = 0
while count < 5:
    print(count)
    count += 1

# Loop control
break      # Exit loop
continue   # Skip to next iteration
```

## Functions
```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Ahmad")             # Hello, Ahmad!
greet("Ali", "Hi")         # Hi, Ali!

# *args and **kwargs
def add(*nums):
    return sum(nums)

def show_info(**kwargs):
    for k, v in kwargs.items():
        print(f"{k}: {v}")

show_info(name="Ahmad", age=20)

# Lambda
square = lambda x: x ** 2
print(square(5))           # 25

# Map, Filter, Reduce
nums = [1, 2, 3, 4, 5]
list(map(lambda x: x*2, nums))          # [2,4,6,8,10]
list(filter(lambda x: x > 2, nums))     # [3,4,5]
```

## OOP (Classes)
```python
class Animal:
    # Class variable
    category = "Living Being"

    def __init__(self, name, sound):
        self.name = name       # Instance variable
        self.sound = sound

    def speak(self):
        return f"{self.name} says {self.sound}"

    def __str__(self):
        return f"Animal: {self.name}"

# Inheritance
class Dog(Animal):
    def __init__(self, name):
        super().__init__(name, "Woof")

    def fetch(self):
        return f"{self.name} fetches the ball!"

dog = Dog("Bruno")
print(dog.speak())     # Bruno says Woof
print(dog.fetch())     # Bruno fetches the ball!
print(isinstance(dog, Animal))  # True
```

## Error Handling
```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except (TypeError, ValueError) as e:
    print(f"Type error: {e}")
else:
    print("No error!")     # Runs if no exception
finally:
    print("Always runs")   # Always executes

# Raise custom error
if age < 0:
    raise ValueError("Age cannot be negative")
```

## File Handling
```python
# Write
with open("file.txt", "w") as f:
    f.write("Hello, World!")

# Read
with open("file.txt", "r") as f:
    content = f.read()
    # OR
    lines = f.readlines()   # list of lines

# Append
with open("file.txt", "a") as f:
    f.write("\nNew line")

# JSON
import json
data = {"name": "Ahmad", "age": 20}
json.dumps(data)           # dict to JSON string
json.loads('{"name":"Ahmad"}')  # JSON string to dict

with open("data.json", "w") as f:
    json.dump(data, f, indent=2)
```

## Useful Standard Library
```python
import os
os.getcwd()                # Current directory
os.listdir(".")            # List directory
os.path.exists("file.txt") # Check if exists
os.makedirs("folder", exist_ok=True)

import datetime
now = datetime.datetime.now()
print(now.strftime("%Y-%m-%d %H:%M"))

import random
random.random()            # 0.0 to 1.0
random.randint(1, 100)    # 1 to 100
random.choice(["a","b"])  # Random from list
random.shuffle(my_list)   # Shuffle in place

import math
math.sqrt(16)              # 4.0
math.floor(3.7)            # 3
math.ceil(3.2)             # 4
math.pi                    # 3.14159...

import re   # Regex
re.search(r'\d+', 'abc123')   # Match object
re.findall(r'\d+', 'a1b2c3')  # ['1','2','3']
re.sub(r'\s+', '-', 'a b c')  # 'a-b-c'
```
