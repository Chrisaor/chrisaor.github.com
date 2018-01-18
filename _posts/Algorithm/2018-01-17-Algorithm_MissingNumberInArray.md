---
layout: post
title:  "[Basic] Missing number in array"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given an array of size n-1 and given that there are numbers from 1 to n with one missing, the missing number is to be found.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N.
The second line of each test case contains N-1 input C[i],numbers in array.

Output:

Print the missing number in array.

Constraints:

1 ≤ T ≤ 200<br>
1 ≤ N ≤ 1000<br>
1 ≤ C[i] ≤ 1000<br>

Example:

Input:<br>
2<br>
5<br>
1 2 3 5<br>
10<br>
1 2 3 4 5 6 7 8 10

Output:<br>
4<br>
9

```python
def MissingNumber(n, arr):
    sumOfN = n*(n+1)/2
    return sumOfN - sum(arr)

t = int(input())
for i in range(t):
    n = int(input())
    arr = map(int, input().split())
    print(MissingNumber(n, arr))
```
 