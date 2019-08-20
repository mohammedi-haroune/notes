# Forward
The book is really effective, that is you read a small chunk of the book to get a really interesting ideas to write a better python code (Example: item 9 Consider Generator Expressions for Large Comprehensions), Pythonic ways to code most common patterns (Example: item 10 Prefer `enumerate` Over `range`and item 11 Use `zip` to Process Iterators in Parallel) and also advice for good practices (Example: item 1: Follow the PEP 8 Style Guide).

What I really liked in the book is: the author putting a lot attention on writing readable, reusable and maintainable code, thinking a lot about newbies who will read your code and also about adding functionalities to to your code while preserving backward compatibility.

# Content
## Chapter 1: Pythonic Thinking
### Item 1: Know Which Version of Python You’re Using
There are two ways to inspect python interpreter version:
1. type `python -v`
2. use `os.sys.version`

### Item 2: Follow the PEP 8 Style Guide
The PEP 8 contains guidelines about code formatting

### Item 3: Know the Differences Between bytes, str, and unicode
In python 3: str is a sequence of unicode characters and bytes is a sequence of bytes

In python 2: unicode is a sequence of unicode characters and str is a sequence of bytes

Use helper functions (`isinstance`) to ensure that the inputs you operate on are the type of character sequence you expect (8-bit values, UTF-8 encoded characters, Unicode characters, etc.).

If you want to read or write binary data to/from a file, always open the file using a binary mode (like 'rb' or 'wb').

### Item 4: Write Helper Functions Instead of Complex Expressions
Python expressiveness let you define complex expressions, thought for the sake of code readability and maintanbility prefer defining helper functions instead.

### Item 5: Know How to Slice Sequences
Avoid being verbose: Don’t supply 0 for the start index or the length of the sequence for the end index.

Slicing is forgiving of start or end indexes that are out of bounds, making it easy to express slices on the front or back boundaries of a sequence (like a[:20] or a[-20:]).

Assigning to a list slice will replace that range in the original sequence with what’s referenced even if their lengths are different.

### Item 6: Avoid Using start, end, and stride in a Single Slice
If it's the case use separate instruction for slicing from start to end and for stride
Example
```python
l = l[::2]
l = l[1:9]
```

### Item 7: Use List Comprehensions Instead of map and filter
For the sake of readability
```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# Prefer this
squares = [x ** 2 for x in a]
# Than this
squares = map(lambda x: x ** 2, a)
```

### Item 8: Avoid More Than Two Expressions in List Comprehensions
For the sake of readability
```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
# Prefer this 
filtered = []
for row in matrix:
    if sum(row) >= 10
        for x in row:
            if x % 3 == 0:
                filtered.append(x)
# Than this
filtered = [x for row in matrix if sum(row) >= 10 
              for x in row if x % 3 == 0]
```

### Item 9: Consider Generator Expressions for Large Comprehensions
Generator expressions avoid memory issues by producing outputs one at a time as an iterator.

Generator expressions can be composed by passing the iterator from one generator expression into the for subexpression of another.

Replacing the brackets `[]` with parenthesis `()` if for comprehensions will result in a generator

### Item 10: Prefer enumerate Over range
Sometimes you want to loop over a sequence by index, python provide a utility method for that called `enumerate`
```
for idx, item in enumerate(list):
    print(‘%d: %s’ % (i, item))
```

### Item 11: Use zip to Process Iterators in Parallel
Python provides a utility method called `zip` to process iterators in parallel
```python
for name, count in zip(names, letters):
    print('%s: %s' % (name, count))
```

### Item 12: Avoid else Blocks After for and while Loops
Python supports `else` block in `for` and `while` but it's counterintuitive and can be confusing, you should avoid it as much as you can.
### Item 13: Take Advantage of Each Block in try/except/else/finally
```python
handle = open(path, ‘r+’)
try:
    data = handle.read()
    op = json.loads(data)
    value = (
    op[‘numerator’] /
    op[‘denominator’])
except ZeroDivisionError as e
    return
else:
    op[‘result’] = value
    result = json.dumps(op)
    handle.seek(0)
    handle.write(result)
    return value
finally:
    handle.close()
```
The `else` block helps you minimize the amount of code in try blocks and visually distinguish the success case from the try/except blocks.

## Chapter 2: Functions
### Item 14: Prefer Exceptions to Returning None
### Item 15: Know How Closures Interact with Variable Scope
### Item 16: Consider Generators Instead of Returning Lists
### Item 17: Be Defensive When Iterating Over Arguments
### Item 18: Reduce Visual Noise with Variable Positional Arguments
### Item 19: Provide Optional Behavior with Keyword Arguments
### Item 20: Use None and Docstrings to Specify Dynamic Default Arguments
### Item 21: Enforce Clarity with Keyword-Only Arguments
## Chapter 3: Classes and Inheritance
### Item 22: Prefer Helper Classes Over Bookkeeping with Dictionaries and Tuples
### Item 23: Accept Functions for Simple Interfaces Instead of Classes
### Item 24: Use @classmethod Polymorphism to Construct Objects Generically### Item 25: Initialize Parent Classes with super
### Item 26: Use Multiple Inheritance Only for Mix-in Utility Classes
### Item 27: Prefer Public Attributes Over Private Ones
### Item 28: Inherit from collections.abc for Custom Container Types
## Chapter 4: Metaclasses and Attributes
### Item 29: Use Plain Attributes Instead of Get and Set Methods
### Item 30: Consider @property Instead of Refactoring Attributes
### Item 31: Use Descriptors for Reusable @property Methods
### Item 32: Use __getattr__, __getattribute__, and __setattr__ for Lazy
Attributes
### Item 33: Validate Subclasses with Metaclasses
### Item 34: Register Class Existence with Metaclasses
### Item 35: Annotate Class Attributes with Metaclasses
## Chapter 5: Concurrency and Parallelism
### Item 36: Use subprocess to Manage Child Processes
### Item 37: Use Threads for Blocking I/O, Avoid for Parallelism
### Item 38: Use Lock to Prevent Data Races in Threads
### Item 39: Use Queue to Coordinate Work Between Threads
### Item 40: Consider Coroutines to Run Many Functions Concurrently
### Item 41: Consider concurrent.futures for True Parallelism
## Chapter 6: Built-in Modules
### Item 42: Define Function Decorators with functools.wraps
### Item 43: Consider contextlib and with Statements for Reusable try/finally Behavior
### Item 44: Make pickle Reliable with copyreg
### Item 45: Use datetime Instead of time for Local Clocks
### Item 46: Use Built-in Algorithms and Data Structures
### Item 47: Use decimal When Precision Is Paramount
### Item 48: Know Where to Find Community-Built Modules
## Chapter 7: Collaboration
### Item 49: Write Docstrings for Every Function, Class, and Module
### Item 50: Use Packages to Organize Modules and Provide Stable APIs### Item 51: Define a Root Exception to Insulate Callers from APIs
### Item 52: Know How to Break Circular Dependencies
### Item 53: Use Virtual Environments for Isolated and Reproducible Dependencies
## Chapter 8: Production
### Item 54: Consider Module-Scoped Code to Configure Deployment Environments
### Item 55: Use repr Strings for Debugging Output
### Item 56: Test Everything with unittest
### Item 57: Consider Interactive Debugging with pdb
### Item 58: Profile Before Optimizing
### Item 59: Use tracemalloc to Understand Memory Usage and Leaks
