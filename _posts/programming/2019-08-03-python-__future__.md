---
title: "[Python] What is __future__ ?"
categories:
  - Programming
tags:
  - deep dive python
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
This post belongs to **deep dive python** tag. Deep dive python posts consist of 
advanced-level python syntax or skills that are not really obvious.
You can search *'deep dive python'* in the search bar above to explicitly look up the *posts by deep dive python tags*.  
  

```python
from __future__ import absolute_import, division, print_function,
				nested_scopes, generators, with_statement, unicode_literals
```
Have you seen this statement?

## As an aside...
What we will learn today is actually not a must-know item in python programming, but it is still good to know :) Don't worry this post is short.
When we learn programming or do our own side projects, we often get inspirations from other good sources.
We can either read someone's blog posts or look up one's Github page.  
While doing so, we realise that it is not really difficult to see some weird statement like "from \__future__ import blah blah" - no offense if it's not weird at all to you. At least I felt it is :)  

## Problems with Maintaining Compatibility
Like almost every programming languages does, they are always being developed to a better version and released to the public even before we get used to the 'older' version. My first "hello world" Python programme was written in the mid-2017, and if I remember correctly, teachers from my online course used both Python 2 and Python 3.4, while I was using Python 3.5. Two years has passed and currently, the latest python version is 3.7.4. You can check how rapidly python versions are changing from [official python website](https://www.python.org/downloads/). In fact, quite a lot of python 3 versions are compatible with each other. However, there are quite big difference between version 2 and version 3. One good example is "print". In python version 2, "print" was not a function but a keyword, therefore not needing an opening and closing parentheses.

```python
# In python version 2
print "hello world"

# From python version 3
print("hello world")
```
The big difference may well cause very annoying problems when working with team members who are using different version of python. Also, some systems are still running under python 2. To deal with this problem, python developers came up with the solution, which is \__future__ statement.

## from \__future__ import ...
With inclusion of future statement at the top of your code you can safely maintain the compatibility with other versions that others may use.  
From \__future__, you can import  
1. absolute_import
2. division, print_function
3. nested_scopes
4. generators
5. with_statement
6. unicode_literals
7. generator_stop
8. annotations

Moreover, note that **this is not a module**.  
From python documentation, it says as follows,  

"A future statement is a directive to the compiler that a particular module should be compiled using syntax or semantics that will be available in a specified future release of Python where the feature becomes standard."  

**Therefore, it is important to include future statement at the top of your file, because they change how your Python module is parsed.**  

For more details, check out this [python documentation](https://docs.python.org/3/reference/simple_stmts.html#future-statements)
