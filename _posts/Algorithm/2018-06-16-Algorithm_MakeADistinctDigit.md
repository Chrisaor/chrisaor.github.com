---
layout: post
title:  "[Basic] Make a Distict Digit Array"
date:   2018-06-16 12:22:00 +0900
categories: Algorithm
---

Given an array A[] of n elements.The task is to make a sorted array which will contain all distinct digits present in A[].
 

**Input:**  
The first line of input contains an integer T denoting the no of test cases. Then T test cases follow. Each test case contains an integer N, denoting the length of the array. Then in the next line are N space separated integers of the array. 

**Output:**  
For each test case in a new line print the distinct array of digits.

**Constraints:**  
1<=T<=100  
1<=n<=200  
1<=A[]<=1000  


**Example:**  
**Input:**  
2  
3  
131 11 48  
4  
111 222 333 446  

**Output:**  
1 3 4 8  
1 2 3 4 6

```python
def distinct_array(arr):
    result = list()
    for i in range(len(arr)):
        for j in range(len(str(arr[i]))):
            if str(arr[i])[j] not in result:
                result.append(str(arr[i])[j])

    return ' '.join(sorted(result))

t = int(input())
for i in range(t):
    n = input()
    arr1 = list(map(int, input().split()))
    print(distinct_array(arr1))
```