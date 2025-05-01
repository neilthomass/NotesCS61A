---
title: "Control"
weight: 1
---

# Control

## Side Effects

Side effects occur in functions when the function alters the global environment in some form. This could be in the form of altering a variable in the global scope, or using a print statement inside a function. One easy way to tell if a function contains side effects is that if a function acts like a mathematical function, it has no side effects.

A Mathematical function f(x) takes inputs and always provides the same outputs, without doing anything else. As a result, one input will never have two or more different outputs. If a programming function has the same property where certain outputs always provide the same inputs without doing anything else, it has no side effects.

### Example With No Side Effects

```python
def no_side_effects(x): # Squares x
    return x*x

no_side_effects(2) # >>> 4
no_side_effects(2) # >>> 4
```

In this example, every input will always map to the same output as the function does not do anything other than provide an output given an input.

### Example With Side Effects

```python
ben = 1

def with_side_effects(x):
    return x*ben

with_side_effects(2) # >>> 2
ben = 2
with_side_effects(2) # >>> 4
```

While this example is not going to be used in a practical setting, it serves as a good example of what a side effect is. As you can see, even though we called the function `with_side_effects` twice with the same input, the output was different, which goes against the definition of a mathematical function. As a result, there is a side effect in that function.

A side note:

While the print function does not change the value of any variables, it does cause a side effect by outputting something to the console, which does go against how a mathematical function works. However, while side effects are usually better to avoid where possible, print could sometimes be useful for debugging.

An example of a side effect that can't be avoided is writing to a file - this is necessary at times.

A function without side effects is also known as a pure function, while a function with side effects is also known as an impure function.

## The 'None' Value

In Python, any function that does not return a value will return None, which when printed will render None to the console, and when called, will not render anything in the console.

For example:

```python
def no_return(x):
    x = 1

print(no_return(2))
# >>> None
```

Note that the `print()` function has no return value, and thus will return None when called.

## Nested Print Statements

A nested print function is somewhat weird, but worth it to learn.

Take this for example, what do you think will get outputted to the console? Try to think for yourself before opening the answer box below.

```python
print(print(1))
```

For a harder example, try this one:

```python
print(print("x"), print("y"))
```

## Default Arguments

In the function signature, one of the inputs can have a default value. This is useful in situations where there is a most likely case for a function, but where it still makes sense for users to have some control.

For example, the default `round()` function in Python takes in 1 required parameter, with 1 optional parameter (which defaults to 0).

```python
round(2.5342) # >>> 3
round(2.5342, 2) # >>> 2.53
```

You can build your own function with default arguments by simply specifying it in the header.

For example:

```python
def ben(baron, box="tao"):
    return baron + box

ben("baron") # >>> 'barontao'
ben("baron", "hej") # >>> 'baronhej'
```

Without the second optional parameter hej, the function defaulted to the value tao.

If you have multiple default arguments, you can also override them in this way:

```python
def ben(baron, box="tao", foo="baz"):
    return baron + box + foo

ben("baron", foo="yu") # >>> barontaoyu
ben("baron", box="yu") # >>> baronyubaz
```

## Multiple Return Values

One aspect of Python uncommon in other languages is the allowed use of multiple return values in functions. This can be done in a function by using multiple return values separated by a comma.

Any code that calls the function can either store it in a variable as a tuple (more on this later) or can be unpacked. For example:

```python
def return_two_values():
    return 1, 2

return_two_values() # >>> (1, 2) in tuple form

a, b = return_two_values() 
a # >>> 1
b # >>> 2
```

## Multiple Variable Assignment

The values on the right side are evaluated first before being assigned, so you can swap the values of two variables in one line by simply doing the following:

```python
x, y = y, x
```

## Boolean

A Boolean is a value that is either True or False, and is used frequently in many applications. For example, your mobile device would likely have a Boolean variable that stores whether your WiFi, flashlight, bluetooth etc. is turned on.

An expression can evaluate to a Boolean. For example:

```python
passed_class = grade >= 70 # Will evaluate either true or false depending on the condition
take_shower = (not eecs_major) or did_sports
```

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| == | Equality |
| != | Inequality |
| > | Greater Than |
| < | Less Than |
| >= | Greater Than or Equals |
| <= | Less Than or Equals |

**Checking for Equality**: It is a common mistake to use = instead of == to check for equality. Please remember that = is for assigning a variable and cannot be used for checking for equality in a conditional statement. Python will throw a syntax error, but other languages may not, so not mixing these up is a good habit to get used to.

### Logical Operators

| Operator | Meaning |
|----------|---------|
| and | Evaluates to True if both values are True |
| or | Evaluates to True if any of the values are True |
| not | Evaluates to True if the value is False, else evaluates to True |

### Execution rules of logical operators

The statements are evaluated from left to right, but sometimes, these statements do not all need to be evaluated.

