---
layout: post
title:  "[Basic] Maximum product of two numbers"
date:   2018-01-23 13:40:00 +0900
categories: Algorithm
---


Given an array with all elements greater than or equal to zero.Return the maximum product of two numbers possible.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N, N is size of array.
The second line of each test case contains N input A[i].

Output:

Print the maximum product of two numbers possible.

Constraints:

1 ≤ T ≤ 20  
2 ≤ N ≤ 50  
0 ≤ A[i] ≤ 1000  

Example:

Input:  
1  
5  
1 100 42 4 23  

Output:  
4200  


```python
def maximum_product(arr):
    result = []
    for i in range(len(arr)):
        for j in range(i+1,len(arr)):
            result.append(arr[i]*arr[j])
    return str(max(result))


t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(maximum_product(arr))
```


 