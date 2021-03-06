---
layout: post
title:  "[Basic] Check if two arrays are equal or not"
date:   2018-02-08 10:15:00 +0900
categories: Algorithm
---
[https://practice.geeksforgeeks.org/problems/check-if-two-arrays-are-equal-or-not/0](https://practice.geeksforgeeks.org/problems/check-if-two-arrays-are-equal-or-not/0)

Given two arrays of equal length, the task is to find if given arrays are equal or not. Two arrays are said to be equal if both of them contain same set of elements, arrangements (or permutation) of elements may be different though.

**Note** : If there are repetitions, then counts of repeated elements must also be same for two array to be equal.

**Examples:**

```
Input  : A[] = {1, 2, 5, 4, 0};
         B[] = {2, 4, 5, 0, 1}; 
Output : 1

Input  : A[] = {1, 2, 5};
         B[] = {2, 4, 15}; 
Output : 0
```

**Input:**  
The first line of input contains an integer T denoting the no of test cases. Then T test cases follow.  Each test case contains an integer N denoting the size of the array. Then in the next two lines are N space separated values of the array of arrays A[] and B[].

**Output:**  
For each test case in a new line print 1 if the arrays are equal else print 0.

**Constraints:**  
1<=T<=100  
1<=N<=100  
1<=A[],B[]<=1000  

**Example:**  
**Input:**  
2    
5  
1 2 5 4 0  
2 4 5 0 1  
3  
1 2 5  
2 4 15  
**Output:**  
1  
0

```python
def equal_arr(arr1, arr2):
    if sorted(arr1) == sorted(arr2):
        return 1
    else:
        return 0


t = int(input())
for i in range(t):
    n = int(input())
    arr1 = list(map(int, input().split()))
    arr2 = list(map(int, input().split()))
    print(equal_arr(arr1,arr2))
```