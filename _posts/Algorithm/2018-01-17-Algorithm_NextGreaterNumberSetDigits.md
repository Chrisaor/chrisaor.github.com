---
layout: post
title:  "[Easy]  Next greater number set digits"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given a number n, find the smallest number that has same set of digits as n and is greater than n. If x is the greatest possible number with its set of digits, then print “not possible”.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is n,n is the number.

Output:

Print the greater number than n with same set of digits and if not possible then print "not possible" without double quote.

Constraints:

1 ≤ T ≤ 100<br>
1 ≤ n ≤ 100000

Example:

Input:<br>
2<br>
143<br>
431

Output:<br>
314<br>
not possible

```python
from itertools import permutations

def nextGreater(n):
    sortedStr = sorted(n)
    strOder = []
    idx = 0
    permList = permutations(sortedStr)
    for perm in list(permList):
        element = ''.join(perm)
        if element not in strOder:
            strOder.append(element)
    idx = strOder.index(n) + 1
    try :
        answer = strOder[idx]
    except IndexError:
        return "not possible"
    return answer

t = int(input())
for i in range(t):
    n = input()
    print(nextGreater(n))
```
    