---
layout: post
title:  "[Medium] K'th smallest element"
date:   2018-06-16 12:22:00 +0900
categories: Algorithm
---

        
Given an array and a number k where k is smaller than size of array, the task is to find the k’th smallest element in the given array. It is given that all array elements are distinct.

**Input:**

First Line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case consists of three lines. First line of each test case contains an integer N denoting size of the array. Second line contains N space separated integer denoting elements of the array. Third line of the test case contains an integer K.

**Output:**

Corresponding to each test case, print the desired output in a new line.

**Constraints:**

1<=T<=200  
1<=N<=1000  
K

**Expected Time Complexity:** O(n)

**Example:**

**INPUT**  
2  
6  
7 10 4 3 20 15  
3  
5  
7 10 4 20 15  
4

**Output:**  

7  
15

```python
def kth_smallest_element(arr, k):
    return sorted(arr)[k-1]

t = int(input())
for i in range(t):
    n = input()
    arr = list(map(int, input().split()))
    k = int(input())
    print(kth_smallest_element(arr,k))
```
