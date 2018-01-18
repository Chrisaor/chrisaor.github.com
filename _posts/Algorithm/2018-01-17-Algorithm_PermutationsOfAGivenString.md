---
layout: post
title:  "[Basic] Permutations of a given string"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given a string, print all permutations of a given string.

Input:

The first line of input contains an integer T denoting the number of test cases.
Each test case contains a single string in capital letter.

Output:

Print all permutations of a given string with single space and all permutations should be in lexicographically increasing order.

Constraints:

1 ≤ T ≤ 10<br>
1 ≤ size of string ≤ 5

Example:

Input:<br>
2<br>
ABC

ABSG

Output:<br>
ABC ACB BAC BCA CAB CBA 

ABGS ABSG AGBS AGSB ASBG ASGB BAGS BASG BGAS BGSA BSAG BSGA GABS GASB GBAS GBSA GSAB GSBA SABG SAGB SBAG SBGA SGAB SGBA 

```python
from itertools import permutations


def allPermutations(str):
    answer = []
    permList = permutations(str)
    for perm in list(permList):
        answer.append(''.join(perm))
    return ' '.join(answer)


t = int(input())

for i in range(t):
    str1 = input()
    strList = []
    for j in range(len(str1)):
        strList.append(str1[j])
    strList.sort()
    str2 = ''.join(strList)
    print(allPermutations(str2))
```