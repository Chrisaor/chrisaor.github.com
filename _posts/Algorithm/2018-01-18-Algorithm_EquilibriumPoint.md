---
layout: post
title:  "[Easy] Equilibrium point"
date:   2018-01-18 11:31:00 +0900
categories: Algorithm
---

Given an array A your task is to tell at which position the equilibrium first occurs in the array. Equilibrium position in an array is a position such that the sum of elements below it is equal to the sum of elements after it.

Input:

The first line of input contains an integer T denoting the no of test cases then T test cases follow. First line of each test case contains an integer N denoting the size of the array. Then in the next line are N space separated values of the array A.

Output:

For each test case in a new  line print the position at which the elements are at equilibrium if no equilibrium point exists print -1.

Constraints:<br>
1<=T<=100<br>
1<=N<=100<br>

Example:<br>
Input:<br>
2<br>
1<br>
1v
5<br>
1 3 5 2 2

Output:<br>
1<br>
3<br>

Explanation:

1. Since its the only element hence its the only equilibrium point
2. For second test case equilibrium point is at position 3 as elements below it (1+3) = elements after it (2+2)


```python
def Epoint(n, arr):

    for i in range(n):
        midIdx = i
        if sum(arr[0:midIdx]) == sum(arr[midIdx+1:]):
            return midIdx+1
    return -1

t = int(input())

for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(Epoint(n, arr))
```
