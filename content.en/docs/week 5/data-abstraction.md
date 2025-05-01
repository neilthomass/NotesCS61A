---
title: "Data Abstraction"
---

# Data Abstraction

Many values in programs are compound values — a value composed of multiple values (for example coordinates, dates, or geographic positions).

By using a data abstraction, you can manipulate compound values as units without needing to worry about the way that values are stored.

## Pair Abstraction

For data that is stored in pairs, we can manipulate these values using a pair data abstraction:

```python
couple = pair("a", "b")
a = first(couple)
b = second(couple)
```

By implementing `pair()` (our constructor), `first()`, and `second()` (the selectors), you can access these elements without needing to worry about how the data is stored. The only time that people need to worry about how the data is stored is when implementing the functions themselves. One example (implying that there are multiple ways) of implementing these functions can be seen below:

```python
couple = lambda a, b: [a, b]
first = lambda lst: lst[0]
second = lambda lst: lst[1]
```

## Rational Numbers

One reasonable data abstraction to do is to implement rational numbers as a data abstraction. By storing the numerator and denominator separately, we can get precise values of certain fractions such as 1/3.

For example:

```python
half = rational(1, 2)
top = numerator(half) # 1
bottom = denominator(half) # 2
```

We have the structure for a denominator… cool I guess? But at its current state, we can't do anything with the numbers in terms of multiplying/adding/printing them in the way that we expect. As a result, we can write more functions to help us do that using functions.

```python
def mul_rational(x, y):
    return rational(
        numerator(x) * numerator(y),
        denominator(x) * denominator(y)
    )

def add_rational(x, y):
    nx, dx = numerator(x), denominator(x)
    ny, dy = numerator(y), denominator(y)
    return rational(
        nx * dy + ny * dx, 
        dx * dy
    )
```

Notice how at this point we still do not know how `rational()` is implemented.

### Implementation

```python
def rational(n, d):
    return [n, d]

def numerator(rational):
    return rational[0]

def denominator(rational):
    return rational[1]
```

However, `rational(n, d)` doesn't fully simplify the fractions, so to solve that, we can divide both n and d by the greatest common denominator:

```python
def rational(n, d):
    g = gcd(n, d)
    return [n // g, d // g]
```

## Layers of Abstraction

You might be wondering, what's the point of data abstraction?

One reason is that some things are a lot harder to code/understand (in terms of legibility) without using data abstraction in Python (for example coding a tree), but the main reason is for simplicity and extensibility.

What data abstraction does is allow changing the implementation of the function itself without actually needing to manually change all the instances of it. In addition, with a good data abstraction, the programmer will not need to know how the data is implemented, but just needs to use the constructors and selectors to do the job for them.

### Abstraction Barriers

| Layer | Examples |
|-------|----------|
| Representation/Implementation | `[x, y]`, `[0]`, `[1]` |
| Data Abstraction 1 | `make_rational`, `numerator`, `denominator` |
| Data Abstraction 2 | `mul_rational`, `add_rational` |
| User Programs | Could be anything using the layer above |

Each layer would only need to use the layer above it, meaning that when users use the data abstractions, they do not need to care about how it's implemented.

However, this requires that the abstraction barriers are not violated. For example, if you were to do:

```python
add_rational([1, 2], [3, 4])
```

You would be violating the abstraction barrier. This would not work if the implementation of rational were changed for instance. As a result, make sure to use both the constructors and the selectors instead of assuming what the implementation is.

## Dictionaries

A dictionary is another way to store multiple pieces of data, however, it is stored differently to that of lists, and is also accessed slightly differently.

Each element in a dictionary stores a key and a value as a pair, with each element separated by a , (similarly to lists) which looks like the following below:

```python
my_fruits = {"apples": 2, "bananas": 25}
```

If we had the following dictionary, we could call `my_fruits["apples"]` to get the value of apples, which in this case would return 2. If we wanted to edit the amount of apples we had, we would then assign a value to the index: `my_fruits["apples"] = 3` would mutate our dictionary that we have.

### Queries

```python
>>> "apples" in my_fruits # able to search for keys
True
>>> 2 in my_fruits # not able to search for values
False
>>> len(my_fruits)
2
```

### Rules

- keys cannot be a list or a dictionary (or any mutable type). However, values can be of any types (including dictionaries).
- There can only be one value mapped to every key (similarly to mathematical functions)

### Iteration

```python
for fruit in my_fruits:
    print(fruit, my_fruits[fruit])
    # apples 2
    # bananas 25
```

Notice that fruit iterates through the keys and not the values!

### Dictionary Comprehensions

Very similar to list comprehensions, but it uses {} instead of []; in the form of `{<key>: <value> for x in <iter>}`.

```python
{x: x*x for x in [1, 2, 3, 4]}
# would return a dictionary of 
{
    1: 1,
    2: 4,
    3: 9,
    4: 16
}
``` 