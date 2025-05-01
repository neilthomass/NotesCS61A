---
title: "Generators"
---

# What is a Generator

Generators can be used to create your own iterators with custom values. This is useful when you want to create an iterator with unpredictable results (for example numbers with irregular intervals, possibly based on different arguments in a given function).

The way you define a generator function is through the yield keyword (instead of return in a function). In this sense, a generator is a type of iterator that yields results from a generator function.

```python
def numbers():
    num = 0
    while num < 3:
        yield num
        num += 1
```

Calling the generator function will return a generator (which itself is an iterator, which means the next function works similarly to how it does for iterators):

```python
num_iter = numbers()
next(num_iter) # 0
next(num_iter) # 1
next(num_iter) # 2
next(num_iter) # StopIterator
```

## How Generators Work

When the function called, Python immediately returns an iterator without running any lines of code in the function itself.

When next is called on the iterator, it starts executing the body of the function until it hits the first yield statement. Once it hits the yield statement, the function pauses execution (not to be confused with return where the function completely stops executing), and when next is called again, the function will continue executing from where it paused. If it doesn't find any yield statements from there, it raises a StopIteration exception.

```python
def printer(n):
    print("we have started")
    while n < 3:
        yield n
        n = n + 1

printing = printer(0)

next(printing)
# we have started
# 0
next(printing)
# 1
next(printing)
# 2
next(printing)
# StopIteration
```

We can use for loops over generators like we can over iterators:

```python
def printer(n):
    print("we have started")
    while n < 3:
        yield n
        n = n + 1

for item in printer(0):
    print(item)

# we have started (goes through the function body as normal)
# 0
# 1
# 2
```

## Why Use Generators?

For some functions, using generators can save a lot of computing time. This is because they only generate the next item when needed (instead of having to find every solution in a recursive solution for example, you can just compute one element as a time (as you need it) in certain situations).

### Example: Fibonacci Numbers

Instead of an iterative solution:

```python
def fib(n):
    prev = 0
    curr = 1
    iterations = 1
    while iterations < n:
        prev, curr = curr, prev + curr
        iterations += 1
    return curr
```

we can use a generator for this:

```python
def gen_fib():
    prev = 0
    curr = 1
    while True:
        yield prev
        prev, curr = curr, prev + curr
```

This will allow you to generate Fibonacci numbers as needed.

## Yield From

The yield from keyword allows you to yield from an iterable/iterator one at a time:

```python
def lst_yielder():
    lst = [0, 1, 2, 3]
    yield from lst

lst = lst_yielder()
next(lst) # 0
next(lst) # 1
next(lst) # 2
next(lst) # 3
```

However, one of the very cool things about yield from is that you can yield the results of another generator function, which makes it act sort of like recursion in a way.

```python
def countdown(n):
    if k > 0:
        yield n
        yield from countdown(n - 1)
```

When the function hits yield from countdown(n - 1), it makes another instance of countdown, with a lower value of n, which will then pause when it reaches yield n in the function body.

## Generator Functions with Returns

Using a return keyword in a generator function does not work exactly as intended.

Remember how earlier it was mentioned that if a function was unable to find another yield statement, it would cause a StopIteration error? Well, what a return statement does traditionally is exit out of a function, which in this case means that no further yield statement will be found. As a result, a StopIteration error will occur.

```python
def f(x):
    yield x
    yield x + 1
    return
    yield x + 2

list(f(1)) # [1, 2]
```

### With Return Values

```python
def f(x):
    yield x
    yield x + 1
    return x + 1
    yield x + 2

list(f(1)) # [1, 2]
```

Notice how nothing changed? Return values can't be yielded in this way. There is a way to yield these return values, but it is not in the scope of CS61A.

## Count Partitions (Yield)

```python
def partitions(n, m):
    """List partitions.

    >>> for p in partitions(6, 4): print(p)
    4 + 2
    4 + 1 + 1
    3 + 3
    3 + 2 + 1
    3 + 1 + 1 + 1
    2 + 2 + 2
    2 + 2 + 1 + 1
    2 + 1 + 1 + 1 + 1
    1 + 1 + 1 + 1 + 1 + 1
    """
    if n < 0 or m == 0:
        return
    else:
        if n == m:
            yield str(m)
        for p in partitions(n - m, m):
            yield f"{m} + {p}"
        yield from partitions(n, m - 1)
```

This one is a bit difficult to understand. Let's break it down:

1. The base case `n < 0 or m == 0` has a return statement in it. What this means is that if we ever get to the point where either of these cases happen, a StopIteration error will occur, which in this program, means that specific combination will not be yielded.

2. In the case where `n == m`, it means that there will only be one possible combination to make up that partition, so that specific partition size will be yielded.

3. Afterwards, we use a for loop to deal with the case where we do use m (because using yield from does not allow you to change the values that you want to yield), then just yield from the case where m isn't used and it decreases by 1. 