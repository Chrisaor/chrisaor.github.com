---
layout: post
title:  "[Stage 27] Even Digits Only"
date:   2018-06-04 12:16:00 +0900
categories: Algorithm
---

Check if all digits of the given integer are even.

**Example**

- For `n = 248622`, the output should be
`evenDigitsOnly(n) = true`;
- For `n = 642386`, the output should be
`evenDigitsOnly(n) = false`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] integer n**

_Guaranteed constraints_:
`1 ≤ n ≤ 109`.

**[output] boolean**

`true` if all digits of `n` are even, `false` otherwise.

```python
def evenDigitsOnly(n):
    while n > 0:
        if not n % 2 == 0:
            return False
        else:
            n //= 10
    return True
```