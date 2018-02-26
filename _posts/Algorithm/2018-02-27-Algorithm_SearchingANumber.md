---
layout: post
title:  "[Basic] Searching a number"
date:   2018-02-27 10:15:00 +0900
categories: Algorithm
---

Given an array of N elements and a integer K , return the position of first occurence of K in given array.  Position of first element is considered as 1.  
Output -1 if the number is not found in an array.

**Input:**

The first line contains 'T' denoting the number of testcases. Then follows description of testcases.
Each case begins with a two space separated integer N and K denoting the size of array and the value of K respectively. The next line contains the N space separated integers denoting the elements of array.


**Output:**

For each test case, print the index of first occurrence of given number K.  
Print -1 if the number is not found in array.


**Constraints:**

1<=T<=100  
1<=N<=1000  
1<=K<=100000  
1<=A[i]<=100000  


**Example:**

**Input :**  
2   
5 16  
9 7 2 16 4  
7 98  
1 22 57 47 34 18 66  

**Output : **  
4  
-1  

```python
def search_num(n, arr):
    try:
        return arr.index(n)+1
    except ValueError:
        return -1

t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    n = N[1]
    arr = list(map(int, input().split()))
    print(search_num(n, arr))
```