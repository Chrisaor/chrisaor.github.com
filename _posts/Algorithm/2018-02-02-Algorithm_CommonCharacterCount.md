---
layout: post
title:  "[Stage 10] Common Character Count"
date:   2018-02-02 10:15:00 +0900
categories: Algorithm
---

Given two strings, find the number of common characters between them.

**Example**

For `s1 = "aabcc"` and `s2 = "adcaa"`, the output should be
`commonCharacterCount(s1, s2) = 3`.

Strings have `3` common characters - `2` "a"s and `1` "c".

**Input/Output**

**[execution time limit] 4 seconds (py3)**

**[input] string s1**

A string consisting of lowercase latin letters `a-z`.

Guaranteed constraints:  
`1 ≤ s1.length ≤ 15`.

**[input] string s2**

A string consisting of lowercase latin letters `a-z`.

Guaranteed constraints:
`1 ≤ s2.length ≤ 15`.

**[output] integer**

```python
def commonCharacterCount(s1, s2):
    s1_list = list()
    s2_list = list()

    for i in s1:
        s1_list.append(i)

    for i in s2:
        s2_list.append(i)

    cnt = 0
    for i in s1_list:
        if i in s2_list:
            s2_list.remove(i)
            cnt += 1
    return cnt
```

Clean Code

```python
def commonCharacterCount(s1, s2):
    com = [min(s1.count(i),s2.count(i)) for i in set(s1)]
    return sum(com)
```