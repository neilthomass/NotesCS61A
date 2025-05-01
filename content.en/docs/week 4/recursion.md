---
title: "Recursion"
---

# Recursion

## Recursive Functions

A function is recursive if the body of the function calls itself (either directly or indirectly).

Recursive functions pretty much always lead to a base case by operating on increasingly smaller instances of the problem. A base case is the simplest case possible where a calculation can actually be done.

For example, a recursive function to sum the digits in a number can be written as below:

```python
def sum_digits(n):
    assert n >= 0
    if n < 10: # Base case, only happens when there's 1 remaining digit
        return n
    return sum_digits(n // 10) + n % 10 # Adds the rest of the number with the final number
# sum_digits(n // 10) just calls the sum_digits function again but with a simpler case in this case.
```

Another example is a factorial function:

```python
def factorial(n):
    assert n >= 0
    if n == 1 or n == 0:
        return 1
    else: 
        return n * factorial(n - 1)
```

If you notice how factorial works, it repeatedly multiplies n with a value one lower than itself, which allows the recursive function above to work.

As a result, the anatomy of a recursive function can be broken down into the following components:

- Base Case: The smallest sub-problem
- Recursive Case: Breaking down a problem into a smaller sub-problem
- Conditional Statement: Decides whether something is a base case or a recursive case.

## Recursion in Environment Diagrams

If we use the same factorial function from above, calling factorial(3) on it, we can visualize the function calls in the environment diagram below in two steps — first reaching the base case, then evaluating the result.

```
Global Frame
f1: factorial(x) [parent = Global]
f2: factorial(x) [parent = Global]
f3: factorial(x) [parent = Global]
func factorial(n) [parent = Global]
factorial
3
n
3 * (factorial(2))
Return Value
2
n
2 * (factorial(1))
Return Value
1
n
1
Return Value
```

After the base case is reached, each return value in the stack can be evaluated:

```
Global Frame
f1: factorial(x) [parent = Global]
f2: factorial(x) [parent = Global]
f3: factorial(x) [parent = Global]
func factorial(n) [parent = Global]
factorial
3
n
3 * 2 = 6
Return Value
2
n
2 * 1 = 2
Return Value
1
n
1
Return Value
```

As a result, we can visualize a recursive call almost like that of a stack. The values keep reducing to a simpler case until a base case is reached, and when that happens, the result for each recursive call (starting from the base case) is then evaluated, which then will combine to evaluate to the final result.

## Verifying Recursive Functions

### Domino Example

Let's use an example to illustrate how designing/verifying a recursive function can work.

Take for example that you had a line of a thousand equally spaced dominoes, and you wanted to test whether tipping one would tip all of them, you could just see if 1 domino would fall if tipped, then assume that any domino will tip the next one, then verify that tipping the first domino tips the next one.

This can be generalized with two different methods:

### Recursive Leap of Faith

Steps:

1. Verify the base case — make sure it's functional and works properly
2. Assume that a simplified case of the function is correct (← leap of faith)
3. Verify that the function itself returns the simplified function calls correctly

For a more concrete example, take the factorial function from above:

1. Verify that `return 1 if n == 0 or n == 1` is the correct base case
2. Assume that `factorial(n - 1)` returns the correct value
3. Verify that `n * factorial(n - 1)` is the correct statement

### Recursive Cat's Promise

While this usually isn't a cat's promise, but rather an elf, I like cats, so I'm going to go with cats.

With this perspective, the recursive cat handles the simplified recursive call, and promises that they will calculate the smaller recursive call for you while you handle the rest.

For example, to calculate 3!, you would ask yourself how you could calculate 3! if you knew the value of 2!, which in this case, is simply 3 * 2! — as a result, you do! Then, the recursive cat promises to handle the result of factorial(2) for you. (The recursive cat then calls on itself to find the value of factorial(1), but you do not need to know that to be able to solve factorial(5) — all you need is the value of factorial(4))

## Mutual Recursion

When recursive functions are defined in terms of each other, the functions are mutually recursive.

For example, we can have this (useless) function that determines whether a number is even or odd in a mutually recursive manner:

```python
def even(n):
    if n == 0:
        return True
    else:
        return odd(n - 1)

def odd(n):
    if n == 0:
        return False
    else:
        return even(n - 1)

print(even(4)) # True
```

Note:
Mutually recursive functions can be written as a single recursive function by simply breaking the abstraction boundary between the two functions. For example, the code above can be written in the following manner:

```python
def even(n):
    if n == 0:
        return True
    else:
        if (n - 1) == 0:
            return False
        return even((n - 1) - 1)
```

As you can see, the code checks whether a number is even initially, then checks for the odd case and thus, deals with 2 digits in 1 go. However, this implementation is far more convoluted than the mutually recursive version (and you can imagine how much more complex this would be with more complexity), so using a mutually recursive solution can simply be a mechanism for keeping your code simple; in other words: maintaining abstraction.

A good indicator of when a mutual recursive solution could be used is when there is a natural recursive solution, but there is more than 1 case that needs to be checked for (in the above example, whether the number was even or odd)

## Recursion + Iteration

### Converting Recursion to Iteration

First, you must figure out what state needs to be maintained by the iterative function that the recursive function would store itself. For example, in the factorial function, the recursive function works by multiplying n with a simpler version of the factorial function, meaning that the iterative version would need something to store n and multiply it with n - 1 until a 'base case' is reached.

For instance:

```python
def recursive_factorial(n):
    assert n >= 0
    if n == 1 or n == 0:
        return 1
    else:
        return n * recursive_factorial(n - 1)

def iterative_factorial(n):
    k = n - 1
    while k > 0: # 'Base Case'
        n = n * k # Does the multiplication
        k = k - 1 # Decrements n, similar to recursive_factorial(n - 1)
    return k
```

### Converting Iteration to Recursion

Converting iteration to recursion is sometimes easier than doing it the other way around. Essentially, the state of an iteration can be passed in as arguments to the recursive function:

```python
def iterative_sum_digits(n):
    total = 0
    while n >= 10:
        digit_sum = digit_sum + n % 10 # 
        n = n // 10
    return total

def recursive_sum_digits(n, total):
    if n < 10:
        return n
    else:
        return(n // 10, total + n % 10) # Iterative variables passed as arguments for the recursive version
```

## Helper Functions

If a recursive function ever needs to keep track of more variables than the original function provides, you probably need a helper function for that.

```python
def is_prime(n):
    '''Returns True if n is a prime number, else returns False
    >>> is_prime(13)
    True
    >>> is_prime(14)
    False
    >>> is_prime(2)
    True
    '''
    assert n >= 2
    def helper(i):
        if i == n:
            return True
        elif n % i == 0:
            return False
        else:
            return helper(i + 1)
    return helper(2)
``` 