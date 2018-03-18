---
layout: post
title:  "[Stage 20] Array Maximal Adjacent Difference"
date:   2018-03-18 11:50:00 +0900
categories: Algorithm
---

Given an array of integers, find the maximal absolute difference between any two of its adjacent elements.

**Example**

For `inputArray = [2, 4, 1, 0]`, the output should be
`arrayMaximalAdjacentDifference(inputArray) = 3`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] array.integer inputArray**

**Guaranteed constraints:**  
`3 ≤ inputArray.length ≤ 10`,  
`-15 ≤ inputArray[i] ≤ 15`.

**[output] integer**

The maximal absolute difference.

```python
def arrayMaximalAdjacentDifference(inputArray):
    for i in range(len(inputArray)):
        if i == 0:
            cnt = 0
        elif abs(inputArray[i] - inputArray[i-1]) >= cnt:
            cnt = abs(inputArray[i] - inputArray[i-1])
    return cnt
```
