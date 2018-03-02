---
layout: post
title:  "[Easy] Majority Element"
date:   2018-03-02 11:15:00 +0900
categories: Algorithm
---

Write a program to find the majority element in the array. A majority element in an array A[] of size n is an element that appears more than n/2 times (and hence there is at most one such element).  If input array doesn't contain a majority element, then output "NO Majority Element"

**Input:**  The first line of the input contains T denoting the number of testcases. The first line of the test case will be the size of array and second line will be the elements of the array.

**Output:** For each test case the output will be the majority element of the array.

**Constraints:**

1 <=T<= 100

1 <=N<= 100

0 <= a[i]<= 100


**Example:**

**Input:**

2  
5  
3 1 3 3 2  
3  
1 2 3  

**Output:**  
3  
NO Majority Element  


```python
def majority_element(arr):
    list1 = list()
    for i in arr:
        list1.append(arr.count(i))

    majority = arr[list1.index(max(list1))]
    if max(list1) > len(arr)/2:
        return majority
    else:
        return 'NO Majority Element'


t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(majority_element(arr))
```