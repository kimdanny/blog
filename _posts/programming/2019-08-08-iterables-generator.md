---
title: "[Python] Iterables and Generators (1) - theory"
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

## 1. Iterables / Iterator
### 1-1. Iterables
Iterable is literally an object that can be iterated (looped) over.
Iterable object has an '\__iter__()' method in its definition, which returns an iterator.
Every iterables can be looped over using a 'for' statement as below.
`list`, `tuple`, `dictionary`, `string` and `file` are iterables.

```python
# list
for x in [1, 2, 3]:
    print(x)
# tuple
for x in (1, 2, 3):
    print(x)
# dictionary
for key in {'one':1, 'two':2}:
    print(key)
# string
for char in "123":
    print(char)
# file
for line in open("sample.txt"):
    print(line)
```
### 1-2. Iterator
These operations (eg. for) that help iterables iterate through its elements are called iterator.
Thus, whenever we use 'for' loop, we are actually using an iterator, and they are an object with a '\__next()__' method.
In fact, the for statement is calling a 'next()' method at every step to get single elements from the container.
Most importantly, you can make an iterator object by calling an 'iter()' function. You can simply wrap an object with parentheses like `iter(obj)`.

Let's check how it works with this simple code.
```python
sampleList = [1, 2, 3]
# sampleList = range(1, 4)
iterList = iter(sampleList)
# Now, iterList is an iterator object that has a next() method
print(next(iterList))   # 1
print(next(iterList))   # 2
print(next(iterList))   # 3
# raise StopIteration when iteration is completed.
print(next(iterList))   # StopIteration
```

Note that you can also do `sampleList = range(1, 4)`. `range` belongs to class 'range', and it is one of the Sequence Types
(list, tuple, range). It's a whole new different story, so i will skip this content from here. But it is really good to know as well.
Knowing this deeply will help you understand why `for x in range(10)` works when you make loops.  

Plus, here's a really good implementation of Reverse Class by [python documentation](https://docs.python.org/3/tutorial/classes.html#iterators).
```python
# Rights reserved by python documentation.
class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
        return self.data[self.index]
```

## 2. Generators / yield
Generators are one of the useful ways to create iterators. 
If a container object’s \__iter__() method is implemented as a generator, it will return a generator object, 
supplying the \__iter__() and \__next__() methods.

### 2-1. Simple Generator
For simple Generators, you can use list comprehension. 
You just simply write list comprehension statement inside the parentheses.
Below shows that created generator object supplies the \__iter()__ and \__next()__ method.

```python
sampleGenerator = (x for x in range(1, 10))
print(next(sampleGenerator))    # 1
print(next(sampleGenerator))    # 2
print(next(sampleGenerator))    # 3
print(next(sampleGenerator))    # 4
for i in sampleGenerator:
    print("in for statement", i)
# in for statement 5
# in for statement 6
# in for statement 7
# in for statement 8
# in for statement 9

print(next(sampleGenerator))    # StopIteration
```
### 2-2. More complex Generator
You can make Generator in a different way for more complex operations.
When creating function-like Generators, you write code that are really similar to the normal function definition.
One little difference is that you write `yield` instead of `return`. Once you write `yield`, the function you've made will return a Generator.
Did similar thing as above.

```python
def myGenerator():
    testList = range(1, 10) # 1 - 10
    for x in testList:
        yield str(x) + " has been yielded"

# call generator
generator = myGenerator()
print(next(generator))
print(next(generator))
print(next(generator))
for x in generator:
    print(x, "in for loop")
print(next(generator))
```

Terminal output:
```
1 has been yielded
2 has been yielded
3 has been yielded
4 has been yielded in for loop
5 has been yielded in for loop
6 has been yielded in for loop
7 has been yielded in for loop
8 has been yielded in for loop
9 has been yielded in for loop
Traceback (most recent call last):
  File "xxxx.py", line xx, in <module>
    print(next(generator))
StopIteration
```

*Continue Reading this topic at  
[Iterables and Generators (2) - itertools and Flattening a multi-dimensional list](https://kimdanny.github.io/programming/itertools-flattening-list/)*
