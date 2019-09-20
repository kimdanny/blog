---
title: "[Quick tip] Avoid modifying lists during iteration"
categories:
  - Programming
tags:
  - deep dive python
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
## 1. Bad Programming style that I had in the past
While programming, at a really high chance, we encounter a situation that we need to modify a list.
For example, we often check a certain condition(s) of each element in the list while looping through the list. 
Once the condition has met, it is really easy for programmers to modify the list in whatever way they want to, **inside the looping statement**.
We can add or remove some elements in the list. Even worse, in some languages you can delete the entire object of the element in the list while looping through it.
The following consequence is that **you mess around with an index arrangement**.
The byproduct you made as a result of the bad coding style costs a lot, especially when the loop is huge and does complex operations.  
Hate Debugging!! innit!  

Let me show you one of my past solution from LeetCode problem:  
(It was about sorting array by parity. Given an array A of non-negative integers, 
i had to return an array consisting of all the even elements of A, followed by all the odd elements of A)

```python
def sortArrayByParity(A):
    for i in range(len(A)):
        if A[i]%2==0:
            A.insert(0, A[i])
            # can do either del or remove
            del A[i+1]
            # A.remove(A[i+1])
    return A
```
Of course this works, but this coding style is quite dangerous. Do you see what extra manipulation i had to do because of the dangerous behaviour?
I had to delete (i+1)th element instead of (i)th element. This can be seen as an easy manipulation, but it's only because this problem was simple.


### 1-1. Possible error 1 - function doesn't perform in an intended way
This example shows clearly that you can easily mess up your list and see an unexpected result after applying function you've defined.
```python
def badFuntion(A):
    """
    :param A: List[Int]
    remove an item if it's divisible by 3
    """
    for index, item in enumerate(A):
        print("index: {}, item: {}".format(index, item))
        if item % 3 == 0:
            # can do either of these options
            # A.remove(item)
            del A[index]

testcase = [-3, 0, 1, 2, 3, 3, 4, 6]
print("original list:", testcase)
print("expected list: [1, 2, 4]")
badFuntion(testcase)
print("messed up list:", testcase)
```
Terminal output:
```
original list: [-3, 0, 1, 2, 3, 3, 4, 6]
expected list: [1, 2, 4]
index: 0, item: -3
index: 1, item: 1
index: 2, item: 2
index: 3, item: 3
index: 4, item: 4
index: 5, item: 6
messed up list: [0, 1, 2, 3, 4]
```
Where have the index 6 and 7 gone?  
Why the list still contains 0 and 3, which are divisible by three?

### 1-2. Possible error 2 - Index Out of Range

```python
testcase = [1, 2, 3, 4, 5, 6, 7]
for i in range(len(testcase)):
    print(testcase[i], end=' ')
    if i==2:
        del testcase[i]
```
Terminal output:
```
1 2 3 5 6 7 Traceback (most recent call last):
  File "xxxx.py", line xxx, in <module>
    print(testcase[i], end=' ')
IndexError: list index out of range
```

## 2. Problem Analysis / Possible Solutions

### 2-1. Problem Analysis - how many times the 'len(testcase)' is called?
The reason for the above two possible errors are obvious. No need to elaborate. However, there is one thing that is not really obvious.
When we are using 'range(len(testcase))' inside the for statement, do you know how many times it is called?
I know, this question is really easy as well. Because it is only called once when you start the loop, it causes those errors.
If the for loop checks the length of its input data every time it finishes one iteration, why would've those errors be occurred from the beginning?  

But can you prove it?  
Not-really-obvious part starts from here (at least i think it is).  

You can make function for checking function call and put that into the loop.
```python
def check(data):
    length = len(data)
    print("called,"+"length:"+str(length))
    return range(len(data))

testcase = [1, 2, 3, 4, 5, 6, 7]
for i in check(testcase):
    print(testcase[i], end=' ')
    if i==2:
        del testcase[i]
```
Terminal output:
```
called,length:7
1 2 3 5 6 7 Traceback (most recent call last):
  File "xxxx.py", line xxx, in <module>
    print(testcase[i], end=' ')
IndexError: list index out of range
```
Now you can see the check function is called only once upon starting the loop.

### 2-2. Possible Solutions
In most case, there are plenty of other solutions that does not use modification of list while looping, so just try to avoid doing it if possible.
The solution you came up with might take up more memory or a bit slower, but i believe it is still worth avoiding modifying a list during iteration for most cases.  

Possible solutions are:
1. List Comprehensions
2. Try using while loop instead of for loop. (while-loop can be a low-level approach for a for-loop, so you can get a direct control of indices)
3. Create a new filtered list by appending some elements you need rather than deleting it
4. Import itertools (only for python) - check [Iterables and Generators(2)](https://kimdanny.github.io/programming/itertools-flattening-list/)
5. Get a separate list that contains indices that has to be removed
6. Use a copy of a list and iterate through it and modify the original

You can see much better code for 'sortByParity' from [my github repo/leetcode/sort-array-by-parity](https://github.com/kimdanny/leet-code/blob/master/Sort_array_by_parity.py)  

Disclaimer: This is my personal opinion from my past experience.  
Denies judgemental opinions but welcomes criticism!!
