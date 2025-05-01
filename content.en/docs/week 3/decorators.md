---
title: "Decorators"
---

# Decorators

Python decorators allow you to wrap functions inside one another easily. These may not be seen that often in the CS61A course, but in general, are pretty good to know due to the simplicity that it has as well as the common usage of it in web frameworks such as Flask, or in larger codebases.

## Example Usage

Let's take this trace function for example:

It returns a function that takes a single argument which traces the inputs and outputs of each function.

```python
def trace1(f):
    def traced(x):
        print(f"Input: {x}")
        result = f(x)
        print(f"Output: {result}")
        return result
    return traced
```

If we wanted to trace `abs(-4)` using trace1, we would call `trace1(abs)(-4)` to get an output in the console of:

```python
>>> trace1(abs)(-4)
Input: -4
Output: 4
```

You could also use it in the following way if you wanted the trace function to be called on every instance of a function being called:

```python
def square(x):
    return x*x

square = trace1(square)
```

The code above would then make sure the trace1 function is wrapped around every square call, meaning that when square is called, trace1 is always used.

However, there is a shorthand way of doing the above, which is to use a decorator:

```python
@trace1
def square(x):
    return x*x
``` 