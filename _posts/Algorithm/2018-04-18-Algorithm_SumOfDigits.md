---
layout: post
title:  "[Basic] Sum of digits"
date:   2018-04-18 12:10:00 +0900
categories: Algorithm
---

Given an integer N, find sum of all digits of N.

**Input:**

The first line contains an integer T, total number of test cases. Then following T lines contains an integer N.

**Output:**

Calculate the sum of digits of N.

**Constraints:**

1 ≤ T ≤ 30

1 ≤ N ≤ 1000

**Example:**

**Input**  
2  
123  
45  

**Output**  
6  
9

```python
def sum_of_digits(n):
    count = 0
    while n > 0:
        count += (n%10)
        n = n//10
    return count

t = int(input())
for i in range(t):
    n = int(input())
    print(sum_of_digits(n))
```
