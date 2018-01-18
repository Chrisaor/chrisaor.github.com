---
layout: post
title:  "[Easy] Key Pair"
date:   2018-01-18 11:30:00 +0900
categories: Algorithm
---

Given an array A[] of n numbers and another number x, determine whether or not there exist two elements in A whose sum is exactly x.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N and X,N is the size of array.
The second line of each test case contains N integers representing array elements C[i].

Output:

Print "Yes" if there exist two elements in A whose sum is exactly x, else "No" without quotes.

Constraints:

1 ≤ T ≤ 200<br>
1 ≤ N ≤ 200<br>
1 ≤ C[i] ≤ 1000

Example:

Input:<br>
2<br>
6 16<br>
1 4 45 6 10 8<br>
5 10<br>
1 2 4 3 6

Output:<br>
Yes<br>
Yes

```python
def keyPair(n, x, list2):
    for i in range(n):
        temp = 0
        for j in range(i+1,n):
            temp = list2[i] + list2[j]
            if temp == x:
                return "Yes"

    return "No"
    
t = int(input())
for i in range(t):
    size = list(map(int, input().split()))
    n = size[0]
    x = size[1]
    list2 = sorted(list(map(int, input().split())))
    print(keyPair(n,x,list2)) 
```