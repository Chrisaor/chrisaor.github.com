---
layout: post
title:  "[Basic] Sum of two large Numbers"
date:   2018-01-31 15:15:00 +0900
categories: Algorithm
---

Given two non-negative numbers X and Y. The task is calculate the sum of X and Y.

**Input:**

The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case contains two numbers X and Y.

**Output:**  

For each test case, if the number of digits in sum (X+Y) are equal to the number of digits in X, then print sum, else print X.

**Constraints:**
1<=T<=500  
1<=|Number length |<=100  

**Example:**  
**Input:**  
2  
25 23  
3 5  

**Output:**  
48  
8  


```python
def sum_of_two_large_numbers(n, m):
    digit_max = 0
    digit_sum = 0
    num_max = n
    sum_of_two = n+m
    while num_max != 0:
        num_max = num_max // 10
        digit_max += 1
    while sum_of_two != 0:
        sum_of_two = sum_of_two //10
        digit_sum += 1
    if digit_max == digit_sum:
        return n+m
    else:
        return n

t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    n = N[0]
    m = N[1]
    print(sum_of_two_large_numbers(n,m))
```


함정이 꽤 많았던 문제~~그냥 혼자 속았던 문제~~
