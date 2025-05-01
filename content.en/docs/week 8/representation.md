---
title: "String Interpolation and Representation"
---

# String Interpolation and Representation

## String Interpolation

This is not part of representation, but is instead an extremely useful tool to make writing strings with multiple variables far cleaner. You may have already seen me use string interpolation earlier on in the course.

In Python, the cleanest way to use string interpolation is with an f-string, where the letter f is appended before quotation marks.

```python
f""
```

You can see the syntax highlighting sees the f in a different colour! With this notation, we can then put expressions in our string itself and have them evaluate to 'proper' values. Different languages deal with string interpolation in different manners.

```python
one = 1
two = "two"
five = "five"
f"{one + 1} {two}, {five}" # will return "2 two, five"
```

## Representation

### Built-in Object Attributes

In Python, every built in type inherits from object. Everything in Python is an object of some sort. As a result, they all have their own dunder methods that are different for every object.

If you run dir() on an object (which could be a string, list, or something else), you will see a list of its methods. Behind the scenes, Python runs these dunder methods, so we don't really need to worry about those (kind of like data abstraction if you think about it)

### String Representation

The `__str__` method returns a string representation made to be human readable.

The use of this method is most likely better with a concrete example, so let's define our own class for Rational numbers (similarly to the data abstraction we were using earlier).

```python
from math import gcd

class Rational:
    def __init__(self, numerator: int, denominator: int): 
        g = gcd(numerator, denominator)
        self.numerator = numerator // g
        self.denominator = denominator // g
```

So far, if we make an instance of the class and then print that instance, we get a non-useful output:

```python
>>> my_rational = Rational(2, 3)
>>> print(my_rational)
<__main__.Rational object>
```

When we define our own `__str__` method, calling print() on the instance will look for the `__str__` method and return the value. However, the print method removes quotes, while calling str() on the same thing will not remove quotes (an example will be given in the doctests).

```python
class Rational:
    """
    >>> my_rational = Rational(2, 3)
    >>> print(my_rational)
    2 / 3
    >>> str(my_rational)
    '2 / 3'
    """
    def __init__(self, numerator: int, denominator: int): 
        g = gcd(numerator, denominator)
        self.numerator = numerator // g
        self.denominator = denominator // g

    def __str__(self):
        return f"{self.numerator} / {self.denominator}" # We define how our class will look in the console here
```

### Machine Representation

On the other hand, the `__repr__` method is used to return a string that would evaluate to an object with the same values (in a traditional sense - when you override the `__repr__` method, you can set it to anything you want)

The goal is to make it such that when you call eval() on the result, it should return the same valued object (but not the same pointer).

```python
class Rational:
    """
    >>> my_rational = Rational(2, 3)
    >>> my_rational
    Rational(2, 3)
    >>> eval(Rational.__repr__(my_rational))
    Rational(2, 3)
    >>> repr(my_rational)
    'Rational(2, 3)'
    """
    def __init__(self, numerator: int, denominator: int): 
        g = gcd(numerator, denominator)
        self.numerator = numerator // g
        self.denominator = denominator // g

    def __str__(self):
        return f"{self.numerator} / {self.denominator}"

    def __repr__(self):
        return f"Rational({self.numerator}, {self.denominator})"
```

Notice how calling repr(my_rational) returned what you would expect but in quotation marks.

### Implicit calling of print()

| Type | Methods of Calling |
|------|-------------------|
| Implicitly calls print() | Directly calling in interactive environment; print() |
| Does not implicitly call print() | repr(), str() |

Doing print("Ben") to the console will output Ben (without the quote marks). In other words, calling print() on something removes a set of quote marks. However, calling str("Ben") in the console will output 'Ben' instead, suggesting that it doesn't call print() to remove the set of quotes. If we directly output something to the console, it first calls repr() on it, then prints it to the console. So overall, when we just call something, it does the equivalent of print(repr("Ben")), which is the same as print("'Ben'"), which will remove the outside set of quotes and then return 'Ben' to the console.

```python
print("Ben") # Ben
"Ben" # 'Ben' (comes from print(repr("Ben")))

repr("Ben") # "'Ben'"
str("Ben") # 'Ben'
```

## Special Methods

There are other special dunder methods that map to built-in behaviour. For example, the `__add__` method is called when two objects are added together:

```python
2 + 3 # is the same as 2.__add__(3)
```

If we wanted to add two of our Rational classes together, it would error because there is currently no `__add__` method defined. However, if we were to define our own `__add__` method, we could make it such that Python would know how to deal with addition!

```python
class Rational:
    def __init__(self, numerator: int, denominator: int): 
        g = gcd(numerator, denominator)
        self.numerator = numerator // g
        self.denominator = denominator // g

    def __add__(self, other):
        assert isinstance(other, Rational) # Will require that the other thing passed in was a rational

        new_numerator = self.numerator * other.denominator + other.numerator * self.denominator
        new_denominator = self.denominator * other.denominator
        return Rational(new_numerator, new_denominator)

    def __str__(self):
        return f"{self.numerator} / {self.denominator}"

    def __repr__(self):
        return f"Rational({self.numerator}, {self.denominator})"
```

This can be done for other methods like `__mul__` - you just have to implement them yourselves.

## Weird behaviour of str and repr

### repr()

Will ignore any instance variables created called `__repr__`. Only looks for this instance variable in the class.

### str()

Will ignore any instance variables created called `__str__`. Only looks for this instance variable in the class.

If no `__str__` is found (in the whole lookup order), it defaults to the first `__repr__` it can find. 