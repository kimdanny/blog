---
title: "[Python] Iterables and Generators (2) - Flattening a multi-dimensional list"
categories:
  - programming
tags:
  - deep dive python
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
This post belongs to **deep dive python** tag. Deep dive python posts consist of 
advanced-level python syntax or skills that are not really obvious.
You can search *'deep dive python'* in the search bar above to explicitly look up the *posts by deep dive python tags*.  

This post is continued from [Iterables and Generators(1)-theory](https://kimdanny.github.io/programming/iterables-generator/)

Prior Knowledge of `hasattr()` and `list.extend(obj)` is encouraged.  

Basically, our code will check if one element is iterable object or not.
If one element has an attribute that is called '\__iter__', it means the element is the list inside the given list 
which we want to `extend` it.  
Of course you can directly check its type by `type(x) == list`. However, I just wanted to show you guys that it has the '\__iter__' attribute.

### Flattening a 2-Dimensional List

```python
twoDList = [4, 3, 7, [1, 2, 3], 35, 9]

flattened = []

for x in twoDList:
    if hasattr(x, '__iter__'):
        flattened.extend(x)
    else:
        flattened.append(x)
```

### Flattening a n-Dimensional List
Can recursively call the function.

```python
nDList = [4, 3, 7, [1, 2, [10, 20, 30, [5]], 3], 35, 9]

def flatten(data):
    flattend = []
    for x in data:
        if hasattr(x, '__iter__'):
            flattend.extend(flatten(x))
        else:
            flattend.append(x)
    return flattend
```


*Continue Reading this topic at  
[Iterables and Generators (3) - itertools](https://kimdanny.github.io/programming/itertools/)*
