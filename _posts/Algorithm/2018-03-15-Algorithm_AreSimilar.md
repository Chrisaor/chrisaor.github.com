---
layout: post
title:  "[Stage 16] Are Similar?"
date:   2018-03-15 11:40:00 +0900
categories: Algorithm
---

Two arrays are called _similar_ if one can be obtained from another by swapping at most one pair of elements in one of the arrays.

Given two arrays `a` and `b`, check whether they are _similar_.

**Example**

- For `a = [1, 2, 3]` and `b = [1, 2, 3]`, the output should be
`areSimilar(a, b) = true`.

The arrays are equal, no need to swap any elements.

For `a = [1, 2, 3]` and `b = [2, 1, 3]`, the output should be
`areSimilar(a, b) = true`.

We can obtain b from a by swapping 2 and 1 in b.

For `a = [1, 2, 2]` and `b = [2, 1, 1]`, the output should be
`areSimilar(a, b) = false`.

Any swap of any two elements either in `a` or in `b` won't make `a` and `b` equal.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

**[input] array.integer a**

Array of integers.

**Guaranteed constraints:**  
3 ≤ a.length ≤ 105,  
1 ≤ a[i] ≤ 1000.  

**[input] array.integer b**

Array of integers of the same length as `a`.

**Guaranteed constraints:**
`b.length = a.length`,  
`1 ≤ b[i] ≤ 1000`.

**[output] boolean**

`true` if `a` and `b` are similar, `false` otherwise.


```python
def aresimilar(a, b):
    if a == b:
        return True
    elif sorted(a) != sorted(b):
        return False

    for i in a:
        if i not in b:
            return False

    cnt = 0
    for i,j in zip(a,b):
        if i != j:
            cnt += 1
            if cnt > 2:
                return False
    if cnt <=2:
        return True
```