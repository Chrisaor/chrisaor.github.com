---
layout: post
title:  "[Stage 17] Array Change"
date:   2018-03-15 11:45:00 +0900
categories: Algorithm
---

You are given an array of integers. On each move you are allowed to increase exactly one of its element by one. Find the minimal number of moves required to obtain a strictly increasing sequence from the input.

**Example**

For `inputArray = [1, 1, 1]`, the output should be
`arrayChange(inputArray) = 3`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] array.integer inputArray**

**Guaranteed constraints:**    
`3 ≤ inputArray.length ≤ 105`,
`-105 ≤ inputArray[i] ≤ 105`.

**[output] integer**

The minimal number of moves needed to obtain a strictly increasing sequence from `inputArray`.
It's guaranteed that for the given test cases the answer always fits signed `32`-bit integer type.

```python
def arrayChange(inputArray):
    sum1 = 0
    for i in range(len(inputArray)):
        temp = list()
        if i == 0:
            continue
        else:
            if inputArray[i] > inputArray[i-1]:
                continue
            else:
                temp.append(inputArray[i])
                inputArray[i] = inputArray[i-1] +1
                sum1 += inputArray[i] - temp[0]
    return sum1
```