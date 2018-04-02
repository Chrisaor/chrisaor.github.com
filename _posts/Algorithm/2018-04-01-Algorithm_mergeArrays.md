---
layout: post
title:  "[Easy] Merge Two Sorted Arrays"
date:   2018-04-01 11:55:00 +0900
categories: Algorithm
---

You have to merge the two sorted arrays into one sorted array (in non-increasing order)

**Input:**

First line contains an integer **T**, denoting the number of test cases.
First line of each test case contains two space separated integers **X and Y**, denoting the size of the two sorted arrays.
Second line of each test case contains **X** space separated integers, denoting the first sorted array P.
Third line of each test case contains **Y** space separated integers, denoting the second array Q.


**Output:**

For each test case, print **(X + Y)** space separated integer representing the merged array.


**Constraints:**

1 <= **T** <= 100  
1 <= **X, Y** <= 5*104  
0 <= **Pi, Qi** <= 109


**Example:**

**INPUT:**

1  
4 5  
7 5 3 1  
9 8 6 2 0  

**OUTPUT:**

9 8 7 6 5 3 2 1 0

```python
def merge_array(arr1, arr2):
    return ' '.join(map(str, sorted(arr1+arr2,reverse=True)))

t = int(input())
for i in range(t):
    N = input()
    arr1 = list(map(int, input().split()))
    arr2 = list(map(int, input().split()))
    print(merge_array(arr1, arr2))
```
