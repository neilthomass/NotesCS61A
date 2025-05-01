---
title: "Recursive Data"
---

# Recursive Data

## Linked Lists

Python lists are implemented in a way that makes inserting and deleting from the front of a list very inefficient. This is because Python implemented a list such that the first element of the list is always at the same memory location and elements in a list are located right next to each other, so when something is inserted to the start, everything gets pushed forward by one space in memory to make space for an element.

If you ever need to constantly add to the front of a list, a Python list may not be a good idea. That's where a Linked List can come in handy.

Imagine building a list where each element can be anywhere in memory, and all that needs to happen is that one element knows where the next is:

I drew each node (or element) in a different location on the page just to illustrate that these blocks could be anywhere on the page and do not need to be directly next to each other like in a list. Each instance/element/node of our linked list just needs a value inside it, and an arrow pointing to the next linked list object. If we think of it this way, we can construct our own Linked List class.

```python
class Link:
    empty = () # How we will refer to an empty linked list
    def __init__(self, first, rest = empty):
        self.first = first
        self.rest = rest
```

Using this class, we can then define our first linked list!

```python
my_link = Link(1, Link(2), Link(3)) # Notice how similar it is to the tree data abstraction you used earlier
```

In the example above, 1 is what gets stored in self.first, while Link(2, Link(3)) is what gets stored in self.rest, meaning that we are able to continue accessing our linked list.

However, one thing is pretty clear: it's impossible to directly access our Link(3) instance without first going through our whole list, following the list of pointers right until the end. This is one of the downsides of using a linked list in comparison to a python list - there are both benefits and drawbacks.

## Link Class

A fancier version of the linked list that CS61A uses is the following:

```python
class Link:
    """A linked list."""
    empty = ()

    def __init__(self, first, rest=empty):
        assert rest is Link.empty or isinstance(rest, Link) # makes sure that the rest is either a linked list or emtpy
        self.first = first
        self.rest = rest

    def __repr__(self):
        if self.rest:
            rest_repr = ', ' + repr(self.rest)
        else:
            rest_repr = ''
        return 'Link(' + repr(self.first) + rest_repr + ')' # Returns the link class

    def __str__(self):
        string = '<'
        while self.rest is not Link.empty:
            string += str(self.first) + ' ' # Recursion!!!
            self = self.rest
        return string + str(self.first) + '>'
```

Using this structure can make us certain that a Link instance is only able to point to either another instance of Link or Link.empty.

## Exercise: Creating a Range in a Linked List format

Let's try to create something equivalent to [x for x in range(3, 6)] but in a linked list rather than a pythonic list.

```python
def range_link(start, end):
    if start >= end:
        return Link.empty # base case
    else:
        return Link(start, range_link(start + 1, end)) # Recursive case to link the lists together
```

## Exercise: Making a map function for a Linked List

### Making a copy

```python
def map_ll(fn, ll):
    if ll is Link.empty:
        return Link.empty
    return Link(fn(ll.first), map_ll(fn, ll.rest))
```

### Mutating the list given

We can mutate the list because it's stored in a class, and as you know from making your classes, it is possible to mutate your instance variables (which is what appears here) which is why these solutions may feel weird initially.

```python
def map_ll(fn, ll):
    while ll is not Link.empty:
        ll.first = fn(ll.first) # direct mutation of the linked list
        ll = ll.rest # move forward in the linked list
```

## Exercise: Filter

```python
def filter_ll(fn, ll):
    if ll is Link.empty:
        return Link.empty
    elif fn(ll.first):
        return Link(ll.first, filter_ll(fn, ll.rest))
    else:
        return filter_ll(fn, ll.rest) # moves onto the next case
```

## General Mutation caveats

You can sometimes have infinite linked lists if you set the rest instance variable to point to itself.

```python
my_ll = Link(3)
my_ll.rest = my_ll
```

What this will do is point the linked list to itself, so when you try to do a recursive solution on it, it will never find a case where my_ll.rest is Link.empty.

## Inserting to the Linked List

If you look at the image above, it outlines the process of inserting into the middle of a linked list in a visual manner.

```python
def insert_ll(ll, val, position):
    if ll is Link.empty:
        assert False, "Position not found in Linked List"

    if position == 0:
        new_link = Link(val)
        new_link.rest = ll.rest
        ll.rest = new_link
    else:
        insert_ll(ll.rest, val, position - 1)
```

## Tree Class

Remember the tree we made as a data abstraction? One issue that we had with the original implementation is that we couldn't mutate our tree label. However, because of the properties of classes, we are able to do that with a new implementation of our tree:

```python
class Tree:
    def __init__(self, label, branches=[]):
        self.label = label
        self.branches = list(branches)

    def is_leaf(self):
        return not self.branches
```

This will let us directly change self.label.

CS61A uses this:

```python
class Tree:
    def __init__(self, label, branches=[]):
        self.label = label
        for branch in branches:
            assert isinstance(branch, Tree)
        self.branches = list(branches)

    def is_leaf(self):
        return not self.branches

    def __repr__(self):
        if self.branches:
            branch_str = ', ' + repr(self.branches)
        else:
            branch_str = ''
        return 'Tree({0}{1})'.format(self.label, branch_str)

    def __str__(self):
        return '\n'.join(self.indented())

    def indented(self):
        lines = []
        for b in self.branches:
            for line in b.indented():
                lines.append('  ' + line)
        return [str(self.label)] + lines
```

## Tree Mutation

### Exercise: Double every label of a tree

Let's try to double every label of a tree by mutating every value:

```python
def double(t):
    t.label = t.label * 2
    for b in t.branches:
        double(b)
```

### Exercise: Prune Trees

Removing subtrees from a tree is called pruning. When you do this, always prune before doing the recursive call.

```python
def prune(t, n):
    """Prune all subtrees with label n"""
    t.branches = [b for b in t.branches if b.label != n]
    for b in t.branches:
        prune(b, n)
``` 