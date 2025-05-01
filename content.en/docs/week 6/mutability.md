---
title: "Mutability"
---

# Mutability

## Objects

An object is a bundle of data and behaviour, with each type of object called a class.

Every value in Python is an object.

All objects have attributes, and objects often have associated methods.

### Example (Strings)

A string is an object — try running `type("")` in Python console and seeing what it outputs.

Strings have attributes (for example the data inside it) and also has methods such as `string.upper()`.

## List Mutation

Lists (like dictionaries) are object types that can be mutated. This means that the value bound to the name can change without reassignment. This can potentially be very useful, but at times, can also be very dangerous due to the way Python works with lists.

Two methods to mutate lists:

```python
s = [1, 2]
s.append(3)
s # [1, 2, 3]

l = [1, 2]
l.extend([4])
l # [1, 2, 4]
l.extend([3, 2])
l # [1, 2, 4, 3, 2]
```

While these look identical to `s = s + [3]`, they behave very differently. This is because the + operator makes a shallow copy of the list rather than directly mutating the list itself. This makes a difference when two names are pointing to the same list object.

Try running the code below on PythonTutor:

```python
lst1 = [1, 2, 3]
lst2 = lst1
lst3 = lst2
# Notice how after the above 3 statements are executed, all 3 names point to the same list object
lst1 = lst1 + [3, 2] # Makes a copy of the list, does not mutate the original
lst2.append(4)
print(lst1) # [1, 2, 3, 3, 2]
print(lst2, lst3) # [1, 2, 3, 4] [1, 2, 3, 4]
```

Notice how even after I updated only lst2, lst3 got updated alongside it. This is one of the dangers of mutation (and is why I generally prefer to use a method that doesn't mutate anything: it makes debugging a lot easier when variables aren't changing without explicitly being assigned to change.)

### Method Mutation (Removing)

- `pop()` returns and removes the last element in the list
- `remove()` removes the first element equal to the argument

```python
s = [1, 2]
s.pop()
s # [1]

l = [1, 2, 4]
l.remove(2)
l # [1, 4]
```

### Mutation with Slicing

You can also mutate lists with list slicing or indexing:

```python
lst = [1, 2, 3, 4, 5]
lst[0] = "Hi"
lst # ["Hi', 2, 3, 4, 5]

lst[1:3] = [25, 6]
lst # ["Hi", 25, 6, 4, 5]

lst[1:1] = [3, 2, 1] # Inserting Elements
lst # ["Hi", 3, 2, 1, 25, 6, 4, 5]

lst[:5] = [] # Deleting Elements
lst # [6, 4, 5]

lst[len(lst):] = [3] # Appending elements
lst # [6, 4, 5, 3]
```

All the statements above mutate the list that we have.

### Python Weirdness

In Python, doing `lst = lst + [1]` is different to `lst += [1]`.

`+=` actually acts as `.extend([...])` rather than making a copy. As a result, doing `lst += [1]` mutates the original list, while `lst = lst + [1]` does not mutate the original. Weird behaviour, sure, but this is very important to know.

## Tuples

Tuples are a data type that are immutable. This means that once the tuple is created and assigned to a name, you can be confident that the tuple will always remain the same unless the name is bound to a different value.

Tuples are also a sequence, which means you can view them as essentially immutable lists.

```python
empty = () # empty tuple

coordinates = (32, 21) # tuple with multiple elements

temperature = (30,) # tuple with 1 element (the , at the end is necessary for python to parse it as a tuple)
```

The read-only list operators work with tuples:

```python
t1 = (1, 2)
t2 = (3, 4)

t3 = t1 + t2 
t3 # (1, 2, 3, 4)

1 in t2 # False
1 in t3 # True

t4 = t3[1:] # (2, 3, 4)
```

## Identity vs Equality

Identity vs Equality essentially boils down to `is` vs `==`. The `is` Boolean operator checks whether the objects are the same (for example in the case of a list, whether the two names are pointing to the same list), while the `==` operator just checks whether the values are the same.

```python
lst1 = [1, 2]
lst2 = lst1
lst3 = [1, 2]

lst1 is lst2 # True
lst1 is lst3 # False
lst1 == lst3 # True
```

## 'Mutable' Functions (A function with changing state)

You can have a function that changes state by having a mutable object in the parent frame:

```python
def bank(initial_fund):
    bank_account = [initial_fund]
    def withdraw(amount):
        if bank_account[0] < amount:
            return "NO!"
        else:
            bank_account[0] = bank_account[0] - amount
            return bank_account
    return withdraw

withdrawer = bank(100)

withdrawer(25) # 75
withdrawer(25) # 50
``` 