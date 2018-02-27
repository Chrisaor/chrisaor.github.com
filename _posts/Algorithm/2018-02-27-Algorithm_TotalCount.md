---
layout: post
title:  "[Basic] Total count"
date:   2018-02-27 11:15:00 +0900
categories: Algorithm
---


Given an array and a threshold value k where k is used to divide each element of the array. Find the total number of divisions we get after dividing each element of the array by k.   
for example:  
A[ ] = 5 8 10 13 6 2 and k is 3  
Output will be 17   

**Explanation:**  

|Number|Parts|counts|
|:-:|:-:|:-:|
|5|{3,2}|2|
|8|{3,3,2}|3|
|10|{3,3,3,1}|3|
|13|{3,3,3,3,1}|5|
|6|{3,3}|2|
|2|{2}|1|

The result thus will be 2+3+4+5+2+1 = 17 

**Input:**

The first line of input contains a single integer **T** denoting the number of test cases. Then **T** test cases follow. Each test case consist of two lines. The first line of each test case consists of an integer **N** and threshold value **k**, where **N** is the size of array.
The second line of each test case contains **N** space separated integers denoting array elements.

**Output:**

Corresponding to each test case, in a new line, print the total count.

**Constraints:**

1 ≤ T ≤ 100  
1 ≤ N ≤ 500  
1 ≤ A[i] ≤ 1000  
1 ≤ Threshold value(k) ≤ 20  

**Example:**

**Input**  
1  
6 3  
5 8 10 13 6 2  

**Output**  
17


```python
def total_count(key, arr):
    cnt = 0
    for i in range(len(arr)):
        if arr[i]//key == arr[i]/key:
            cnt += arr[i]//key
        else:
            cnt += arr[i]//key + 1
    return cnt

key = 3
arr = [5, 8, 10, 13, 6, 2]

t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    key = N[1]
    arr = list(map(int, input().split()))
    print(total_count(key, arr))
```