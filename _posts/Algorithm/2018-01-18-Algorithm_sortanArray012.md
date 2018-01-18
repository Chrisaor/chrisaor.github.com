---
layout: post
title:  "[Easy] Sort an array of 0s, 1s and 2s"
date:   2018-01-18 11:31:00 +0900
categories: Algorithm
---

Write a program to sort an array of 0's,1's and 2's in ascending order.

Input:

The first line contains an integer 'T' denoting the total number of test cases. In each test cases, First line is number of elements in array 'N' and second its values.

Output: 

Print the sorted array of 0's, 1's and 2's.

Constraints: 

1 <= T <= 100<br>
1 <= N <= 100<br>
0 <= arr[i] <= 2

Example:

Input :

2<br>
5<br>
0 2 1 2 0<br>
3<br>
0 1 0
 
Output:

0 0 1 2 2<br>
0 0 1

```python
def sortArr012(arr):
    sortedarr = sorted(arr)
    answer = ' '.join(map(str, sortedarr))
    return answer

t = int(input())
for i in range(t):
    n = list(map(int, input().split()))
    arr = list(map(int, input().split()))
    print(sortArr012(arr))
```
