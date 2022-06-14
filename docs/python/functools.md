# Functools 
Source: https://docs.python.org/3/library/functools.html

### `@functools.cached_property(func)`

> Transform a method of a class into a property whose value is computed once and then cached as a normal attribute for the life of the instance. Similar to property(), with the addition of caching. Useful for expensive computed properties of instances that are otherwise effectively immutable.

### `functools.cmp_to_key(func)`
> Transform an old-style comparison function to a key function. Used with tools that accept key functions (such as sorted(), min(), max(), heapq.nlargest(), heapq.nsmallest(), itertools.groupby()). This function is primarily used as a transition tool for programs being converted from Python 2 which supported the use of comparison functions.

> A comparison function is any callable that accept two arguments, compares them, and returns a negative number for less-than, zero for equality, or a positive number for greater-than. A key function is a callable that accepts one argument and returns another value to be used as the sort key.

Example:
```python
sorted(iterable, key=cmp_to_key(locale.strcoll))  # locale-aware sort order
```

### `@functools.total_ordering`
> Given a class defining one or more rich comparison ordering methods, this class decorator supplies the rest. This simplifies the effort involved in specifying all of the possible rich comparison operations:

> The class must define one of `__lt__()`, `__le__()`, `__gt__()`, or `__ge__()`. In addition, the class should supply an `__eq__()` method.

### `class functools.partialmethod(func, /, *args, **keywords)`
Needs more investigation

### `@functools.singledispatch`

Example:
```python
from functools import singledispatch
@singledispatch
def fun(arg, verbose=False):
    if verbose:
        print("Let me just say,", end=" ")
    print(arg)

@fun.register
def _(arg: int, verbose=False):
    if verbose:
        print("Strength in numbers, eh?", end=" ")
    print(arg)

@fun.register
def _(arg: list, verbose=False):
    if verbose:
        print("Enumerate this:")
    for i, elem in enumerate(arg):
        print(i, elem)
```

### `class functools.singledispatchmethod(func)`
Needs more investigation

### `functools.update_wrapper(wrapper, wrapped, assigned=WRAPPER_ASSIGNMENTS, updated=WRAPPER_UPDATES)`
> Update a wrapper function to look like the wrapped function. The optional arguments are tuples to specify which attributes of the original function are assigned directly to the matching attributes on the wrapper function and which attributes of the wrapper function are updated with the corresponding attributes from the original function. The default values for these arguments are the module level constants WRAPPER_ASSIGNMENTS (which assigns to the wrapper function’s __module__, __name__, __qualname__, __annotations__ and __doc__, the documentation string) and WRAPPER_UPDATES (which updates the wrapper function’s __dict__, i.e. the instance dictionary).

# Datamodel
Source: https://docs.python.org/3/reference/datamodel.html
