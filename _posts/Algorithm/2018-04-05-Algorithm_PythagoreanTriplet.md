---
layout: post
title:  "[Easy] Pythagorean Triplet"
date:   2018-04-05 11:55:00 +0900
categories: Algorithm
---

Given an array of integers, write a function that returns true if there is a triplet (a, b, c) that satisfies a2 + b2 = c2.

**Input:**  
The first line contains 'T' denoting the number of testcases.  Then follows description of testcases.  
Each case begins with a single positive integer N denoting the size of array.  
The second line contains the N space separated positive integers denoting the elements of array A.

**Output:**  
For each testcase, print "Yes" or  "No" whtether it is Pythagorean Triplet or not.

**Constraints:**  
1<=T<=50  
1<=N<=100  
1<=A[i]<=100  

**Example:**  
**Input:**  
1    
5  
3 2 4 6 5  
**Output:**  
Yes


```python
def pythagorean(arr):
    sq_arr = [i*i for i in arr]
    for i in range(len(sq_arr)):
        for j in range(i+1,len(sq_arr)):
            sum = sq_arr[i]+sq_arr[j]
            if sum in sq_arr:
                return 'Yes'
    return 'No'

t = int(input())
for i in range(t):
    n = input()
    arr = list(map(int, input().split()))
    print(pythagorean(arr))
```