---
title: "[Python] lambda Vs. def"
categories:
  - programming
tags:
  - deep dive python
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
Recently, while solving math problems for many hours I ended up getting sick of solving simple calculation at the end of one problem,
so I tried making a simple python automation programme for calculations.
It was mostly about putting multiple values into a function and finding the values that make the function return 0.
In the code, I was overusing lambda expression for every function I had to define.  
However, some of those lambda functions I used were inside a `for loop`, which makes the code smelly! (Needs to be Refactored!)  

This is the part of the code that smells a lot.
```python
nominator = [1, 2, 3, 4]
denominator = [1, 3]
for d in denominator:
    for u in nominator:
        frac = nominator/denominator
        func = lambda x: 3*(x**4) -29*(x**2) + 8
        
        if not func(frac):
            print(nominator, denominator)
print("no solution found")
```
Yes I'm telling you. I was pretty dumb at the moment :(

After figuring that out, 
I became quite curious about the performance between `lambda` and `def`,
and how they behaviour differently in the back side of the high level programming language, python.
What I meant by its performance is mainly as to time it takes to perform a certain task and user-usability (which one is easy to use).

### Measuring Time

I have recorded their time taken to complete one task for 100 iterations

```python
import time
import matplotlib.pyplot as plt
import numpy as np

lambda_function = lambda x: 3*(x**4) -29*(x**2) + 8

def def_function(x):
    return 3*(x**4) -29*(x**2) + 8

lambdaTime = []
defTime = []
for _ in range(100):
    start1 = time.time()
    lambda_function(123)
    end1 = time.time()
    lambdaTime.append(end1-start1)

    start2 = time.time()
    def_function(123)
    end2 = time.time()
    defTime.append(end2-start2)

#  Graph
fig, ax = plt.subplots()

x = np.array(lambdaTime)
y = np.array(defTime)

plt.scatter(np.arange(100), x, marker='o', label='lambda')
plt.scatter(np.arange(100), y, marker='^', label='def')

ax.set_xlabel("iteration")
ax.set_ylabel("def time & lambda time")

ax.set_xlim(0, 100)
ax.set_ylim(min(min(defTime, lambdaTime))-np.mean(defTime), max(max(defTime, lambdaTime))+np.mean(defTime))

ax.legend(
    loc='best',
    shadow=True,
    fancybox=True,
    borderpad=1
)

plt.show()
```
This Image shows that they have the same time-wise performance.  
![def_lambda](/images/def_lambda.png)  


### Bytecode disassembling

```python
import dis

lambda_function = lambda x: 3*(x**4) -29*(x**2) + 8

def def_function(x):
    return 3*(x**4) -29*(x**2) + 8

# bytecode disassemble
print("Lambda")
dis.dis(lambda_function)
print("def")
dis.dis(def_function)
```

Terminal output:
```bash
Lambda
6         0 LOAD_CONST               1 (3)
          2 LOAD_FAST                0 (x)
          4 LOAD_CONST               2 (4)
          6 BINARY_POWER
          8 BINARY_MULTIPLY
         10 LOAD_CONST               3 (29)
         12 LOAD_FAST                0 (x)
         14 LOAD_CONST               4 (2)
         16 BINARY_POWER
         18 BINARY_MULTIPLY
         20 BINARY_SUBTRACT
         22 LOAD_CONST               5 (8)
         24 BINARY_ADD
         26 RETURN_VALUE
def
9         0 LOAD_CONST               1 (3)
          2 LOAD_FAST                0 (x)
          4 LOAD_CONST               2 (4)
          6 BINARY_POWER
          8 BINARY_MULTIPLY
         10 LOAD_CONST               3 (29)
         12 LOAD_FAST                0 (x)
         14 LOAD_CONST               4 (2)
         16 BINARY_POWER
         18 BINARY_MULTIPLY
         20 BINARY_SUBTRACT
         22 LOAD_CONST               5 (8)
         24 BINARY_ADD
         26 RETURN_VALUE
```
They do generate the same bytecode!
