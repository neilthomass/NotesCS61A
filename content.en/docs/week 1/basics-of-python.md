---
title: "Basics of Python"
---

# Basics of Python

## Expressions and Values

- Programs work to manipulate values
- Expressions in programs evaluate to values
- Values can have different data types (string, float, boolean, integer, etc.)
- Python evaluates these expressions, and then (potentially) displays its values

## Data Types

| Data Type | Example Values |
|-----------|----------------|
| Integers  | 2, 44, 25      |
| Floats    | 3.14, 2.73, 69.69432 |
| Booleans  | True, False    |
| Strings   | "Hi", "ben"    |

## Operators

These are pretty self-explanatory

| Operator | Example Expression | What it does |
|----------|-------------------|--------------|
| +        | 10 + 2            | Adds two values together |
| -        | 10 - 2            | Subtracts values |
| *        | 10 * 2            | Multiplies values |
| /        | 10 / 2            | Divides values |
| //       | 9 // 2            | Takes the floor of the divided value (result on the left would evaluate to 4) |
| %        | 9 % 2             | Takes the remainder of the division expression (result on the left would evaluate to 1) |
| **       | 2 ** 2            | Finds the value of the left value to the power of the right value |

## Call Expressions

Oftentimes, however, expressions use function calls rather than operators (and the operators above have call function equivalents!)

For example, running `add(10, 2)` does the same thing as `10 + 2` as shown in the example above, but with different form.

### Why use call functions?

One possible reason is that call functions are sometimes a lot easier to understand, especially when you have multiple nested call functions (as opposed to multiple mathematical operators)

A call function always executes the same way with the same procedure, which goes as follows:

```python
add(10, 69)
# add is the operator, 10 and 69 are the operands in this instance.
```

Python will first evaluate the operator (and), then will evaluate the operands (10, then 69), then apply the operator (function) to the operands (arguments), in that order, which can be summarized as follows:

1. Evaluate the operator
   - Usually this means check to see if it exists, but the operator itself may be a function in some situations.
2. Evaluate the operand(s)
3. Apply the operator to the operand(s)

Operators and more commonly, operands can also be written as expressions, so these must be evaluated before step 3 can occur (meaning that if you have any sort of errors in that code block, Python will throw an error!)

### Example of nested expressions

```python
from operator import add
add(add(3,add(3,2)),add(add(5,4),add(7,6)))
```

While the above example above is very impractical and will not appear in any sort of serious coding, it is a pretty good example of how Python evaluates nested expressions.

Let's unpack how Python deals with the above expression!

1. Python reads from left to right in this expression, so first the add operator will be evaluated
2. Next, the leftmost argument for add will be read, which in this case is the large block `add(3,add(3,2))`
3. As this is still another call function, the add will be evaluated, with the operands 3 and `add(3,2)`
4. 3 is a 'base case', so that does not need to be further evaluated, but `add(3,2)` goes through the same procedure, which will then evaluate to 5.
5. Now because the inner add function has been fully evaluated, the value is calculated in reverse order
6. 3 + 5 is calculated, which will return 8 for the first argument.
7. Same thing happens for the other operand, which will then evaluate to 22
8. 22 and 8 get summed, which will then result in 30

## Names

A name can be bound to a value. This does not necessarily need to be a variable - it could also be a function or an expression for instance.

Names are often used because they can be reused in different parts of the code. A name that's bound to a value is known as a variable.

For example:

```python
x = 2
y = 3

print(x + y) # Returns 5
print(x - y) # Returns -1
```

These values can also change:

```python
x = 2
print(x) 
# >>> 2
x = x + 5
print(x) 
# >>> 7
```

The equals sign used above is not similar to the one used in mathematics; it is used for assignment rather than equality, which means that you set a value to the variable.

This assignment statement works by:
1. Evaluating the expression on the right of the =
2. Binding the value of the expression to the name on the left side of the =

## Environment Diagrams

These are very useful to visualize how the Python interpreter thinks (and also appear somewhat often in exam questions). PyTutor is a very good resource to generate these environment diagrams if you ever get confused about how assignment works.

## Functions

Functions are very useful in programming languages in general because they allow lines of code to be easily reused. Functions, however, are slightly more complicated than variables due to the local and global frames (more on that later).

### What is a function?

A function is a sequence of code that can be called on at any point in the program.

While there are built in functions (like the `add(a, b)` function used earlier), sometimes, they need to be built by hand.

A function has two portions: an input (the arguments a and b) and an output (a return value). These are not required, but depending on the use case of the function, they are very useful.

### How do we write a function?

There are two common methods to write a function in Python, with the most common one being the def statement, which can be written in the format below:

```python
def <name>(<parameters>):
    return <expression>
```

For example, instead of using the built-in `add()` function from Python, we can create our own by doing the following:

```python
def add(a, b):
    return a + b
```

After the code is defined (e.g. below the function definition), you can then call it:

```python
add(5, 6)
# >>> 11
```

In python, the first line is the function signature, and all lines thereafter (there can be more than 1) are considered the function body.

### The return keyword

In a Python function statement, the return keyword is vital. What it does is return a value to the place where the function was called, and then exits the function (both properties are very important to remember!)

The return keyword acts very differently to print, even if they are in the same function. Print does not do any assignment, while return does. The example below may help illustrate my point:

```python
def example(x):
    print(x)
    return x*2

value = example(3)
# Running the above line will print 3 first, then assign 3*2 to value
print(value)
# >>> 6
```

## Frames

There are different frames, which you can think of as different rooms in the same house.

The global frame is an environment that contains all the variables and functions that were created in the main body of the program.

A function's local frame is a child of the global frame, where it has its own set of variables that can't be accessed outside the function. For example:

```python
unhelpful_name = 0 # variable in the global frame

def unhelpful_function(unhelpful_name): # variable in the local frame (even though it has the same name it isn't the same variable)
    return unhelpful_name # >>> 2
    # The above will return 2 as opposed to 0 because the unhelpful_name variable called in the function is the one passed into the function.

unhelpful_function(2)
```

## Name Lookup Rules

Python looks up names in a user-defined function using the following logic:

1. Look it up in the local frame
2. If the name is not in the local frame, look for the name in the parent frame
3. If the name is not in any searched frame, throw a NameError.

## Ending Notes

If you don't understand any of this, it is very important to ask for more help, whether that be from your peers or from your TAs, or even searching on the internet. Another very useful resource (especially for understanding how frames work) is PyTutor. Please use it. 