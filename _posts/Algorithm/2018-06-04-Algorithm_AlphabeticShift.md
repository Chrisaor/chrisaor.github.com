---
layout: post
title:  "[Stage 28] Alphabetic Shift"
date:   2018-06-04 12:18:00 +0900
categories: Algorithm
---

Given a string, replace each its character by the next one in the English alphabet (`z` would be replaced by `a`).

**Example**

For `inputString = "crazy"`, the output should be
`alphabeticShift(inputString) = "dsbaz"`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] string inputString**

Non-empty string consisting of lowercase English characters.

_Guaranteed constraints_:
`1 ≤ inputString.length ≤ 1000`.

- **[output] string**

The result string after replacing all of its characters.

```python
from string import ascii_lowercase

def alphabeticShift(inputString):
    answer = ''
    strings  = ascii_lowercase+'a'
    for i in inputString:
        answer += strings[strings.index(i)+1]
    return answer
```
