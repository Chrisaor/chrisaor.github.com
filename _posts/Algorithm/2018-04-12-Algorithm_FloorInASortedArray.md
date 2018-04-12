---
layout: post
title:  "[Easy] Floor in a Sorted Array"
date:   2018-04-12 11:55:00 +0900
categories: Algorithm
---

Given a sorted array having no duplicates, arr[] and a value, x, find floor of x in given array.  
Floor of x is the largest element in arr[] such that the element is smaller than or equal to x. If floor exists, then return **index of** it, else return -1.

**Examples:**

```
Input : arr[] = {1, 2, 8, 10, 11, 12, 19}, x = 5
Output : 1 
1 is index of 2. 2 is the largest element in arr[]
smaller than 5.

Input : arr[] = {1, 2, 8, 10, 11, 12, 19}, x = 20
Output : 6
19 is the largest element in arr[] smaller than 20.

Input : arr[] = {1, 2, 8, 10, 11, 12, 19}, x = 0
Output : -1
Since floor doesn't exist, output is -1.
```

**Input:**

The first line of input contains an integer T denoting the number of test cases.  
The first line of each test case is N and x, N is the size of array.
The second line of each test case contains N array elements.

**Output:**

Print **index of** floor of x if it exists, else print -1

**Constraints:**

1 ≤ T ≤ 500  
1 ≤ N ≤ 1000  
0 ≤ X ≤ 1000   
1 ≤ arr[i] ≤ 10000  

**Example:**

**Input:**  
3  
7 0  
1 2 8 10 11 12 19  
7 5  
1 2 8 10 11 12 19  
7 10  
1 2 8 10 11 12 19  

**Output:**   
-1  
1  
3

```python
def Floor(arr, x):
    for i in range(len(arr)):
        if arr[0] == x:
            return 0
        elif arr[0] > x:
            return -1
        elif arr[i] > x:
            return i-i
        elif arr[i] == x:
            return i
    return i

t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    x = N[1]
    arr = list(map(int,input().split()))
    print(Floor(arr, x))
```
