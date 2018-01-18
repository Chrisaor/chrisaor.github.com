---
layout: post
title:  "[Easy] Next larger element"
date:   2018-01-18 12:30:00 +0900
categories: Algorithm
---

Given an array A [ ] having distinct elements, the task is to find the next greater element for each element of the array in order of their appearance in the array. If no such element exists, output -1 

Input:

The first line of input contains a single integer T denoting the number of test cases.Then T test cases follow. Each test case consists of two lines. The first line contains an integer N denoting the size of the array. The Second line of each test case contains N space separated positive integers denoting the values/elements in the array A[ ].
 
Output:

For each test case, print in a new line, the next greater element for each array element separated by space in order.

Constraints:<br>
1<=T<=200<br>
1<=N<=1000<br>
1<=A[i]<=1000<br>

Example:<br>

Input:<br>
1<br>
4<br>
1 3 2 4 <br>

Output:<br>
3 4 4 -1<br>

Explanation:<br>
In the array, the next larger element to 1 is 3 , 3 is 4 , 2 is 4 and for 4 ? since it doesn't exist hence -1.


```python
def nextLarger(n, arr):
    result = []
    for i in range(n):
        if i+1 == n:
           result.append(-1)
        else:
            for j in range(i, n):
                if j+1 == n:
                    result.append(-1)
                elif arr[j+1] > arr[i]:
                    result.append(arr[j+1])
                    break;

    answer = ' '.join(list(map(str, result)))
    return answer


t = int(input())
for x in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(nextLarger(n,arr))
```