# Python Cheatsheet

## Variables & Types
```python
name    = "Ahmad"      # str
age     = 20           # int
price   = 99.9         # float
active  = True         # bool
nothing = None         # NoneType
big_num = 1_000_000   # underscores for readability

# Type conversion
int("42"), float("3.14"), str(42), bool(0)

# Type check
type(age)           # <class 'int'>
isinstance(age, int)  # True
```

## Strings
```python
s = "Hello, World!"

len(s)              # 13
s.upper()           # "HELLO, WORLD!"
s.lower()
s.strip()           # remove whitespace
s.lstrip() / s.rstrip()
s.replace("World", "Python")
s.split(", ")       # ["Hello", "World!"]
s.splitlines()      # split on newlines
",".join(["a","b"]) # "a,b"
s.startswith("Hello"), s.endswith("!")
s.find("World")     # 7 (-1 if not found)
s.count("l")        # 3
s[0:5]              # "Hello" (slicing)
s[::-1]             # reverse
s.center(20, "-")   # "---Hello, World!---"
s.zfill(10)         # pad with zeros
s.isdigit(), s.isalpha(), s.isalnum()

# f-strings (preferred)
name = "Rana"
print(f"Hello, {name}!")
print(f"{price:.2f}")          # 99.90
print(f"{1_000_000:,}")        # 1,000,000
print(f"{0.75:.0%}")           # 75%
print(f"{name!r}")             # 'Rana' (repr)

# Multiline
text = """Line 1
Line 2
Line 3"""
```

## Lists
```python
fruits = ["apple", "banana", "mango"]

# Access
fruits[0]           # "apple"
fruits[-1]          # "mango"
fruits[1:3]         # ["banana", "mango"]
fruits[::2]         # every 2nd

# Mutate
fruits.append("grape")
fruits.extend(["kiwi", "pear"])   # add multiple
fruits.insert(1, "cherry")
fruits.remove("banana")           # by value
popped = fruits.pop()             # remove last
popped = fruits.pop(0)            # remove by index
fruits.sort(key=str.lower)
fruits.sort(reverse=True)
fruits.reverse()
fruits.clear()

# Info
len(fruits)
fruits.index("mango")     # raises ValueError if not found
fruits.count("apple")
"apple" in fruits         # True

# List comprehension
squares  = [x**2 for x in range(10)]
evens    = [x for x in range(20) if x % 2 == 0]
matrix   = [[i*j for j in range(3)] for i in range(3)]
filtered = [x.strip() for x in raw if x.strip()]

# Functional
list(map(str, [1,2,3]))            # ["1","2","3"]
list(filter(None, [0,1,"",2]))     # [1, 2]
from functools import reduce
reduce(lambda a,b: a+b, [1,2,3])   # 6

# Zip & Enumerate
for i, fruit in enumerate(fruits, start=1):
    print(i, fruit)

for name, score in zip(names, scores):
    print(name, score)

# Unpack
first, *rest = fruits
a, b, c = [1, 2, 3]
```

## Dictionaries
```python
person = {
    "name": "Ahmad",
    "age":  20,
    "city": "Faisalabad"
}

# Access
person["name"]                # KeyError if missing
person.get("email", "N/A")    # safe

# Modify
person["email"] = "a@b.com"   # add / update
del person["city"]
popped = person.pop("age", None)
person.update({"role": "admin", "age": 21})

# Info
"name" in person               # True
person.keys()                  # dict_keys
person.values()
person.items()                 # key-value pairs
len(person)

# Iterate
for key, val in person.items():
    print(f"{key}: {val}")

# Dict comprehension
squares = {x: x**2 for x in range(5)}
filtered = {k: v for k, v in person.items() if v}

# Merge (Python 3.9+)
merged = dict1 | dict2
dict1 |= dict2  # in-place

# defaultdict
from collections import defaultdict
dd = defaultdict(list)
dd["fruits"].append("apple")   # no KeyError

# Counter
from collections import Counter
words = ["a","b","a","c","b","a"]
Counter(words)   # Counter({'a':3, 'b':2, 'c':1})
Counter(words).most_common(2)
```

## Tuples & Sets
```python
# Tuple — ordered, immutable
coords = (10.5, 24.8)
x, y = coords
point = 1, 2, 3   # parens optional

# Named tuple
from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)
print(p.x, p.y)

# Set — unordered, unique
tags = {"python", "web", "python"}   # {"python", "web"}
tags.add("django")
tags.discard("web")    # no error if missing
tags.remove("js")      # KeyError if missing

set1 = {1, 2, 3}
set2 = {2, 3, 4}
set1 & set2            # {2, 3} intersection
set1 | set2            # {1,2,3,4} union
set1 - set2            # {1} difference
set1 ^ set2            # {1,4} symmetric difference
set1.issubset(set2)
set1.issuperset(set2)
```

