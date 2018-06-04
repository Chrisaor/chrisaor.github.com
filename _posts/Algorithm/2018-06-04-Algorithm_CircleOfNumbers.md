---
layout: post
title:  "[Stage 30] Circle of Numbers"
date:   2018-06-04 12:19:00 +0900
categories: Algorithm
---

Consider integer numbers from `0` to `n - 1` written down along the circle in such a way that the distance between any two neighbouring numbers is equal (note that `0` and `n - 1` are neighbouring, too).

Given `n` and `firstNumber`, find the number which is written in the radially opposite position to `firstNumber`.

**Example**

For `n = 10` and `firstNumber = 2`, the output should be
`circleOfNumbers(n, firstNumber) = 7`.

![image](https://user-images.githubusercontent.com/33015649/40902673-c34897c8-680f-11e8-8259-c9a27fb6ac32.png)


**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] integer n**

A positive **even** integer.

_Guaranteed constraints_:
**4 ≤ n ≤ 20**.

- **[input] integer firstNumber**

_Guaranteed constraints_:
`0 ≤ firstNumber ≤ n - 1`.

[output] integer

```python
def circleOfNumbers(n, firstNumber):
    return firstNumber+n//2 if firstNumber+n//2 < n else firstNumber+n//2 - n
```
 