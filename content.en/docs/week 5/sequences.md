---
title: "Sequences"
---

# Sequences

## Lists in Environment Diagrams

Lists can represented with box and pointer notation (similarly to functions); however, unlike functions, each element in the array has its own box, and is index labelled.

What this implies is that assigning a variable to another list will not create a copy of that list, but rather point towards the same list — this ends up being a correct assumption to make.

Each box can either hold a value (for example a number or a string), or an object (for example, a function, another list, or a Class).

## List Slicing

Slicing a list creates a new list (as in it points to a separate list). The behaviour is very similar to the range() function — it starts on the first 'argument' provided, ends on the number right before the second 'argument', with step of the third 'argument'. In this case however, the separator is : rather than ,. If there is no argument provided next to the :, it defaults to 0.

The syntax is `lst[<start>:<end>:<step_size>]`

Below are some examples:

```python
lst = [1, 2, "bananas"]
lst[0:] # [1, 2, "bananas"]
lst[:2] # [1, 2]
lst[1:] # [2, "bananas"]
lst[::2] # [1, "bananas"]
```

This behaviour also works with strings:

```python
string = "benbaron"
string[3:] # "baron"
string[:3] # "ben"
```

## Small Practice Problems

### Recursion in Lists

Imagine summing the numbers in a list but using recursion rather than iteration or the sum() function.

```python
def sum_numbers(lst):
    '''Returns the sum of the numbers in lst
    >>> sum_numbers([2, 3, 4])
    9
    '''
```

We could implement the function above using list comprehension:

```python
def sum_numbers(lst):
    '''Returns the sum of the numbers in lst
    >>> sum_numbers([2, 3, 4])
    9
    '''
    if lst == []: # base case
        return 0
    else:
        return lst[0] + sum_numbers(lst[1:]) 
        # takes the first number and 
        # recursively calls the function on a smaller list.
```

### Reversing a String (Recursively)

```python
def reverse_string(word):
    '''Reverse the string provided in word
    >>> reverse_string("ben")
    neb
    '''
```

Our base case here would be when the word provided is an empty string.

```python
def reverse_string(word):
    '''Reverse the string provided in word
    >>> reverse_string("ben")
    neb
    '''
    if word == "":
        return ""
    else:
        return reverse_string(word[1:]) + word[0]
        # keep in mind the order of the operation above matters
        # word[0] is put afterwards because
        # that should be added up later
```

## Built-in functions for Iterables

| Function | Description |
|----------|-------------|
| `sum(iterable, start)` | Returns the sum of the values in iterable, with a starting sum of start (defaults to 0) |
| `all(iterable)` | Returns True if all the values of iterable are Truthy values (or if the iterable is empty), else returns False |
| `any(iterable)` | Returns True if any of the values of iterable are Truthy, else returns False |
| `max(iterable, key=None)` | Returns the maximum value in iterable |
| `min(iterable, key=None)` | Returns the minimum value in iterable |

### Examples of the built-in functions

```python
sum([3, 2, 1], 50) # 56

any([True, False, False, False]) # True
any([3, 2, 1, 0]) # True
all([3, 2, 1]) # True
all([True, False, False, False]) # False

max([3, 2, 1]) # 3
max(["a", "b", "c"]) # "c"
max(range(10)) # 9

# the 'key' parameter can be used to compare certain elements in an array:
coords = [[1, 2], [4, 3], [3, 90]]
max(coords, key = lambda x: x[0]) # [4, 3] (x iterates through coords and then checks the max value of the first element)
``` 