## Control Flow
```python
# if / elif / else
if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teen")
else:
    print("Child")

# Ternary
status = "Active" if is_active else "Inactive"

# match (Python 3.10+)
match role:
    case "admin":  print("Full access")
    case "user":   print("Limited access")
    case _:        print("No access")

# for
for i in range(5):                  # 0-4
for i in range(1, 10, 2):           # 1,3,5,7,9
for item in iterable:
for i, item in enumerate(items):
for k, v in dict.items():

# while
while count < 5:
    count += 1
else:                  # runs if loop finished normally (no break)
    print("Done")

# Control
break       # exit loop
continue    # skip to next iteration
pass        # no-op placeholder
```

## Functions
```python
def greet(name: str, greeting: str = "Hello") -> str:
    """Greet someone. Docstring here."""
    return f"{greeting}, {name}!"

# *args and **kwargs
def func(*args, **kwargs):
    print(args)    # tuple
    print(kwargs)  # dict

func(1, 2, 3, name="Ahmad", role="admin")

# Keyword-only args
def connect(*, host, port=3306):
    ...
connect(host="localhost")  # must use keyword

# Lambda
square = lambda x: x ** 2
sorted_items = sorted(items, key=lambda x: x["name"])

# Decorators
def timer(fn):
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = fn(*args, **kwargs)
        print(f"{fn.__name__}: {time.time()-start:.3f}s")
        return result
    return wrapper

@timer
def slow_fn():
    time.sleep(1)
```

## OOP
```python
class Animal:
    category = "Living Being"  # class variable

    def __init__(self, name: str, sound: str):
        self.name  = name    # instance variable
        self.sound = sound
        self._age  = 0       # protected convention
        self.__secret = ""   # name-mangled (private)

    def speak(self) -> str:
        return f"{self.name} says {self.sound}!"

    @classmethod
    def from_dict(cls, data: dict):
        return cls(data["name"], data["sound"])

    @staticmethod
    def kingdom():
        return "Animalia"

    def __repr__(self): return f"Animal({self.name!r})"
    def __str__(self):  return self.name
    def __len__(self):  return len(self.name)
    def __eq__(self, other): return self.name == other.name

# Inheritance
class Dog(Animal):
    def __init__(self, name):
        super().__init__(name, "Woof")
        self.tricks = []

    def speak(self):          # override
        return f"{super().speak()} *wags tail*"

    def learn(self, trick):
        self.tricks.append(trick)

# Abstract base class
from abc import ABC, abstractmethod
class Shape(ABC):
    @abstractmethod
    def area(self) -> float: ...

    @abstractmethod
    def perimeter(self) -> float: ...

class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self):      return 3.14159 * self.r ** 2
    def perimeter(self): return 2 * 3.14159 * self.r
```

## Error Handling
```python
try:
    result = 10 / int(input("Divisor: "))
except ZeroDivisionError:
    print("Cannot divide by zero")
except ValueError as e:
    print(f"Invalid input: {e}")
except (TypeError, OverflowError) as e:
    print(f"Math error: {e}")
else:
    print(f"Result: {result}")   # runs only if no exception
finally:
    print("Always runs")          # cleanup

# Custom exception
class ValidationError(ValueError):
    def __init__(self, field, message):
        super().__init__(f"{field}: {message}")
        self.field = field

raise ValidationError("email", "Invalid format")

# Context managers
with open("file.txt", "r", encoding="utf-8") as f:
    content = f.read()
# file auto-closed even on error
```

## File & JSON
```python
import json, pathlib

# Pathlib (modern)
path = pathlib.Path("data/file.txt")
path.parent.mkdir(parents=True, exist_ok=True)
path.write_text("Hello", encoding="utf-8")
content = path.read_text(encoding="utf-8")
path.exists(), path.is_file(), path.is_dir()
list(path.parent.glob("*.txt"))

# JSON
data = {"name": "Ahmad", "scores": [95, 87, 92]}

# Write
with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2, ensure_ascii=False)

# Read
with open("data.json", "r", encoding="utf-8") as f:
    loaded = json.load(f)

# String conversion
json_str = json.dumps(data, indent=2)
parsed   = json.loads(json_str)
```

## Useful Standard Library
```python
import os, sys, re, random, math, datetime
from pathlib import Path
from collections import Counter, defaultdict, deque
from itertools import chain, product, combinations, permutations
from functools import lru_cache, partial
from typing import Optional, Union, List, Dict, Tuple, Any

# Random
random.random()              # 0.0 – 1.0
random.randint(1, 100)       # 1 – 100 inclusive
random.choice(seq)
random.choices(seq, k=5)     # with replacement
random.sample(seq, k=5)      # without replacement
random.shuffle(lst)          # in-place

# Math
math.sqrt(16)   # 4.0
math.floor(3.7), math.ceil(3.2)
math.log(100, 10)            # 2.0
math.pi, math.e, math.inf

# Datetime
now = datetime.datetime.now()
now.strftime("%Y-%m-%d %H:%M:%S")
datetime.datetime.strptime("2024-09-04", "%Y-%m-%d")
datetime.timedelta(days=7)

# Re (regex)
re.search(r'\d+', 'abc123')          # match object or None
re.findall(r'\d+', 'a1b22c333')      # ['1','22','333']
re.sub(r'\s+', '-', 'a b  c')        # 'a-b-c'
re.split(r'[,;]\s*', 'a, b; c')      # ['a','b','c']

# lru_cache — memoize
@lru_cache(maxsize=None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)
```
