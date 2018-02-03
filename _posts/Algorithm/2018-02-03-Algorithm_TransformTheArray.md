---
layout: post
title:  "[Easy] Transform the array"
date:   2018-02-03 10:15:00 +0900
categories: Algorithm
---

Given an array containing integers, zero is considered an invalid number and rest all other numbers are valid. If two nearest valid numbers are equal then double the value of the first one and make the second number as 0. At last move all the valid numbers on the left.

**Input:**
The first line of input contains an integer **T** denoting the number of test cases. The first line of each test case consists of an integer **n**. The next line consists of **n** spaced integers.

**Output:**  
Print the resultant array.

**Constraints:**  
1<=T<=100  
1<=N<=10000  
1<=A[i]<=10000  

**Example:**  
**Input:**  
1  
12  
2 4 5 0 0 5 4 8 6 0 6 8  

**Output:**  
2 4 10 4 8 12 8 0 0 0 0 0  

```python
def transform_the_array(arr):
    rm_zero = list()
    trans_arr = list()
    zeros = 0
    for i in arr:
        if i == 0:
            zeros += 1
        else:
            rm_zero.append(i)

    for i in range(len(rm_zero)):
        if i == 0:
            trans_arr.append(rm_zero[0])
        elif rm_zero[i] == trans_arr[-1]:
            trans_arr[-1] = trans_arr[-1]*2
            zeros += 1
        else:
            trans_arr.append(rm_zero[i])

    answer = trans_arr + [0]*zeros
    return ' '.join(map(str, answer))

t = int(input())
for i in range(t):
    n = input()
    arr = list(map(int, input().split()))
    print(transform_the_array(arr))
```
