---
layout: post
title:  "[Basic] Frequency of Array Elements"
date:   2018-03-19 11:45:00 +0900
categories: Algorithm
---

      
Given an unsorted array of N integers which can contain integers from 1 to N. Some elements can be repeated multiple times and some other elements can be absent from the array. Count frequency of all elements from 1 to N.

**Input:**

The first line contains an integer 'T' denoting the total number of test cases. In each test cases, the first line contains an integer 'N' denoting the size of array. The second line contains N space-separated integers A1, A2, ..., AN denoting the elements of the array.

**Output:**

For each test case output N space-separated integers denoting the frequency of each element from 1 to N.

**Constraints:**

1 ≤ T ≤ 100

1 ≤ N ≤ 100

**Example:**

**Input:**  
2  
5  
2 3 2 3 5  
4  
3 3 3 3  

**Output:**  
0 2 2 0 1  
0 0 4 0  


```python
def frequency_elements(N, arr):
    e_list = list()
    for i in range(N):
        e_list.append(0)

    for i in range(len(arr)):
        e_list[arr[i]-1] += 1

    return ' '.join(map(str, e_list))

t = int(input())
for i in range(t):
    N = int(input())
    arr = list(map(int, input().split()))
    print(frequency_elements(N, arr))
```
