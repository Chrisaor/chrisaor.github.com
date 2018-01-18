---
layout: post
title:  "[Basic] Remove Spaces"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given a string, remove spaces from it

Input: First line of the input is the number of test cases T. And first line of test case contains a string whose spaces are to be removed.

Output:  Modified string without any space

Constraints:  Input String Length <= 1000

Example:
Input:<br>
2<br>
geeks  for geeks<br>
&nbsp;&nbsp;&nbsp;&nbsp; g f g<br>

Output:<br>
geeksforgeeks<br>
gfg

```python
def removeSpace(s):
    result = []
    answer = ''
    parsedStr = s.split(' ')
    for i in range(len(parsedStr)):
        if parsedStr[i] != ' ':
            result.append(parsedStr[i])
    answer = ''.join(result)
    return answer

t = int(input())
for i in range(t):
    s = input()
    print(removeSpace(s))
```

