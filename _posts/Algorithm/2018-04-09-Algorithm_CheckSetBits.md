---
layout: post
title:  "[Basic] Check set bits"
date:   2018-04-09 11:55:00 +0900
categories: Algorithm
---

Given a number N. Write a program to check whether every bit in the binary representation of the given number is set or not.

**Input:**  
First line of input contains a single integer T which denotes the number of test cases. T test cases follows. First line of each test case contains a single integer N which denotes the number to be checked.

**Output:**  
For each test case, print "YES" without quotes if all bits of N are set otherwise print "NO".

**Constraints:**  
1<=T<=1000  
0<=N<=1000  

**Example:**  
**Input:**  
3  
7  
14  
4  
**Output:**  
YES  
NO  
NO  

```python
def check_set_bits(n):
    for i in bin(n)[2:]:
        if i == '0':
            return 'NO'
    return 'YES'

t = int(input())
for i in range(t):
    N = input()
    n = int(input())
    print(check_set_bits(n))
```
