---
layout: post
title:  "[Easy] Perfect Numbers"
date:   2018-04-06 11:55:00 +0900
categories: Algorithm
---

Given a number and check if a number is perfect or not. A number is said to be perfect if sum of all its factors excluding the number itself is equal to the number.

**Input:**  
First line consists of T test cases. Then T test cases follow .Each test case consists of a number N.

**Output:**  
Output in a single line 1 if a number is a perfect number else print 0.

**Constraints:**  
1<=T<=300   
1<=N<=10000  

**Example:**  
**Input:**  
2  
6  
21  
**Output:**  
1  
0  

```python
def perfect_numbers(n):
    sum = 0
    factors = list()
    for i in range(1,n+1):
        if n % i == 0:
            factors.append(i)
    factors = factors[:-1]
    for i in factors:
        sum += i
    if sum == n:
        return 1
    else:
        return 0

t = int(input())
for i in range(t):
    n = int(input())
    print(perfect_numbers(n))
```

