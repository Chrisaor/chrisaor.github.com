---
layout: post
title:  "[Level 12] Sorted by Height"
date:   2018-02-07 10:15:00 +0900
categories: Algorithm
---


Some people are standing in a row in a park. There are trees between them which cannot be moved. Your task is to rearrange the people by their heights in a non-descending order without moving the trees.

Example

For `a = [-1, 150, 190, 170, -1, -1, 160, 180]`, the output should be
`sortByHeight(a) = [-1, 150, 160, 170, -1, -1, 180, 190]`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] array.integer a**

If `a[i] = -1`, then the ith position is occupied by a tree. Otherwise a[i] is the height of a person standing in the `i`<sup>th</sup> position.

Guaranteed constraints:  
`5 ≤ a.length ≤ 15,
-1 ≤ a[i] ≤ 200`.

**[output] array.integer**

Sorted array `a` with all the trees untouched.

```python
def sortByHeight(a):
    b = [i for i in sorted(a) if i != -1]
    j = 0
    for i in range(len(a)):
        if a[i] != -1:
            a[i] = b[j]
            j += 1
    return a
```
