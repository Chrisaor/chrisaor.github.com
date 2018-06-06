---
layout: post
title:  "[Easy] Greater on right side"
date:   2018-06-06 12:22:00 +0900
categories: Algorithm
---

Given an array of integers, replace every element with the next greatest element (greatest element on the right side) in the array. Since there is no element next to the last element, replace it with -1.

**Input:**

The first line of input contains an integer T denoting the number of test cases.  
The first line of each test case is N,N is the size of array.
The second line of each test case contains N input A[i].

**Output:**

Print the modified array.

**Constraints:**

1 ≤ T ≤ 50  
1 ≤ N ≤ 100  
1 ≤ A[i] ≤ 1000  

**Example:**  

**Input:**  
2  
6  
16 17 4 3 5 2  
4  
2 3 1 9  

**Output:**  
17 5 5 5 2 -1  
9 9 9 -1


```python
def greater_on_right(arr):
    temp = arr
    result = list()
    for i in range(len(arr)):
        if i == len(arr)-1:
            result.append(-1)
        else:
            result.append(max(temp[i+1:]))
    return ' '.join(map(str, result))

t = int(input())
for i in range(t):
    n = input()
    arr = list(map(int, input().split()))
    print(greater_on_right(arr))
```