**and statement procedure:**
1. Evaluate the left statement.
2. If it evaluates to a False value x, the expression evaluates to x.
3. Else, the expression evaluates to the value of the expression on the right.

**or statement procedure:**
1. Evaluate the left statement.
2. If it evaluates to a True value x the expression evaluates to x.
3. Else, the expression evaluates to the value of the expression on the right.

This procedure functions using just Booleans, but strange things occur when you use other values instead.

For example:

```python
5 and 2 # >>> 2
5 or 2 # >>> 5
not 5 # >>> False
not 0 # >>> True
0 and False # >>> 0
```

For the `and` and `or` operators, numbers were returned rather than Booleans due to the procedure of evaluating these logical statements.

There is an order of operations for Booleans (not → and → or), but generally, use brackets to make your statements clearer.

You can use these expressions in functions as the return value. For example:

```python
def boolean_example():
    return is_ben or is_tao # This will return either True or False depending on the Boolean expression.
```

## Statements

A statement is executed to perform an action.

### Compound Statements

A compound statement is a statement that contains groups of other statements.

One example of which are conditional statements, which give your code a way to execute a different suite of statements based on whether conditions are met:

```python
if <condition>:
    this_may_be_executed(1)
elif <condition_2>:
    this_may_be_executed(2)
else:
    this_may_be_executed(3)
```

An if statement looks like the code above. The block indented after the if, elif, and else statements only get executed if the code directs it to.

For instance, if `<condition>` were True, then `this_may_be_executed(1)` is the only statement that gets evaluated, and the ones in the other blocks are skipped over.

If `<condition_2>` were True, then `this_may_be_executed(2)` is the only statement that gets evaluated.

If both `<condition>` and `<condition_2>` are False, then `this_may_be_executed(3)` is evaluated.

This means that the code does not get executed unless certain conditions are met, which is different from call expressions where every operand gets evaluated. (This property is important for some questions)

Additionally, this also allows for multiple return statements in functions because only that specific block gets executed, rather than every block.

```python
def returning_conditional(x):
    if x > 0:
        return "positive"
    if x < 0:
        return "negative"
    if x == 0:
        return "neutral"
```

## While Loop

A while loop in Python executes a block of code as long as a condition is true. This loop keeps on getting checked after each iteration.

One problem of a while loop is that an infinite loop can easily occur if you aren't careful.

```python
counter = 1
while counter < 5:
    print(counter)
    counter += 1
    '''
    > 1
    > 2
    > 3
    > 4
    '''
```

In the example above, 5 does not get printed because during that iteration, the counter variable is already 5, and the conditional 5 < 5 returns False.

A while loop is very useful if you do not know how many repeats of the code you need to do, while a for loop (explained on another page) is better if you know how many loops to do.

### Break Statement

If you ever want to prematurely leave a code block, you can use the break keyword.

```python
while True:
    print(1)
    break
# >>> 1
```

The above code would usually give an infinite loop, but the break statement prevents that from happening.

## For Loop

A for loop in Python executes a block of code for a set number of times. It provides a cleaner way to write while loops as long as they are iterating over some sort of sequence, for instance, the `range()` function.

The for loop syntax is as follows:

```python
for <name> in <expression>:
    <suite>
```

1. Evaluate `<expression>` — this must evaluate to an iterable value (strings, lists (more on this later), range()) etc
2. For each element in that `<expression>` (in order), bind `<name>` to the element in the current frame
3. Execute the suite, with `<name>` bound to a new value.

That might be slightly confusing for now, but just know that you can do everything a for loop can do with a while loop. So, if you see an example that uses a for loop, you can re-imagine it as a while loop, and it would still act the same.

### The range() function

The `range()` function is used quite often in conjunction with the for loop. It represents a sequence of integers. There are 3 arguments that `range()` takes, each of which will be explained, then demonstrated below:

1. If there is just one argument x, `range(x)` will start on 0, then keep increasing the number by 1 until x - 1 (0 <= i < x where i is the current value)
2. If there are two arguments x, y, `range(x, y)` will start on x, then keep increasing the number by 1 until y - 1.
3. If there are three arguments x, y, z, `range(x, y, z)` will start on x, then keep incrementing the number by z (this can be negative), and then end on y - z.

```python
for n in range(5): # equivalent to range(0, 5)
    print(n)
'''
> 0
> 1
> 2
> 3
> 4
'''

for n in range(1, 5):
    print(n)
'''
> 1
> 2
> 3
> 4
'''

for n in range(5, 0, -1):
    print(n)
'''
> 5
> 4
> 3
> 2
> 1
'''
```

As can be seen in the 3 examples above, the `range()` function works well with the for loop.

When `range()` is passed only 1 parameter in a for loop, it starts off at 0, then ends off at the integer before n. With 2 parameters, the for loop's value starts off at the first parameter's value, then ends off at the integer before the second parameter's value. The last parameter specifies the amount n should be changed by each loop, whether it be negative or a value other than 1. 