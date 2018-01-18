---
layout: post
title:  "[Basic] K largest elements"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given an array, print k largest elements from the array.  The output elements should be printed in decreasing order.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N and k, N is the size of array and K is the largest elements to be returned.
The second line of each test case contains N input C[i].

Output:

Print the k largest element in descending order.

Constraints:

1 ≤ T ≤ 100<br>
1 ≤ N ≤ 100<br>
K ≤ N<br>
1 ≤ C[i] ≤ 1000

Example:

Input:
2<br>
5 2<br>
12 5 787 1 23<br>
7 3<br>
1 23 12 9 30 2 50

Output:<br>
787 23<br>
50 30 23

```python
t = int(input())

for i in range(t):
    answer = []
    n = list(map(int, input().split()))
    arr = list(map(int, input().split()))
    for j in range(int(n[1])):
        answer.append(max(arr))
        arr.remove(max(arr))
    print(" ".join(str(i) for i in answer))
```