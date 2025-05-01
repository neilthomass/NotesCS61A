---
title: "Calculator Language"
---

# Calculator Language

## Compiled vs Interpreted

High Level Languages (basically languages with a lot of abstraction from machine code - essentially 1s and 0s) are typically either compiled or interpreted.

### Compiling

When a program is compiled, the source code is directly translated to machine code right from the start - this code can then be distributed and run repeatedly.

### Interpreted

When a program is interpreted, the source code is run directly without first compiling it. This gives interpreted languages more overhead, but also makes it such that you remove the time needed for compilation.

## Phases of an Interpreter

In order to interpret source code, programs need to be written to understand said source code.

A usual method is the following:

Source Code -> Lexing -> Parsing -> Abstract Syntax Tree

### Lexing and Parsing

The way to parse a scheme expression is to first collect the tokens of that expression (lexing), then turning it into a form readable by the language that it is to be interpreted by.

```
(+ 1 (* 3 4)) -> Scheme Expression
['(', '+', '1', '(', '*', '3', '4', ')', ')'] -> Tokenized
Pair('+', Pair(1, Pair(Pair([...])))) -> Parsed
```

### Lexical Analysis

Lexical Analysis is the process of turning an expression to its tokens. For example, the code block above shows lexical analysis of the phrase `(+ 1 (* 3 4))` to our tokens `['(', '+', '1', '(', '*', '3', '4', ')', ')']`.

This process is iterative, and processes lines one at a time.

### Syntactic Analysis

The syntactic analysis turns our tokens into a parsed form that is readable by programs. For example, the Pair class is used in the Python interpreted version of the calculator language.

This process is tree-recursive - it eventually returns a tree-like structure using linked lists and then processes multiple lines.

### Pair Class

The Pair class is what's used in CS61A to represent scheme lists.

```python
s = Pair(1, Pair(2, Pair(3, nil)))
print(s)  # (1 2 3)
len(s)    # 3
```

## Calculator Language

Programming languages have the following features:

- Syntax: The statements and expressions in the code
- Semantics: The rules for evaluation within those statements and expressions

When creating a new programming language, we need the following things:

1. Documentation (for how to use the program)
2. Canonical Implementation (an interpreter/compiler for the language)

### Syntax

Our calculator expression will only have primitive expressions (numbers) and call expressions (+, -, *, /) with nothing else. This will give us the power to write mathematical expressions in scheme, then parse it using our parser.

The image above shows an example of an expression and the lexing and parsing that occurs from it.

### Semantics

The evaluation rules for a calculation expression is defined recursively.

- Primitive: expresses to itself
- Call Expression: Evaluates to its operator applied on its operands.

If we look at the expression tree above, whenever there's a box, we need to evaluate the result from said box (which keeps going down until there are no more boxes to evaluate)

### Evaluation

This evaluation function computes the value of our expression. This function basically just looks at the data type then applies a different rule.

#### calc_eval

```python
def calc_eval(exp):
    if isinstance(exp, (int, float)):
        return exp # if number, just give back a number
    elif isinstance(exp, Pair):
        arguments = exp.rest.map(calc_eval) # evaluate the rest of the arguments in the pair
        return calc_apply(exp.first, arguments) # apply the operator to the operands
    else:
        raise TypeError
```

#### calc_apply

```python
def calc_apply(operator, args):
    if operator == '+':
        return reduce(add, args, 0)
    elif operator == '-':
        ...
    elif operator == '*':
        ...
    elif operator == '/':
        ...
    else:
        raise TypeError
``` 