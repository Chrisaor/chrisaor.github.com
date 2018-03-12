---
layout: post
title:  "[Basic] Longest Consecutive 1's"
date:   2018-03-12 11:25:00 +0900
categories: Algorithm
---

Given a number N, Your task is to find the  length of the longest consecutive 1's in its binary representation.

**Input:**  
The first line of input contains an integer T denoting the no of test cases. Then T test cases follow. Each test case contains an integer N.

**Output:**  
For each test case in a new line print the length of the longest consecutive 1's in N's binary representation.

**Constraints:**  
1<=T<100  
1<=N<=1000  

**Example:**  
**Input:**  
2  
14  
222  
**Output:**  
3   
4 


```python
def longest_consecutive(num):
    temp = list()
    cnt = 0
    while num > 0:
        if num % 2 == 1:
            cnt += 1
        else:
            temp.append(cnt)
            cnt = 0
        num = (num >> 1)
    temp.append(cnt)
    return max(temp)

t = int(input())
for i in range(t):
    num = int(input())
    print(longest_consecutive(num))
```