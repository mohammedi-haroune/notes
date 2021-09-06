- pep8
- metaprogramming
- variable unpacking
- try, execpt, else
- context manager
- extend import functionality
- decorator
- Saving Memory with `__slots__`
- Controlling What Can Be Imported and What Not with '__all__'
- Comparison Operators the Easy Way with `functools.total_oredering`
- Sliceable
- namedtuple
- logging
- requests
- click or argparser
- defaultdict
- comprehensions
- sum, min, max, any, all
- descriptors

## Context manager
```python
class Connection:
	def __init__(self):
		...

	def __enter__(self):
		# Initialize connection...

	def __exit__(self, type, value, traceback):
		# Close connection...

with Connection() as c:
	# __enter__() executes
	...
	# conn.__exit__() executes
```


```python
from contextlib import contextmanager

@contextmanager
def tag(name):
	print(f"<{name}>")
	yield
	print(f"</{name}>")

with tag("h1"):
	print("This is Title.")
```
