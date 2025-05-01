---
title: "Efficiency"
---

# Efficiency

The efficiency of an algorithm is very important. While it isn't too important in CS 61A due to the (relatively) small data set and number of computations needed, in big sets of data or some extremely computer taxing computations, efficiency makes a world of a difference in terms of actually being able to make computations.

However, CS 61A still wants you to understand how to calculate efficiency in a broad sense.

## Orders of Growth

Here are some common orders of growth:

| Order of growth | Description |
|----------------|-------------|
| Constant Growth | Regardless of the input size, it will take the same amount of time |
| Logarithmic Growth | The number of steps increases proportionately to the logarithm of the input size (pretty good to aim for) |
| Linear Growth | The number of steps increases in direct proportion to the input size |
| Quadratic Growth | The number of steps increases in proportion to the square of the input size |
| Exponential Growth | The number of steps increases faster than a polynomial function can |

One way to tell the order of growth is to create a table of results comparing the input size with the number of operations. From there, we can extrapolate and see what the order of growth actually is.

### Adding to the front of a linked list

| Input Size | Operations |
|------------|------------|
| 1 | 1 |
| 10 | 1 |
| 100 | 1 |
| 1000 | 1 |

If we look at this table, we can see that adding to the front of a linked list can be done in constant time (always the same number of operations depending on the input size). This is often referred to as O(1).

### Fast Exponentiation

```python
def exp_fast(b, n):
    if n == 0:
        return 1
    elif n % 2 == 0:
        return square(exp_fast(b, n//2))
    else:
        return b * exp_fast(b, n-1) 

square = lambda x: x * x
```

This function finds what b ** n is. You don't really need to care about how it's implemented (other than the fact that it is implemented)

| Input Size | Operations |
|------------|------------|
| 0 | 1 |
| 8 | 5 |
| 16 | 6 |
| 1024 | 6 |

In general, you can see that as the input size goes up, the number of operations increases in a logarithmic manner (where the number of operations doesn't really increase too much). This is a pretty good thing to strive for if possible because most of the time constant time is very hard to achieve.

### Finding value in a linked list

| Input Size | Operations |
|------------|------------|
| 1 | 1 |
| 10 | 10 |
| 100 | 100 |
| 1000 | 1000 |

Pretty simple here - increases in linear (O(n)) time.

### Nested For Loop over a 5x5 array

```python
lst1 = [1, 2, 3, 4, 5]
lst2 = [2, 3, 4, 5, 6]

for one in lst1: # takes 5 operations
    for two in lst2: # 5 operations for cycles = 25 operations in total
        print((one, two)) 
```

| Input Size | Operations |
|------------|------------|
| 1 | 1 |
| 5 | 25 |
| 10 | 100 |
| 20 | 400 |

Somewhat similarly obvious - this increases in quadratic time (O(n^2))

### Recursive Fibonacci

Remember our recursive solution for calculating fibonacci numbers? Let's see the efficiency of that.

| Input Size | Operations |
|------------|------------|
| 1 | 1 |
| 2 | 3 |
| 7 | 41 |
| 8 | 67 |
| 20 | 21891 |

This increases even faster than quadratic time. It grows in exponential time (O(2^n))

Similar thing can apply for space efficiency - same rules apply, just for storage/memory needed rather than the approximate number of operations.

## Memoization

Memoization is a strategy used to reduce redundant computation by 'remembering' the status of previous function calls.

By storing the results of every computation we've gone through so far, we do not need to recalculate fib(2) on the right branch.

By making a memoization higher-order function, we can use a function decorator to make all of our function calls 'cache' its results.

```python
def memo(fn):
    cache = {}
    def memoizer(n):
        if n not in cache:
            cache[n] = fn(n)
        return cache[n]
    return memoizer
```

This way, when we call our new Fibonacci function, we grow in linear time rather than exponential time in terms of time complexity (but space complexity is obviously larger to the point of being O(n))

```python
def memo(fn):
    cache = {}
    def memoizer(n):
        if n not in cache:
            cache[n] = fn(n)
        return cache[n]
    return memoizer

@memo # basically does the same as saying fib = memo(fib), which makes it such that fib now points to the memoizer function
def fib(n):
    if n == 0 or n == 1:
        return n
    else:
        return fib(n-2) + fib(n-1)

# Now calling fib(n) will be a lot more efficient
``` 