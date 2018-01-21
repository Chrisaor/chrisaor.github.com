---
layout: post
title:  "[Easy] Reverse words in a given string"
date:   2018-01-19 13:30:00 +0900
categories: Algorithm
---

Given a String of length N reverse the words in it. Words are separated by dots.

Input:

The first line contains T denoting the number of testcases. Then follows description of testcases. Each case contains a string containing spaces and characters.
 

Output:

For each test case, output a single line containing the reversed String.

Constraints:

1<=T<=20
1<=Lenght of String<=2000


Example:

Input:<br>
2<br>
i.like.this.program.very.much<br>
pqr.mno

Output:<br>
much.very.program.this.like.i<br>
mno.pqr

```python
def reverseStr(str1):
    strList = str1.split(".")[::-1]
    answer = '.'.join(strList)
    return answer

t = int(input())
for i in range(t):
    str1 = input()
    print(reverseStr(str1))
```