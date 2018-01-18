---
layout: post
title:  "[School] Largest Element in Array"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given an array, find the largest element in it.

Input:
The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case contains an integer n , the no of elements in the array. Then next line contains n integers of the array.

Output:<br>
Print the maximum element in the array.

Constraints:<br>
1<=T<=100<br>
1<=n<=100<br>
1<=a[i]<=1000

Example:<br>
Input:<br>
2<br>
5<br>
10 324 45 90 9808<br>
7<br>
1 2 0 3 2 4 5

Output :
9808
5

```python
def LargestElement(arr):
    return max(arr)

t = int(input())
for i in range(0, t):
    n = int(input())
    arr = map(int, input().split())
    print(LargestElement(arr))
```