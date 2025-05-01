---
title: "Higher Order Functions"
weight: 2
---

# Higher Order Functions

## Designing Functions

### Describing Functions

| Aspect | Example |
|--------|---------|
| The domain of a function is the expected range of inputs (similarly to a domain in mathematics) | x is a string |
| The range of a function is the set of output values that could be returned | Function square returns a non-negative number |
| The behavior of a (pure) function is the relationship between the input and the output | Function square returns the square of an input x |

## Don't Repeat Yourself!

When making a function, give each function exactly one job, but allow it to be flexible to apply to many related situations. Doing this allows you to remove redundant code and make your code clearer to read, and easier to write.

## Generalization

If you see a common structure, you can refactor your code to move the redundant code to a function and call that instead.

For example, if we take this code block below:

```python
def print_ben_if_positive(n):
    if n > 0:
        print("ben")

def print_ben_if_positive(n):
    if n > 0:
        print("tao")
```

We can see that the if statement is duplicated, so we can take that logic to another function instead, removing redundant code.

```python
def positive_print(number, message):
    if number > 0:
        print(message)

positive_print(n, "ben")
positive_print(n, "tao")
```

## Higher-order Functions

A higher-order function is a function that takes another function as an argument, or returns a function as its result.

### Functions as Arguments

A function can take another function as an argument.

```python
from operator import add, sub

def function_in_function(adding_function, x, y):
    return adding_function(x, y)

function_in_function(add, 3, 2) # >>> 5
function_in_function(sub, 3, 2) # >>> 1
```

In the example above, we passed the `add` and `sub` functions to the function `function_in_function`. This is an example of a higher order function.

Note that in the call functions themselves, `add` and `sub` were called rather than `add()` and `sub()`. This is because of how call functions are evaluated. Running `add()` directly will make the Python interpreter try to evaluate the function itself with no parameters, which would not work, while simply typing `add` will allow `function_in_function` to evaluate `add()` with the proper parameters.

### Functions as Return Values

Another way to have a higher order function is to return a function.

Functions created in other functions are bound to names in their local frame, allowing for cleaner naming when creating function calls. For example:

```python
def make_adder(magnitude):
    def adder(n):
        return n + magnitude
    
    return adder

add_two = make_adder(2)
add_three = make_adder(3)

print(add_two(2), add_three(2)) # >>> 4 5
```

While the example above may not have much practical use, it shows the process of returning a function within a function, making this another higher order function. Take your time to digest how the above two examples work. This will be harder to conceptualize at the beginning.

### Call Expressions as operator expressions

You can run a function like `make_adder(2)(3)` which will return 5. Why is that? Let's look at how the call function order works.

1. `make_adder(2)` is first evaluated, which returns a function `adder` similarly to the function above.
2. Due to `adder` being returned from the operator, the final statement will end up being `adder(3)`, leading to 5 being the final output

## Lambda Expressions

Lambda expressions pretty much work the same way as regular functions, other than the fact that these functions are anonymous - they do not have a name assigned to it.

The syntax looks like the following:

```python
lambda <parameters>: <expression>
```

For instance:

```python
double = lambda x: y*2

double(3) # >>> 6
```

While this may not seem very useful in this context, there are quite a few uses for it. The main idea is that lambda functions are better suited for functions that you want to access in the short term, or only access once.

A lambda function does not contain any statements at all, including if statements and return statements.

For instance, instead of doing:

```python
from operator import add

def square(x):
    return x**2

add(2, square(2))
```

You can do the following:

```python
from operator import add

add(2, lambda x: x**2)
```

## Conditional Expressions

lambda functions not allowing statements can be quite limiting, but that is where conditional expressions come in.

A conditional expression has a form that almost reads like English:

```python
<do this (true)> if <condition> else <do this (false)>
```

This may be strange if you're used to ternary operators in other languages (for example JavaScript) as the order of reading the Python statement may be a bit strange.

However, the operation order is the following:

1. Check the truthiness of `<condition>`
2. If true, evaluate `<do this (true)>`
3. Else, evaluate `<do this (false)>`

In conjunction with lambda functions, you can do the following:

```python
lambda x: x if x > 0 else 0
```

The code block above returns the input if it is positive, else returns 0. 