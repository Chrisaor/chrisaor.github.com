---
layout: post
title:  "[Basic] Value equal to index value"
date:   2018-01-22 13:40:00 +0900
categories: Algorithm
---


Given an array, we need to find that element whose value is equal to that of its index value.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N,N is the size of array.
The second line of each test case contains N input A[].

Output:

Print that element whose value is equal to index value. Print "Not Found" when index value does not match with value.
Note: There can be more than one element in the array which have same value as their index. You need to print every such element's index separated by a single space. Follows 1-based indexing of the array. 


Constraints:

1 ≤ T ≤ 30  
1 ≤ N ≤ 50  
1 ≤ A[i] ≤ 1000

Example:

Input:  
2  
5  
15 2 45 12 7  
1  
1  

Output:  
2  
1


```python
def value_index_equal(arr):
    result = []
    for i in range(len(arr)):
        idx = i+1
        if arr[i] == idx:
            result.append(idx)
    if result != []:
        return ' '.join(map(str, result))
    else :
        return "Not Found"

t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(value_index_equal(arr))
```