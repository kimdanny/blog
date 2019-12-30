---
title: "[Java] Variable Arguments in java?!"
categories:
  - programming
tags:
  - java
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
From these posts, you've learned that python supports getting unfixed number of multiple parameters in functions.  
[Dealing with multiple parameters in python - *args (1)](https://kimdanny.github.io/programming/python-multiple-args/)
[Dealing with multiple parameters in python - **kwargs (2)](https://kimdanny.github.io/programming/python-kwargs/)

Surprisingly, I recently found out that Java (from JDK 5) also supports the same feature!


```java
class VarArgs
{ 
    // A method that takes variable number of integer 
    // arguments. 
    static void test(int... num) 
    { 
        System.out.println("Number of arguments: " + num.length); 
   
        for (int elem : num){
            System.out.print(elem + " " + "\n"); 
        } 
    } 
  
    public static void main(String args[]) { 
        
        test(1);         
        test(1, 2, 3, 4);  
        test();             
    } 
} 

```


You can find variable arguments example from java `ProcessBuilder`.
```java
public ProcessBuilder(String... command) 
```


