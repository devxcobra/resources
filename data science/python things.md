
```python
# Define a generator function
def countdown():
    print("Starting countdown!")
    yield 3
    yield 2
    yield 1

# Create the generator object
gen = countdown()

# Manually pulling values using next()
print(next(gen))  # Output: Starting countdown! \n 3
print(next(gen))  # Output: 2
print(next(gen))  # Output: 1

# The generator is now empty. Calling next() again causes an error.
print(next(gen))  # Raises StopIteration
```

```python
import os
stats = os.stat("example.txt")

print("Size:", stats.st_size, "bytes")
print("Last modified:", stats.st_mtime)
print("Permissions:", oct(stats.st_mode)[-3:])

Size: 48 bytes  
Last modified: 1720948800.0  
Permissions: 600

os.stat() method is used to retrieve metadata about a file such as its size, permissions and timestamps.
```


To access these system-level variables in Python, you use the `os` module. Specifically, Python exposes the environment variables through a mapping object named **`os.environ`**.'
- **Bracket Notation:** `os.environ['HOME']`
    
- **The `.get()` Method:** `os.environ.get('HOME')`



- [Dictionary](https://www.geeksforgeeks.org/python/python-dictionary/) is a collection of key-value pairs used to store and retrieve data using unique keys. Dictionaries preserve insertion order.
- ****Syntax****: Defined using curly braces {} with key-value pairs.

```python
my_dict = {"a": 1, "b": 2, "c": 3}
```


```python
# ==========================================
# 1. LIST
# ==========================================
# Uses square brackets []
# Can hold anything, and you can change, add, or remove items later.

my_list = [1, "hello", 3.14, True]


# ==========================================
# 2. TUPLE
# ==========================================
# Uses parentheses ()
# Can hold anything, but you CANNOT change, add, or remove items later.

my_tuple = (1, "hello", 3.14, True)

# ==========================================
# 3. DICTIONARY
# ==========================================
# Uses curly braces {} with a colon : 
# Stores data in pairs (like looking up a word to get its meaning).

my_dictionary = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}


# ==========================================
# 4. ARRAY
# ==========================================
# Requires bringing in the 'array' tool first.
# Uses a letter code (like 'i' for whole numbers) and square brackets [].
# Can ONLY hold one specific type of data (e.g., only whole numbers).

import array

my_array = array.array('i', [1, 2, 3, 4])
```

