---
layout: post
title:  "[Easy] Maximum Difference"
date:   2018-06-06 12:20:00 +0900
categories: Algorithm
---

Given an array C[] of integers, find out the maximum difference between any two elements such that larger element appears after the smaller number in C[].  
Examples: If array is [2, 3, 10, 6, 4, 8, 1] then returned value should be 8 (Diff between 10 and 2). If array is [ 7, 9, 5, 6, 3, 2 ] then returned value should be 2 (Diff between 7 and 9).

**Input:**

The first line of input contains an integer T denoting the number of test cases.  
The first line of each test case is N,N is the size of array.  
The second line of each test case contains N input C[i].

**Output:**

Print the maximum difference between two element. Otherwise print -1

**Constraints:**

1 ≤ T ≤ 80  
2 ≤ N ≤ 100  
1 ≤ C[i] ≤ 500

**Example:**

**Input:**
2  
7  
2 3 10 6 4 8 1  
5  
1 2 90 10 110  

**Output:**  
8  
109  

```python
def maximum_difference(arr):
    diff = list()
    for i in range(len(arr)):
        for j in range(i+1, len(arr)):
            if arr[i] < arr[j]:
                diff.append(arr[j]-arr[i])
    if len(diff) == 0:
        return -1
    return max(diff)

t = int(input())
for i in range(t):
    n = input()
    arr = list(map(int, input().split()))
    print(maximum_difference(arr))
```
