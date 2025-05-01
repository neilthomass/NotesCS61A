---
title: "Scheme Lists"
---

# Scheme Lists

## Special Forms

### Cond & Begin

The `cond` special form behaves like if-elif-else statements in Python:

```python
if x > 10:
    print('big')
elif x > 5:
    print('medium')
else:
    print('small')
```

In Scheme:
```scheme
(cond ((> x 10) (print 'big))
      ((> x 5) (print 'medium))
      (else (print 'small)))

(cond ((> x 10) 'big)
      ((> x 5) 'medium)
      (else 'small))
```

The `begin` special form combines multiple expressions into one expression:

```python
if x > 10:
    print('big')
    print('guy')
else:
    print('small')
    print('fry')
```

In Scheme:
```scheme
(cond ((> x 10) (begin (print 'big) (print 'guy)))
      (else (begin (print 'small) (print 'fry))))

(if (> x 10) 
    (begin
      (print 'big)
      (print 'guy))
    (begin
      (print 'small)
      (print 'fry)))
```

### Let Expressions

The `let` special form binds symbols to values temporarily; just for one expression:

```python
a = 3
b = 2 + 2
c = math.sqrt(a * a + b * b)
# a and b are still bound down here
```

In Scheme:
```scheme
(define c (let ((a 3)
                (b (+ 2 2)))
           (sqrt (+ (* a a) (* b b)))))
# a and b are not bound down here
```

## Turtle Graphics

### Drawing Stars

Basic turtle commands:
- `(forward 100)` or `(fd 100)` draws a line
- `(right 90)` or `(rt 90)` turns 90 degrees

Example of drawing a star:
```scheme
(define (star n m)
  (let ((a (/ (* 360 m) n)))
    (define (side k)
      (if (< k n) 
          (begin 
            (fd 100) 
            (rt a) 
            (side (+ k 1)))))
    (side 0)))
```

## Scheme Lists

In the late 1950s, computer scientists used some confusing names for list operations:
- `cons`: Two-argument procedure that creates a linked list
- `car`: Procedure that returns the first element of a list
- `cdr`: Procedure that returns the rest of a list
- `nil`: The empty list

Important! Scheme lists are written in parentheses with elements separated by spaces:

```scheme
> (cons 1 (cons 2 nil))
(1 2)
> (define x (cons 1 (cons 2 nil)))
> x
(1 2)
> (car x)
1
> (cdr x)
(2)
> (cons 1 (cons 2 (cons 3 (cons 4 nil))))
(1 2 3 4)
```

### List Construction

There are three main ways to construct lists:

1. `cons`: Always called on two arguments - a first value and the rest of the list
2. `list`: Called on any number of arguments that all become values in a list
3. `append`: Called on any number of list arguments that all become concatenated in a list

Examples:
```scheme
scm> (define s (cons 1 (cons 2 nil)))
scm> (list 3 s)
(3 (1 2))
scm> (cons 3 s)
(3 1 2)
scm> (append s s)
(1 2 1 2)
```

### Recursive Construction

To build a list one element at a time, use `cons`. To build a list with a fixed length, use `list`.

Example of splitting a list:
```scheme
;;; Return a list of two lists; the first n elements of s and the rest
;;; scm> (split (list 3 4 5 6 7 8) 3)
;;; ((3 4 5) (6 7 8))
(define (split s n)
  (if (= n 0)
      (list nil s)
      (let ((split-rest (split (cdr s) (- n 1))))
        (cons (cons (car s) (car split-rest))
              (cdr split-rest)))))
```

## Symbolic Programming

Symbols normally refer to values, but we can refer to symbols directly using quotation:

```scheme
> (define a 1)
> (define b 2)
> (list a b)
(1 2)
> (list 'a 'b)
(a b)
> (list 'a b)
(a 2)
```

Quotation can also be applied to combinations to form lists:
```scheme
> '(a b c)
(a b c)
> (car '(a b c))
a
> (cdr '(a b c))
(b c)
```

## List Processing

### Built-in List Processing Procedures

- `(append s t)`: List the elements of s and t; append can be called on more than 2 lists
- `(map f s)`: Call a procedure f on each element of a list s and list the results
- `(filter f s)`: Call a procedure f on each element of a list s and list the elements for which a true value is the result
- `(apply f s)`: Call a procedure f with the elements of a list s as its arguments

Example:
```scheme
(define count (list 1 2 3 4))
(define beats (map (lambda (x) (list 'and 'a x)) count))
(define rhythm (apply append beats))
``` 