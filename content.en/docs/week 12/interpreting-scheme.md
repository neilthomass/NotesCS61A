---
title: "Interpreting Scheme"
---

# Interpreting Scheme

Interpreters keep going through an evaluate/apply cycle.

## Eval

### Base Case
- Primitive Values

### Recursive Case
- Evaluate(operator, operands) of call expressions
- Apply(procedure, arguments)

## Apply

### Base Case
- Built in procedures

### Recursive Case
- User defined procedures

## Scheme Specific Traits

Nearly everything in Scheme is a list (as mentioned countless times); additionally, nearly everything in scheme is a call expression where the operator is evaluated, then the operands are all evaluated, then these operands are applied to the operator.

However, there are special forms that don't follow this evaluation procedure, which means that we need to create our own ruleset for these special forms.

## Evaluation

Our `scheme_eval` function will choose its execution behaviour based on the expression form given to it:

1. If a primitive is given (booleans, numbers, nil, etc), that primitive is just returned as a value itself
2. If there's a symbol in the expression, try to look up the value in the frame.
3. All other legal expressions are represented as Scheme lists (this could be normal call expressions or special forms) - these are called combinations

## Evaluating Combinations

We could have combinations that are call expressions and combinations that are special forms. How do we tell the difference between them such that we can choose how to evaluate them?

Let's try to look at examples to try and see the difference:

```scheme
(+ 1 2)
(define x (+ 1 2))
(* 1 2)
(if #f (/ 1 0) 0)
```

Notice how the first 'token' can be used to tell whether something is a call expression or a special form. If the first token (after the bracket) matches that of a known special form, we use a different evaluation procedure, else we just do the normal scheme special from evaluation procedure.

## Symbols and Functions

### Frames

Similarly to Python, Scheme variable looks at frames to find the values bound to symbols. In our interpreter, we will represent frames as Frame class instances. Each Frame object will have a lookup and define method. In this version we will not hold return values, but some other implementations may do that.

### Define Expressions

Define would bind a symbol to a value to the first frame of the current environment that the code is in. If we need to define a procedure, what happens is that a lambda function is created, then that lambda function is the bound to the name.

### Applying User-Defined Procedures

When applying user-defined procedures, we need to create a new frame where the arguments of the functions are defined, and then makes it such that the parent of the frame is just the env attribute of the procedure (you will see this in the Scheme Project code) 