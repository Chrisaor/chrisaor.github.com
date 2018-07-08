---
layout: post
title:  "[Stage 32] Absolute Values Sum Minimization"
date:   2018-07-08 12:22:00 +0900
categories: Algorithm
---

Given a sorted array of integers a, find an integer x from a such that the value of

```
abs(a[0] - x) + abs(a[1] - x) + ... + abs(a[a.length - 1] - x)
```
is the smallest possible (here `abs` denotes the absolute value).  
If there are several possible answers, output the smallest one.

**Example**

For `a = [2, 4, 7]`, the output should be
`absoluteValuesSumMinimization(a) = 4`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] array.integer a**

A non-empty array of integers, sorted in ascending order.

Guaranteed constraints:
`1 ≤ a.length ≤ 200`,
`-106 ≤ a[i] ≤ 106`.

**[output] integer**

```python
def absoluteValuesSumMinimization(a):
    result = list()
    for i in range(len(a)):
        sum = 0
        for j in range(len(a)):
            sum += abs(a[i]-a[j])
        result.append(sum)
    return a[result.index(min(result))]
```