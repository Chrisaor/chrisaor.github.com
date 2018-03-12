---
layout: post
title:  "[School] Alone in couple"
date:   2018-03-12 11:40:00 +0900
categories: Algorithm
---

In a party everyone is in couple except one. People who are in couple have same numbers. Find out the person who is not in couple.

**Input:**  
The first line contains an integer 'T' denoting the total number of test cases. In each test cases, the first line contains an integer 'N' denoting the size of array. The second line contains N space-separated integers A1, A2, ..., AN denoting the elements of the array. (N is always odd)


**Output:**  
In each seperate line print number of the person not in couple.


**Constraints:**  
1<=T<=30  
1<=N<=500  
1<=A[i]<=500  
N%2==1  


**Example:**  
**Input:**  
1  
5  
1 2 3 2 1  

**Output:**  
3


```python
def alone_in_couple(N, arr):
    temp = list()
    for i in range(N):
        if arr[i] not in temp:
            temp.append(arr[i])
        else:
            temp.remove(arr[i])
    return temp[0]


t = int(input())
for i in range(t):
    N = int(input())
    arr = list(map(int, input().split()))
    print(alone_in_couple(N, arr))
```