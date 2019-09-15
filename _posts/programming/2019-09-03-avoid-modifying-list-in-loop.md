---
title: "[Quick tip] Avoid modifying lists during iteration"
categories:
  - programming
tags:
  - deep dive python
  - algorithm
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---


Sorry! I'm still preparing for this post :)


## Bad Programming style that I had in the past
While programming, at a really high chance, we encounter a situation that we need to modify a list when looping through it.
For example, we often check some conditions of each element in the list while looping the list. Once the condition has met,
it is really easy for programmers to modify the list in whatever way we want to, **while looping**.
We can add or remove some elements in the list. Even worse, in some languages you can delete entire object of the element in the list while looping through it.
The following consequence is that **you mess up with an index arrangement**.
The byproduct you made as a result of that bad coding style costs a lot, especially when the loop was huge and complex.  
Hate Debugging!!  

Here's one of my past solution code from LeetCode problem:

```python

```

## Possible Solution / Alternative for this problem
Then what other ways can we do??
