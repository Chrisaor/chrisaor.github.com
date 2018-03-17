---
layout: post
title:  "[Stage 18] Palindrom Rearranging"
date:   2018-03-17 11:45:00 +0900
categories: Algorithm
---

Given a string, find out if its characters can be rearranged to form a palindrome.

**Example**

For `inputString = "aabb"`, the output should be
`palindromeRearranging(inputString) = true`.

We can rearrange `"aabb"` to make `"abba"`, which is a palindrome.

**Input/Output**

**[execution time limit] 4 seconds (py3)**

**[input] string inputString**

A string consisting of lowercase English letters.

**Guaranteed constraints:**  
`1 ≤ inputString.length ≤ 50`.

**[output] boolean**

`true` if the characters of the `inputString` can be rearranged to form a palindrome, `false` otherwise.

```python
def palindromeRearranging(inputString):
    temp = list()
    odd_cnt = 0
    if len(set(list(inputString))) == 1:
        return True
    for i in set(inputString):
        print(inputString.count(i))
        if inputString.count(i) % 2 == 1:
            odd_cnt += 1
        else:
            continue
    if odd_cnt <= 1:
        return True
    else:
        return False
```