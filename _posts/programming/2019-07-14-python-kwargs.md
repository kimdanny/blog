---
title: "[Python] How to deal with multiple parameters (2)"
categories:
  - programming
tags:
  - pyFun
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

---------------------------
This post belongs to **pyFun** tag. pyFun posts consist of 
advanced level python syntaxes or skills that are not really obvious
You can search *'pyFun'* in the search bar above to explicitly look up the *posts by pyFun tags*.
---------------------------

*This post is continued from  
[How to deal with multiple parameters (1)](https://kimdanny.github.io/programming/python-multiple-args/)*  

## Quick Recap
Last time we learned how to get multiple (unknown number of) arguments when building a function.  
By using "\*args" as a parameter, we could store multiple arguments in the variable name "args" as a **tuple** form.  
This time we will learn how can multiple (unknown number of) **dictionary** arguments be passed to one function.  

## \**kwargs
First, let's check how it works by simply printing out passed arguments.
```python
def showDict(**kwargs):
    print(kwargs)

showDict(a=80, b=70, c=20, d=100)  # {'a': 80, 'b': 70, 'c': 20, 'd': 100}
```
As you could check above, the parameter with two asterisk \**kwargs accepts multiple arguments that has a form of
'keyword=value' and stores those in a dictionary variable.  

Now, if you utilise .keys() function, this can bring about really synergistic effect like below.

```python
def good_func(**kwargs):
    for key in kwargs.keys():
        print(key, ":", kwargs[key])

good_func(algorithm = 89, discreteMath = 98, statistics = 70, programming = 95)
# output:
# algorithm : 89
# discreteMath : 98
# statistics : 70
# programming : 95
```

## \*args + \**kwargs
To be more synergistic, you can even use both \*args and \**kwargs as below.  

```python
def amazing_func2(*args, **kwargs):
    for key in kwargs.keys():
        print(key, ":", kwargs[key]*args[0])

amazing_func2(0.5, algorithm = 89, discreteMath = 98, statistics = 70, programming = 95)
# output:
# algorithm : 44.5
# discreteMath : 49.0
# statistics : 35.0
# programming : 47.5
```
Note that you have to specify which index you will use in args tuple by doing args[0]  

These techniques will not only help you write a better programme but also help you have a better understanding on plenty of documentations.
