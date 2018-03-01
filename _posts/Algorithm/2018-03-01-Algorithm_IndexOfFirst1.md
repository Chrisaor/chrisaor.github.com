---
layout: post
title:  "[Basic] Index of first 1 in a sorted array of 0's and 1's"
date:   2018-03-01 11:15:00 +0900
categories: Algorithm
---

Given a sorted array consisting 0’s and 1’s. The task is to find the index of first ‘1’ in the sorted array.

**Input:**  
The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case consists of two lines. First line of each test case contains an Integer N denoting size of array and the second line contains N space separated 0 and 1.

**Output:**  
For each test case, in a new line print the index of first 1. If 1 is not present in the array then print “-1”.

**Constraints:**  
1<=T<=300  
1<=N<=105  
0<=A[i]<=1  

**Example:**  
**Input:**  
2  
10  
0 0 0 0 0 0 1 1 1 1  
4  
0 0 0 0  
**Output:**  
6  
-1  

```python
def index_of_1(arr):
    if 1 in arr:
        return arr.index(1)
    else:
        return -1

t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(index_of_1(arr))
```
