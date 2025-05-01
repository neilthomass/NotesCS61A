---
title: "Trees"
---

# Trees

A tree is an abstract data structure (basically not implemented by default in Python), which means we need to use data abstractions in order to implement this structure.

## What does a tree look like?

A tree has a root and a list of branches, where each branch is a tree itself.

A tree with zero branches (the white circles in the drawing above) is called a leaf. A tree also starts at the root, which in the drawing above, is the blue circle.

While this may not look like a tree, but rather roots of the tree itself, you could potentially think of the drawing as an upside down tree.

Also notice how when we look at the branches stemming from the root, we have two separate sub-trees? This means that recursion will be a common way to go about solving tree-related problems.

Each location in a tree can be called a node, and each node has a label which contains the value located within the node (can be any value). Nodes can be parents/children of each other, and the top node is the root node.

## Tree Data Abstraction

There are many possible data abstraction for trees, but the one that CS61A uses is the following:

| Abstraction | Description |
|-------------|-------------|
| `tree(label, branches = [])` | Returns a tree with root label and a list of branches |
| `label(tree)` | Returns the label of the tree |
| `branches(tree)` | Returns the branches of the tree (in a list) — each of which is a tree by itself |
| `is_leaf(tree)` | Returns True if the current node is a leaf node |

For example, a tree could be the following:

```python
t = tree(3, # Root Node
            # Branches:
            [tree(2, [tree(1)], # Left Branch
            tree(4))] # Right Branch
        )
```

The actual implementation of the tree is information that you do not need to know in order to use this data structure, so I will not write the implementation here. The documentation for these data abstractions says that branches(tree) returns a list, meaning that calling branches(tree)[0] is not a violation of data abstraction here.

## Tree Processing

As mentioned earlier, tree problems are solved recursively. Let's figure out why:

Each tree has the following:
- A label
- 0 or more branches, with each branch containing a tree itself.

Due to the fact that each branch contains a tree itself, this data type is recursive.

## Example Questions

### Counting Leaves

If we wanted to count the number of leaves in a tree, how would we do that? Obviously, the solution has to be recursive, so we need to start with the recursive mindset:

- What is our base case?
- What is our recursive case?

```python
def count_leaves(t):
    '''Returns the number of leaf nodes in tree t'''
    if is_leaf(t):
        return 1
    else:
        total_leaves = 0
        for b in branches(t):
            total_leaves += count_leaves(b)
        return total_leaves
```

In the code above, our base case happens when we hit a leaf, and the recursive call uses a for loop to go through all the branches in our tree, and then add the number of leaves to total_leaves, which is then returned.

We could also use the sum() function alongside a list comprehension to condense our code:

```python
def count_leaves(t):
    '''Returns the number of leaf nodes in tree t'''
    if is_leaf(t):
        return 1
    else:
        return sum([count_leaves(b) for b in branches(t)])
```

This works because the sum function can sum up the elements in an iterable.

### Creating Trees

A function that creates another tree based off of an existing tree is often recursive.

Let's recall how a tree is built:

```python
t = tree(3, # Root Node
            # Branches:
            [tree(2, [tree(1)], # Left Branch
            tree(4))] # Right Branch
        )
```

Let's try and make a function that doubles each node in a tree:

```python
def double(t):
    '''Returns a tree with same structure as t, but with each node doubled'''
    if is_leaf(t):
        return tree(label(t*2))
    else:
        return tree(label(t*2), [double(b) for b in branches(t)])
```

Notice how we have repeated code? We can actually shorten the code to one line because of that (the base case here doesn't need to be made explicit, will explain why later)

```python
def double(t):
    '''Returns a tree with same structure as t, but with each node doubled'''
    return tree(label(t*2), [double(b) for b in branches(t)])
```

In the final code, we don't need a base case/recursive case structure that you would traditionally see for recursive questions, but even so, it's a good idea to think of it that way before condensing it down.

The code above works because in the case of a leaf node, [] gets passed into branches within the tree constructor, which by default takes in an empty list if there are no branches. Thus, in the case of a leaf node, all that gets returned there is equivalent of saying tree(label(t*2)).

### Printing Trees

```python
def print_tree(t, indent = 0):
    '''Prints the labels of t with depth based indent'''
    print(" " * indent + str(label(t)))
    for b in branches(t):
        print_tree(tree(b), indent + 1)
```

Look at the example above and see how it works. indent + 1 is important as it will indent the tree to the correct level. 