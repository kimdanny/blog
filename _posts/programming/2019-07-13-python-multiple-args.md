---
title: "[Python] How to deal with multiple parameters (1)"
categories:
  - programming
tags:
  - python
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
## Normally We've done this like...
Probably the majority of experienced python programmers would have encountered this wonderful syntax and
have already incorporated this method to their programming skills or style I guess.  
It is about how we put multiple arguments when making a function. What if we do not know how many parameters
we need for a specific function?  
In python, there is wonderful way to deal with this problem which is **\*args**.  
 <br/>

Let me take an example.  
Normally, we would write a function that adds two parameters as below.
```python
def addTwoParams(a, b):
  return a + b
```
<br/>

Now, your boss is telling you to fix the function to one that takes five parameters and sum them all up.
What are you going to do?  
```python
def addFiveParams(a, b, c, d, e):
  return a + b + c + d + e
```
If your coding style was as above, this blog post will help you survive from your vicious boss.

## \*args  
```python
def adder(*args):
    return sum(args)

print(adder(1, 2, 3, 4))	# 10
```
The parameter name "\*args" stores  unknown number of arguments to the variable named "args"
as a **tuple** data type.  

Below is the proof.  

```python
def showArgs(*args):
    return args

print(showArgs(1, 2, 3, 4))		#(1, 2, 3, 4)
```

If you've once learned the concept of Functional Programming, you will catch this programming concept quite quickly. This is because the syntax teaches a function *How to process* rather than imperatively order a function *What to do*.
