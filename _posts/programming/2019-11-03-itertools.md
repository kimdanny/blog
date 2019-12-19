---
title: "[Python] Iterables and Generators (3) - itertools"
categories:
  - programming
tags:
  - deep dive python
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
This post is continued from [Iterables and Generators (2) - Flattening a multi-dimensional list](https://kimdanny.github.io/programming/flattening-list/).  

In this post, I will introduce you to one of the most powerful python libraries, which is called an `itertools`.
Itertools supports various functions that create iterators for efficient looping.
This module implements a number of iterator building blocks, and you can see some similar implementations from some
functional programming languages, such as Haskell.


## 1. Combinatoric Generators
I feel that Combinatoric generators are frequently used and one of the most useful functions in this module.
You can easily see the difference between different functions and can know how to use those by following examples.
### 1-1. combinations()
@returns: all possible combinations, in sorted order, without any repeated elements

```python
import itertools as it
a = ['a', 'b', 'c' ,'d']

print(it.combinations(a, 2))    # <itertools.combinations object at 0x10dfe2ea8>
print(list(it.combinations(a, 2))) # [('a', 'b'), ('a', 'c'), ('a', 'd'), ('b', 'c'), ('b', 'd'), ('c', 'd')]

for (former, latter) in it.combinations(a, 2):
    print(former, latter)
```

Terminal output:
```bash
<itertools.combinations object at 0x107774ea8>
[('a', 'b'), ('a', 'c'), ('a', 'd'), ('b', 'c'), ('b', 'd'), ('c', 'd')]
a b
a c
a d
b c
b d
c d
```

### 1-2. permutations()
@returns: all possible orderings with no repeated elements

```python
import itertools as it
a = ['a', 'b', 'c' ,'d']

print(it.permutations(a, 2))    
print(list(it.permutations(a, 2))) 

for (former, latter) in it.permutations(a, 2):
    print(former, latter)
```

Terminal output: 
```bash
<itertools.permutations object at 0x10caa9570>
[('a', 'b'), ('a', 'c'), ('a', 'd'), ('b', 'a'), ('b', 'c'), ('b', 'd'), ('c', 'a'), ('c', 'b'), ('c', 'd'), ('d', 'a'), ('d', 'b'), ('d', 'c')]
a b
a c
a d
b a
b c
b d
c a
c b
c d
d a
d b
d c
```

### 1-3. product()
@returns: cartesian product equivalent to a nested for-loop
```python
import itertools as it
a = ['a', 'b', 'c' ,'d']
b = ['1', '2', '3' ,'4']

print("product: ", list(it.product(a, b)))

```

## 2. Iterators terminating on the shortest input sequence
Like I mentioned before, this module is heavily influence by functional programming languages (They are really awesome once you learn it, and my favorite one is Haskell!!).  
Functions you will see now are the things that you will see from Haskell.
One of reasons why my favorite language is python is because python does not confined to a certain programming paradigm, 
which means that you can choose whatever programming style you want (imperative, functional or object-oriented) that fits the best in the context.  

### 2-1. chain(*iterables)
Used for treating consecutive sequence as a single sequence.  
@param: *iterables
@description: returns chained iterables in a single sequence object.

```python
import itertools as it
l1 = ['a', 'b', 'c' ,'d']
l2 = ['1', '2', '3' ,'4']

s1 = "abc"
s2 = "def"

t1 = (1, 'a')
t2 = (2, 'b')

chained = it.chain(l1, l2, s1, s2, t1, t2)
print(type(chained))
print(chained)
print(list(chained))
```

Terminal output:
```bash
<class 'itertools.chain'>
<itertools.chain object at 0x104abbe10>
['a', 'b', 'c', 'd', '1', '2', '3', '4', 'a', 'b', 'c', 'd', 'e', 'f', 1, 'a', 2, 'b']
```

### 2-2. groupby()
You will see very similar functionality in `pandas` library, `sql` and of course Haskell too.
```
[k for k, g in groupby('AAAABBBCCDAABBB')] --> A B C D A B
[list(g) for k, g in groupby('AAAABBBCCD')] --> AAAA BBB CC D
```
Above is a possible use case of groupby() according to the python document.  
However, there is one important point that worth taking a look.

```python
import itertools as it
data = [{'module': 'Comp0147', 'score': "pass"},
        {'module': 'Comp0010', 'score': "fail"},
        {'module': 'Comp0009', 'score': "pass"},
        {'module': 'Comp0008', 'score': "fail"}]

grouped_data = it.groupby(data, key=lambda x: x['score'])
print(grouped_data)    # <itertools.groupby object at 0x107585570>
for key, member in grouped_data:
    print('{}: {}'.format(key, list(member)))
```
This should give you a grouped data that has two groups (pass and fail). However, python does not give you an intended output.
Terminal output:
```bash
<itertools.groupby object at 0x10dcb3570>
pass: [{'module': 'Comp0147', 'score': 'pass'}]
fail: [{'module': 'Comp0010', 'score': 'fail'}]
pass: [{'module': 'Comp0009', 'score': 'pass'}]
fail: [{'module': 'Comp0008', 'score': 'fail'}]
```
If you were using SQL, it would've worked properly, but in python, you have to first sort the keys before grouping it.

```python
import itertools as it
data = [{'module': 'Comp0147', 'score': "pass"},
        {'module': 'Comp0010', 'score': "fail"},
        {'module': 'Comp0009', 'score': "pass"},
        {'module': 'Comp0008', 'score': "fail"}]

sorted_data = sorted(data, key=lambda x: x['score'])
grouped_data = it.groupby(sorted_data, key=lambda x: x['score'])
print(grouped_data)    # <itertools.groupby object at 0x107585570>
for key, member in grouped_data:
    print('{}: {}'.format(key, list(member)))
```
Terminal output:
```bash
<itertools.groupby object at 0x10dcb3c50>
fail: [{'module': 'Comp0010', 'score': 'fail'}, {'module': 'Comp0008', 'score': 'fail'}]
pass: [{'module': 'Comp0147', 'score': 'pass'}, {'module': 'Comp0009', 'score': 'pass'}]
```



### 2-3. takewhile(predicate, iterable)
@params: predicate, iterable  
@description: evaluate `predicate(iterable element)` and returns iterable element that makes it true.  
@beware: This stops at the first instance of an element that the predicate returns false.


### 2-4. dropwhile(predicate, iterable)
@params: predicate, iterable  
@description: returns exactly the opposite to what takewhile() does.
```python
import itertools as it
testList = [-1, -4, 2, 4, 7, 3, 5, 6]
def pred(x):
    return x<3

takeWhileResult = it.takewhile(lambda x: x<3, testList)
print(list(takeWhileResult))      # [-1, -4, 2]

dropWhileResult = it.dropwhile(pred, testList)  # don't do pred(). This is functional programming.
print(list(dropWhileResult))      # [4, 7, 3, 5, 6]
```


### 2-5. built-in map(), zip(), filter()
To be clear, `map()`, `zip()` and `filter()` do not belong to itertool module, but rather it is a built-in function.
Before python3, there was `imap()`, `izip()` and `ifilter()` that returns an iterator rather than a list, 
but from python3 those functions have been removed and replaced by built-int map and zip that returns a list.  
However, I mention these functions in this article because these two functions are actually using `iter()` and `next()` method, 
and can be really synergistic once used together with itertools module.  

(Remember from [Iterables and Generators(1)-theory](https://kimdanny.github.io/programming/iterables-generator/), 
we learned that we can make iterator object by doing `iter(iterable)` and this iterator object has next() method by default).  
