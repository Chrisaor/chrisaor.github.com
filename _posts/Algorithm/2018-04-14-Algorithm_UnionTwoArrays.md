---
layout: post
title:  "[Basic] Union of two arrays"
date:   2018-04-14 11:55:00 +0900
categories: Algorithm
---

Given two arrays A and B, find union between these two array.  If there are repetitions, then only one occurrence of element should be printed in union.

**Input:**

The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case consist of three lines.  
The first line of each test case contains two space separated integers N and M, where N is the size of array A and M is the size of array B.  
The second line of each test case contains N space separated integers denoting elements of array A.  
The third line of each test case contains M space separated integers denoting elements of array B.

**Output:**

Correspoding to each test case, print in a new line, the union of the two arrays in sorted order.

**Constraints:**

1 ≤ T ≤ 30  
1 ≤ N, M ≤ 1000  
1 ≤ A[i], B[i] < 1000  

**Example:**

**Input:**
2  
5 3  
1 2 3 4 5  
1 2 3  
6 2  
85 25 1 32 54 6  
85 2

**Output:**  
1 2 3 4 5  
1 2 6 25 32 54 85  

```python
def union_two(arr1, arr2):
    for i in range(len(arr2)):
        if arr2[i] not in arr1:
            arr1.append(arr2[i])
    return ' '.join(map(str, sorted(arr1)))

t = int(input())
for i in range(t):
    N = input()
    arr1 = list(map(int, input().split()))
    arr2 = list(map(int, input().split()))
    print(union_two(arr1, arr2))
```
