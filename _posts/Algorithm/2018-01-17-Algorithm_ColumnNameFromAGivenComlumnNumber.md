---
layout: post
title:  "[Easy]  Column name from a given column number"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

MS Excel columns has a pattern like A, B, C, … ,Z, AA, AB, AC,…. ,AZ, BA, BB, … ZZ, AAA, AAB ….. etc. In other words, column 1 is named as “A”, column 2 as “B”, column 27 as “AA”.

Input:

The first line of each input consists of the test cases. And, the second line consists of a number N.

Output:

In each separate line print the corresonding column title as it appears in an Excel sheet.

Constraints:

1 ≤ T ≤ 70<br>
1 ≤ N ≤ 4294967295

Example:

Input:

2<br>
28<br>
13

Output:

AB<br>
M

```python
def columName(n):
    answer = ""
    while n:
        letter = chr(((n-1) % 26) + 65)
        answer += letter
        if (n % 26) == 0:
            n = n - 1
        n = n//26
    return answer[::-1]

t = int(input())
for i in range(t):
    n = int(input())
    print(columName(n))
```