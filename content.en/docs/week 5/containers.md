---
title: "Containers"
---

# Containers

## Lists

A list is a container that can hold a sequence of information (usually related information).

Lists can hold any Python values (not the same behaviour in every language), including other lists/objects etc.

```python
empty = [] # empty list

B = ["Ben", "Box", "", "Bufy"] # Strings
numbers = [2, 5, 7] # Integers
floats = [2.0, 3.5, 7.5] # Floats
nested = [[2, 3], 3, 4] # Storing a list inside a list
mixed = ["Hi", 2, 3.2] # Different data types inside a list
```

### List Length

The globally defined `len()` function in Python allows you to find the length of an array, and can be called by simply passing in the array as an argument.

```python
lst = [2, 3, 4, 5]
>>> len(lst)
4
```

This could be useful if you wanted to count the number of elements in an array to calculate the average for instance.

While you could store the length of the list in a variable, it's usually not a good idea because once the list is updated, the variable storing the length will not be updated. As a result, it's not a bad idea to always call `len()` when you need to find the length unless you have a specific use for storing an outdated list length.

```python
length_of_list = len(lst) # 4
lst = lst + [6] # [2, 3, 4, 5, 6], more on list concatenation later
>>> len(lst)
5
>>> length_of_list
4
```

### Indexing (Accessing List Elements)

Now that we have a list, well, cool! But how do we access individual elements?

We can use indexing for that. Each element in the list has its own index (indexes start on 0), and can be accessed by putting the index of the element in question in a square bracket.

```python
lst = [2, 3, 4, 5]
# Index:
#      0  1  2  3

>>> lst[0]
2
>>> lst[3]
5

to_get = 2
>>> lst[to_get]
4

>>> lst[4]
# Will throw an error
```

In Python, negative indices are also possible. Calling a negative index on a list will return the elements starting from the back.

```python
>>> lst[-1]
5
>>> lst[-2]
4
>>> lst[-5]
# Will throw an error
```

Notice how the first element from the back is -1 rather than 0? A somewhat easy way to imagine why that's the case is to imagine negative indices being shorthand for `len(lst) - <index>` — so for example, `lst[-1]` would be `len(lst)` — equal to 4 — then minus 1, which would give 3, so it's effectively the same thing as calling `lst[3]`.

### List Concatenation

You can add two lists together by using the + operator, or the add function, meaning that you can change the information stored in a list.

For now, you will only be able to add to a list with this operator (the - operator does not work on lists), but in the next page, you will see a method of how to take a subset of a list, which essentially does the same thing as removing elements in a slightly safer manner.

```python
list1 = [1, 2, 3, 4]
list2 = [5]

>>> list1 + list2
[1, 2, 3, 4, 5]

>>> add(list1, [6])
[1, 2, 3, 4, 6]
```

For those used to other languages, you may see `.append` and `.push` used, but those actually change the values of the array, which can easily be circumvented by just adding two lists together and assigning that to a new variable. That way you have access to old versions of your variables, making your code slightly easier to read/debug.

### List Repetition

You can also use the * operator and the mul function with lists — however you can only multiply (vanilla Python) lists with an integer, which would repeat the elements already in the list. For example:

```python
lst = [1, 2, 3]
lst3 = lst * 3
>>> lst3
[1, 2, 3, 1, 2, 3, 1, 2, 3]
```

### Nested Lists

As briefly mentioned earlier, you can also put lists inside of lists in Python. For example:

```python
lst = [[1, 2], [3, 4, "Hi"], []]
```

If you think about this in the larger picture, lst itself only has 3 elements — 3 separate lists (with the contents inside them being irrelevant until they need to be accessed). However, once you access one of the lists, you then get another list returned (similarly to how higher-order functions worked when they returned other functions), which can then be indexed again to get a specific value.

```python
>>> lst[0]
[1, 2]

>>> lst[0][1]
2
```

So, knowing that information, what is the length of lst and the length of lst[2]?

### Containment

You can use the `in` operator to see whether a value is in a container. This operator, like `==`, `<`, `>`, etc. returns a Boolean value in this context.

```python
lst = [1, 2, 3]

>>> 1 in lst
True
>>> 2 in lst
True
>>> "2" in lst
False
>>> not (3 in lst)
False
>>> 12 in lst
False
>>> [1] in lst
False # This one is false because none of the elements in lst are lists themselves
```

### For Statements

You can check the Control page to see the fundamental information on for loops and the range() function.

For lists, the range() function does not always need to be used.

#### For-in Loops

If you loop through a list, you can iterate through it without using range():

```python
lst = [1, 2, 3]
for elem in lst: # In every iteration, it binds a value from `lst` to `elem`
    print(elem)

# Console output:
# 1
# 2
# 3
```

If you need to access deeper than 1 level, you can use a nested for-in loop.

#### Sequence Unpacking Example

```python
lst = [[1, 2], [3, 4]]
for a, b in lst:
    print(a + b)

# Console output:
# 3
# 7
```

Each name is bound to a value in this case (just like in multiple assignment)

### List Comprehensions

List comprehension in Python is a very elegant way to create a new list by mapping an existing list's values to a new list.

For example, if you wanted to add 1 to every integer in a list, you could use list comprehension in the following manner:

```python
lst = [1, 2, 3, 4, 5]
lst_plus_1 = [x + 1 for x in lst] # [2, 3, 4, 5, 6]
```

And if you wanted to have a list that only contained odd numbers + 1, you could do the following:

```python
odd_lst_plus_1 = [x + 1 for x in lst if x % 2 == 0] # [2, 4, 6]
```

The if statement acts as a filter — it only puts elements in the new array if it passes a condition.

We can generalize this structure to the following:

```python
[<map expression> for <name> in <iterator> if <condition>]
```

The execution procedure for this (in an environment diagram) would be to:

1. Add a new frame with its current frame as its parent
2. Create an empty result list
3. For each element in iterator, bind it to name
4. If condition returns a true value, then add the value of map expression to the result list in step 2.

## Strings vs Lists

Strings and lists have very similar behaviour (although naturally they do have some differences). They both can act as an iterator (so they can be used as an iterator in a for loop.

Similarities:

```python
string = "ben"
len(string) # 3
string[2] # n
string + " baron" # ben baron
```

However, they differ in these aspects:

```python
lst = [1]
string = "B"

string[0] == string # True
lst[0] == lst # False (This is because lst[0] is equivalent to 1 as opposed to [1])
```

Additionally, the in operator will match substrings inside a string, but will not do so for a list.

```python
lst = ["bent", "tao"]
string = "bent"

"ben" in lst # False
"ben" in string # True
``` 