---
layout: post
title:  "[Easy] Product array puzzle"
date:   2018-04-19 12:10:00 +0900
categories: Algorithm
---

Given an array arr[] of n integers, construct a Product Array prod[] (of same size) such that prod[i] is equal to the product of all the elements of arr[] except arr[i].

**Input:**

The first line of input contains an integer T denoting the number of test cases.  
The first line of each test case is N,N is the size of array.  
The second line of each test case contains N input A[i].

**Output:**

Print the Product array prod[].

**Constraints:**

1 ≤ T ≤ 10  
1 ≤ N ≤ 15  
1 ≤ C[i] ≤ 20

**Example:**

**Input**  
2  
5  
10 3 5 6 2  
2  
12 20

**Output**  
180 600 360 300 900  
20 12

```python
def product_array_puzzle(arr):
    prod = 1
    temp = list()
    result = list()
    for i in range(len(arr)):
        temp = arr[:i] + arr[i+1:]
        for j in temp:
            prod *= j
        result.append(str(prod))
        prod = 1
    return ' '.join(result)

t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(product_array_puzzle(arr))
```