---
layout: post
title:  "[Easy] Sum of Middle Elements of two sorted arrays"
date:   2018-06-15 12:22:00 +0900
categories: Algorithm
---

Given 2 sorted arrays A and B of size N each. Print sum of middle elements of the array obtained after merging the given arrays.

**Input:**  
The first line contains 'T' denoting the number of testcases. Then follows description of testcases.
Each case begins with a single positive integer N denoting the size of array.  
The second line contains the N space separated positive integers denoting the elements of array A.  
The third line contains N space separated positive integers denoting the elements of array B.
 

**Output:**

For each testcase, print the sum of middle elements of two sorted arrays. The number of the elements in the merged array are even so there will be 2 numbers in the center n/2 and n/2 +1. You have to print their sum. 


**Constraints:**  
 1<=T<=50  
 1<=N<=1000  
 1<=A[i]<=100000  
 1<=B[i]<=100000  


**Example:**  
**Input :**   
1  
5  
1 2 4 6 10  
4 5 6 9 12  
 
**Output :**  
11  

```python
def sum_of_middle(arr1, arr2):
    arr3 = sorted(arr1 + arr2)
    result = arr3[len(arr3)//2-1] + arr3[len(arr3)//2]
    return result

t = int(input())
for i in range(t):
    n = input()
    arr1 = list(map(int, input().split()))
    arr2 = list(map(int, input().split()))
    print(sum_of_middle(arr1,arr2))
```