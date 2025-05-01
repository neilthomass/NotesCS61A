---
title: "Tree Recursion"
---

# Tree Recursion

## Order of Recursive Calls

If you know the behaviour of environment diagrams, you could derive the behaviour of recursive calls.

Recaling how environment diagrams behave, a new frame is opened when a user-defined function call occurs, meaning that whenever a recursive function is called on a non-base case scenario, a new frame is opened, and that frame will be evaluated.

As a result, in a tree recursive return value (more on that later), for example `recursive(3) + recursive(4)`, the whole value of `recursive(3)` is evaluated first before `recursive(4)` is evaluated (due to the order of operations of Python).

## Tree Recursion

Tree recursion occurs when a recursive function makes more than one recursive call, thus creating a sort of network of sorts of recursive calls that is sort of shaped like the roots of a tree.

### Fibonacci Numbers

```python
def fib(n):
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    return fib(n - 1) + fib(n - 2)

fib(3)
```

As you can see in the example above, the recursive call returns two instances of the same function, thus creating a structure that calls itself multiple times in a sort of tree-like fashion.

Notice how this is pretty inefficient though! There are multiple calls to `fib(1)`, so that function would need to be evaluated multiple times (and this redundant execution would get executed more if we started with `fib(4)` for instance). There are ways to alleviate this (to a certain extent), but we'll cover that later.

## Counting Partitions

The solution to this classic tree recursion problem has a structure that can be used in other tree recursive problems — it's worth truly understanding how it works.

The question is defined as follows:

Count the number of possible partitions with positive integers n, using parts up to size m.

This problem has two distinct parts, one part calculating the possibilities with the current size m, and the other part calculating the possibilities with smaller part sizes m - 1 in a recursive manner.

For example, for `count_partitions(4, 3)`, you could have the following outcomes:

```
3 1

2 2
2 1 1

1 1 1 1
```

Which would return 4 as those are the total number of possibilities. Each line break represents a different path for the recursive solution.

In general, the recursive call can be generalized as follows `count_partitions(n - m, m) + count_partitions(n, m - 1)` which essentially is adding up the recursive function for the case where m is used, and the case where m is not used. After we have this logic, we just need to come up with the base cases, and assume that our recursive solution will work.

For our base cases, we know that if n is ever negative, there will be no possible results, and if m is equal to 0, it will not be possible to create any partitions of size n (other than 0). For n = 0, you can create 1 total partition by using partition size 0 (which is still less than m).

As a result, the code can be written as follows:

```python
def count_partitions(n, m):
    if n == 0:
        return 1
    elif n < 0 or m == 0:
        return 0
    return count_partitions(n - m, m) + count_partitions(n, m - 1)
``` 