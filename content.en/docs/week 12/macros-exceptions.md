---
title: "Macros and Exceptions"
---

# Macros and Exceptions

## Macros

In Scheme, everything is a list. For example, `(quotient 1 2)` can also be seen as a Scheme list with the elements quotient, 1, and 2. What the ' operator lets us do in Scheme is create a list without evaluating certain options, then delay the evaluation until you need it later. For example:

```scheme
>>> (define lst (list 'quotient 1 2))
lst
>>> lst
(quotient 1 2)
>>> (eval lst)
0.5
>>> (eval (list 'quotient 1 2))
0.5
```

### Quoting

There are two ways to quote an expression:

1. Quoting
2. Quasiquoting

Quasiquotes are different because they allow certain elements to be unquoted using `,`. This means that names can be bound directly:

```scheme
(define a 2)
(define b 3)
'(+ ,a b) ; (+ (unquote (a)) b)
`(+ ,a b) ; (+ 2 b)
```

When you call eval on the quasiquoted expression, it saves the value of a directly into the macro which can be very powerful.

If you try to call `(eval '(+ ,a b))` you will get an error because `,` does not work unless it is in a quasiquote because Scheme.

## Exceptions

Exceptions in Python are used to handle errors. In Python, you most likely would have seen quite a few exceptions (for example ZeroDivisionError, StopIteration, etc.) - there is a way to handle these exceptions.

### try...except

To handle an exception (which keeps the program running - it doesn't make the program stop the moment an error appears), you can use Python's built-in exception handling.

```python
try:
    <body>
except <exception> as <variable>:
    <body>
[except <exception> as <variable> [...]]
[else]
```

First, the body in the try suite is executed first, then if an error is thrown that matches any `<exception>` that you put in, the body corresponding to that suite will be run with `<variable>` bound to the exception.

### Example

```python
try:
    val = 10/0
except ZeroDivisionError as e:
    print(f'handling {type(e)}') # handling <class 'ZeroDivisionError'>
    val = 0
```

### Example inside a function

```python
def div(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        result = float('inf')
    return result

div(10, 1) # 10
div(10, 0) # inf
div(10, -1) # -10
```

### What Would Python Do?

Taken from CS61A Fall 2021 Exceptions slides:

```python
def invert(x):
    inverse = 1/x # Raises a ZeroDivisionError if x is 0
    print('Never printed if x is 0')
    return inverse

def invert_safe(x):
    try:
        return invert(x)
    except ZeroDivisionError as e:
        print('ben')
        return 0

invert_safe(1/0) # Error

try:
    invert_safe(0)
except ZeroDivisionError as e:
    print('tao')
# ben
# this is because the place where the error occurs is in the try except block where 'ben' is printed
```

### Raising Exceptions

#### Assert Statements

assert statements that fail raise an exception of type AssertionError

#### Raise Statement

You can raise any type of exception by using the raise statement:

```python
raise <expression>
```

`<exception>` must evaluate to a subclass of BaseException (or an instance of one). Exceptions are constructed in the same way as do other classes:

```python
raise ZeroDivisionError("lol wtf u doing")
``` 