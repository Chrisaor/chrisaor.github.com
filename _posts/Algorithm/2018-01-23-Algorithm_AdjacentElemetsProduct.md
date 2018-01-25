---
layout: post
title:  "[Stage 4] Adjacent Elements Product"
date:   2018-01-23 10:45:00 +0900
categories: Algorithm
---

Given an array of integers, find the pair of adjacent elements that has the largest product and return that product.

**Example**

For inputArray = [3, 6, -2, -5, 7, 3], the output should be
adjacentElementsProduct(inputArray) = 21.

7 and 3 produce the largest product.

**Input/Output**

- [execution time limit] 4 seconds (py3)

- [input] array.integer inputArray

An array of integers containing at least two elements.

Guaranteed constraints:   
2 ≤ inputArray.length ≤ 10,
-1000 ≤ inputArray[i] ≤ 1000.

- [output] integer

The largest product of adjacent elements.

```python
def adjacentElementsProduct(inputArray):
    result = []
    for i in range(len(inputArray)):
        if i == len(inputArray)-1:
            return max(result)
        else:
            result.append(inputArray[i] * inputArray[i+1])
    return max(result)
```