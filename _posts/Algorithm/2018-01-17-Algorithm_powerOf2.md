---
layout: post
title:  "[Basic] Power of 2"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given a positive integer N, check if N is a power of 2.

Input:<br>
The first line contains 'T' denoting the number of test cases. Then follows description of test cases.
Each test case contains a single positive integer N.


Output:<br>
Print "YES" if it is a power of 2 else "NO". (Without the double quotes)


Constraints:<br>
1<=T<=100<br>
0<=N<=10^18

Example:<br>
Input :<br>
2<br>
1<br>
98

Output :<br>
YES<br>
​NO

Explanation:  (2^0 == 1)

```python
def powerOf2(n):
    if n == 1:
        return print("YES")
    elif n == 0:
        return print("NO")
    else:
        while n // 2 != 0:
            if n % 2 != 0:
                return print("NO")
            else:
                n = n // 2
    return print("YES")





t = int(input())

for i in range(t):
    n = int(input())
    powerOf2(n)
```