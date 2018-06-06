---
layout: post
title:  "[Easy] Convert array into Zig-ag fashion"
date:   2018-06-06 12:21:00 +0900
categories: Algorithm
---

Given an array of distinct elements, rearrange the elements of array in zig-zag fashion in O(n) time. The converted array should be in form a < b > c < d > e < f.The relative order of elements is same in the output i.e you have to iterate on the original array only.

**Input:**  
The first line of input contains an integer T denoting the number of test cases. The description of T test cases follows.
The first line of each test case contains a single integer N denoting the size of array.
The second line contains N space-separated integers A1, A2, ..., AN denoting the elements of the array.

**Output:**  
Print the array in Zig-Zag fashion.

**Constraints:**  
1 ≤ T ≤ 100  
1 ≤ N ≤ 100  
0 ≤A[i]<=10000  

**Example:**  
**Input:**  
2  
7  
4 3 7 8 6 2 1  
4  
1 4 3 2  

**Output:**  
3 7 4 8 2 6 1  
1 4 2 3

```python
def zig_zag_fahion(arr):
    temp = arr[0]
    for i in range(1,len(arr)):
        if i % 2 == 1:
            if arr[i] < temp:
                arr[i], arr[i-1] = arr[i-1], arr[i]
            temp = arr[i]
        else:
            if arr[i] > temp:
                arr[i], arr[i-1] = arr[i-1], arr[i]
            temp = arr[i]
    return ' '.join(map(str, arr))

t = int(input())
for i in range(t):
    n = input()
    arr = list(map(int, input().split()))
    print(zig_zag_fahion(arr))
```
