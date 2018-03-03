---
layout: post
title:  "[Basic] Factorial"
date:   2018-03-03 11:20:00 +0900
categories: Algorithm
---



Calculate the factorial for a given number.

**Input:**  
The first line contains an integer 'T' denoting the total number of test cases. In each test cases, it contains an integer 'N'.


**Output:**  
In each seperate line output the answer to the problem.
 

**Constraints:**  
1<=T<=19  
0<=N<=18  


**Example:**  
**Input:**  
1  
1  

**Output:**  
1



```python
def factorial(n):
    fac = 1
    for i in range(1,n+1):
        fac = fac*i
    return fac


t = int(input())
for i in range(t):
    n = int(input())
    print(factorial(n))
```
