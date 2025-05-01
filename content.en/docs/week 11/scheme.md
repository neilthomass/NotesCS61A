---
title: "Scheme Specification"
---

# Scheme Specification

This document describes the variant of Scheme used in CS61A, which is closest to R5RS with some modifications for educational purposes.

## Overview and Terminology

### Expressions and Environments
- Every expression evaluates to a value
- Some expressions are self-evaluating (numbers, booleans, strings, nil)
- Frames map symbols to values with optional parent frames
- Environment lookup follows the chain of parent frames

### Atomic Expressions
- Numbers, booleans, strings, and nil are self-evaluating
- Symbols evaluate to their bound values in the environment

### Call Expressions
Most combinations are evaluated as call expressions with three steps:
1. Evaluate the operator (must be a procedure)
2. Evaluate the operands in order
3. Apply the procedure to the evaluated arguments

### Special Forms
Special forms have their own evaluation rules and are identified by keywords:
```scheme
(if <predicate> <consequent> [alternative])
(cond <clause> ...)
(and [test] ...)
(or [test] ...)
(let ([binding] ...) <body> ...)
(begin <expression> ...)
(lambda ([param] ...) <body> ...)
(quote <expression>)
(quasiquote <expression>)
(mu ([param] ...) <body> ...)
(define-macro (<name> [param] ...) <body> ...)
```

## Types of Values

### Numbers
- Built on Python's number types
- Supports arbitrarily-large integers and double-precision floats
- Case-insensitive (except in strings)

### Booleans
- Two values: `#t` and `#f`
- Can be input as `true` or `false`
- Only `#f` is false in boolean contexts

### Symbols
- Used as identifiers
- Valid characters: alphanumeric and `!$%&*/:<=>?@^_~+-.`
- Stored internally in lowercase
- Cannot form valid numbers

### Strings
- Atomic data type (no individual characters)
- Immutable
- Entered as characters inside double quotes
- Limited string manipulation support

### Pairs and Lists
- Pairs have `car` and `cdr` fields
- Lists are either `nil` or pairs with list `cdr`
- Streams use promises in their `cdr`
- List literals can be constructed with `quote`

### Procedures
- First-class values
- Types:
  - Built-in procedures
  - Lambda procedures
  - Mu procedures
  - Macro procedures

### Promises and Streams
- Promises represent delayed evaluation
- Created with `delay`
- Evaluated with `force`
- Streams are pairs with promise `cdr`
- Special forms: `cons-stream`, `cdr-stream`

## Special Forms in Detail

### define
```scheme
(define <name> <expression>)
(define (<name> [param] ...) <body> ...)
```
Binds values or creates procedures in the current environment.

### if
```scheme
(if <predicate> <consequent> [alternative])
```
Conditional evaluation based on predicate.

### cond
```scheme
(cond <clause> ...)
```
Multiple condition evaluation with optional else clause.

### let
```scheme
(let ([binding] ...) <body> ...)
```
Creates new frame with bindings for body evaluation.

### begin
```scheme
(begin <expression> ...)
```
Evaluates expressions in sequence, returns last value.

### lambda
```scheme
(lambda ([param] ...) <body> ...)
```
Creates procedure with lexical scoping.

### quote
```scheme
(quote <expression>)
'<expression>
```
Returns unevaluated expression.

### quasiquote
```scheme
(quasiquote <expression>)
`<expression>
```
Returns unevaluated expression with selective evaluation.

### mu
```scheme
(mu ([param] ...) <body> ...)
```
Creates procedure with dynamic scoping.

### define-macro
```scheme
(define-macro (<name> [param] ...) <body> ...)
```
Creates macro procedure for defining new special forms.

## Best Practices

1. **Use proper scoping**
   - Lambda for lexical scoping
   - Mu for dynamic scoping
   - Macros for new special forms

2. **Handle promises carefully**
   - Only force once
   - Check for proper return types
   - Handle errors appropriately

3. **Use appropriate data structures**
   - Lists for sequential data
   - Streams for infinite sequences
   - Pairs for binary relationships

4. **Follow evaluation rules**
   - Understand special forms
   - Know when expressions are evaluated
   - Use quote appropriately

5. **Write clear code**
   - Use meaningful names
   - Comment complex logic
   - Format consistently 