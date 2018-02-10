---
layout: post
title:  "[Basic] Number of occurrence"
date:   2018-02-10 10:15:00 +0900
categories: Algorithm
---

[https://practice.geeksforgeeks.org/problems/number-of-occurrence/0](https://practice.geeksforgeeks.org/problems/number-of-occurrence/0)

Given a sorted array C[] and a number X, write a function that counts the occurrences of X in C[].

**Input:**

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N and X, N is the size of array.
The second line of each test case contains N input C[i].

**Output:**

Print the counts the occurrence of X, if zero then print -1.

**Constraints:**

1 ≤ T ≤ 100  
1 ≤ N ≤ 300  
1 ≤ C[i] ≤ 500  

**Example:**

**Input:**  
2  
7 2  
1 1 2 2 2 2 3  
7 4  
1 1 2 2 2 2 3  

**Output:**  
4  
-1  

**Explanation:**  

In first test case, 2 occurs 4 times in 1 1 2 2 2 2 3  
In second test case, 4 is not present in 1 1 2 2 2 2 3


```python
def number_of_occurrence(number, arr):
    cnt = 0
    for i in range(len(arr)):
        if arr[i] == number:
            cnt +=1
    if cnt == 0:
        return -1
    else:
        return cnt


t = int(input())

for i in range(t):

    N = list(map(int, input().split()))
    n = N[0]
    number = N[1]
    arr = list(map(int, input().split()))
    print(number_of_occurrence(number, arr))
```