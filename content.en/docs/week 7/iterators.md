---
title: "Iterators"
---

# What is an Iterator

An iterator is an object that provides sequential access to values in an iterable one by one.

While you may not see the use of them at the moment, the benefits of using them will be explained a bit later in these sets of notes.

- `iter(iterable)` returns an iterator over the elements of iterable.
- `next(<iter>)` returns the next element in an iterator.

```python
numbers = [1, 2, 3]

iter_numbers = iter(numbers)

next(iter_numbers) # 1
next(iter_numbers) # 2
next(iter_numbers) # 3
next(iter_numbers) # Error (StopIterator exception)
```

In CS61A, questions will never get to the point where handling the StopIterator error will need to happen, but a try ... except code block can handle that error. If you ever get a StopIterator error in your HW or Lab questions, you can just assume you did something wrong.

You can think of an iterator like a bookmark and a book.

Imagine we have a list lst that contains chapters of a book, with every element containing a single chapter:

```python
lst = [1, 2, 3, 4, 5, 6]
```

When we call iter on said list, we essentially create a bookmark starting on chapter 0 (before any other chapters). After finishing a chapter, calling next on the iterator will move the bookmark to chapter 1.

```python
lst = [1, 2, 3, 4, 5, 6]
bookmark1 = iter(lst)

# [1, 2, 3, 4, 5, 6] - no bookmark so far
# ^

next(bookmark1) # 1

# [(1), 2, 3, 4, 5, 6] - bookmark on 1
#   ^
```

If we wanted to create a new bookmark with the same book, we can just call iter again, which would keep bookmark1 the same.

```python
lst = [1, 2, 3, 4, 5, 6]
bookmark1 = iter(lst)
bookmark2 = iter(lst)

next(bookmark1) # 1
next(bookmark1) # 2
# [1, (2), 3, 4, 5, 6] - bookmark on 2
#      ^
next(bookmark2) # 1
# [(1), 2, 3, 4, 5, 6] - bookmark on 1
#   ^
```

## Calling an Iterator on an Iterator

If you call iter on an already-made iterator, similarly to lists, it just points to the same iterator. With our bookmark analogy, it's like getting another variable pointing to the same bookmark — calling next on one instance will also update the bookmark for the other variable.

```python
lst = [1, 2, 3, 4, 5, 6]
bookmark1 = iter(lst)
alt_bookmark1 = iter(bookmark1)

next(bookmark1) # 1
next(alt_bookmark1) # 2
```

## Calling Iterators on Iterables

strings, lists, dictionaries, tuples, ranges, etc. are all iterables. You can call iterators on them and access them sequentially. This should be pretty obvious, but there are special methods that exists for dictionaries.

### Dictionaries

```python
numbers = {
    "one": 1,
    "two": 2,
    "three": 3
}

# Iterator for keys
key_iter = iter(numbers.keys())
next(key_iter) # "one"

# Iterator for values
value_iter = iter(numbers.values())
next(value_iter) # 1

# Iterators for key/value tuples
dict_iter = iter(numbers.items())
next(dict_iter) # ("one", 1)
```

## Iterators with For Loops

When an iterator is used in a for ... in loop, Python will keep calling next on the iterator until a StopIterator error occurs.

```python
lst = [1, 2, 3]
iter_lst = iter(lst)

for item in iter_list:
    print(item)

# 1
# 2
# 3

next(iter_lst) # StopIterator error
```

The for loop would go through the whole book, moving the bookmark to the end. As a result, when you call next again, the bookmark would be at the end, meaning that there are no more additional chapters, so a StopIterator error will occur.

This behaviour implies that iterators are mutable. One an iterator moves forward, it is unable to return the values that came before.

## Useful Built-in Functions

### Functions that return iterables

| Function | Description |
|----------|-------------|
| `list(iterator)` | Returns a list of all items in iterator starting on the current pointer location. |
| `tuple(iterator)` | Returns a tuple of all items in iterator starting on the current pointer location. |
| `sorted(iterator)` | Returns a sorted list of all items in iterator starting on the current pointer location. |

### Functions that return iterators

These are slightly more complicated, so examples will be given for each of the following below:

#### reversed(sequence)
Iterates over item in sequence in reverse order. Returns an iterator.

```python
lst = [1, 2, 3]

print(list(reversed(lst))) # [3, 2, 1]

# list() needs to be called because reversed returns an iterator

for item in reversed(lst):
    print(item)

# 3
# 2
# 1
```

#### zip(*iterables)
Iterates over co-indexed tuples with elements from each iterable. Stops at the shortest list.

```python
lst1 = [1, 2, 3]
lst2 = [4, 5, 6]

for t in zip(lst1, lst2):
    print(t)

# (1, 4)
# (2, 5)
# (3, 6)

for x, y in zip(lst1, lst2):
    print(x + y)

# 5
# 7
# 9
```

## Map and Filter

For map and filter specifically, you can simply use list comprehensions — they're shorter and far more 'Pythonic'. However, knowing the notation for these two functions can help you recognize them in other languages.

### map(function, iterable)

```python
lst = [1, 2, 3]
mapped_lst = map(lambda x: x*2, lst)
print(list(mapped_lst)) # [2, 4, 6]

# Which is the same thing as
[x*2 for x in lst]
```

### filter(function, iterable)

```python
lst = [1, 2, 3]
mapped_lst = map(lambda x: x % 2 == 1, lst)
print(list(mapped_lst)) # [1, 3]

# Which is the same thing as
[x for x in lst if x % 2 == 1]
``` 