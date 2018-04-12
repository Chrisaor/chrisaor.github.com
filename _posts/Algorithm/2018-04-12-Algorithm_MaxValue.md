---
layout: post
title:  "[Easy] Max value"
date:   2018-04-12 11:55:00 +0900
categories: Algorithm
---

In a given array A[] find the maximum value of (A[i] – i) - (A[j] – j) where i is not equal to j. 

i and j vary from 0 to n-1 and N is size of input array A[].  Value of N is always greater than 1.

**Input:**  

The first line of input contains a single integer T denoting the number of test cases. Then T test cases follow. Each test case consist of two lines. The first line of each test case consists of an integer N, where N is the size of array.The second line of each test case contains N space separated integers denoting array elements.

**Output:**

Corresponding to each test case, in a new line, print the maximum value.

**Constraints:**

1 ≤ T ≤ 100  
2 ≤ N ≤ 200  
1 ≤ A[i] ≤ 1000

**Example:**

**Input**
1  
5  
9 15 4 12 13

**Output**  
12     

**Explanation**  
(a[1]-1) - (a[2]-2) = (15-1)-(4-2) = 12

```python
def max_value(arr):
    temp = list()

    for i in range(len(arr)):
        value = 0
        for j in range(i+1,len(arr)):
            value = abs((arr[i]-i) - (arr[j]-j))
            temp.append(value)

    return max(temp)

t = int(input())
for i in range(t):
    n = input()
    arr = list(map(int, input().split()))
    print(max_value(arr))
```