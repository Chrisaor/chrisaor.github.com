---
layout: post
title:  "[Stage 9] All Longest String"
date:   2018-02-01 10:05:00 +0900
categories: Algorithm
---


Given an array of strings, return another array containing all of its longest strings.

**Example**

For `inputArray = ["aba", "aa", "ad", "vcd", "aba"]`, the output should be
`allLongestStrings(inputArray) = ["aba", "vcd", "aba"]`.

**Input/Output**

**[execution time limit] 4 seconds (py3)**

**[input] array.string inputArray**

A non-empty array.

Guaranteed constraints:
`1 ≤ inputArray.length ≤ 10`,
`1 ≤ inputArray[i].length ≤ 10`.

**[output] array.string**

Array of the longest strings, stored in the same order as in the `inputArray`.

```python
def allLongestStrings(inputArray):
    result = list()
    max_str_len = 0
    for i in inputArray:
        if max_str_len < len(i):
            max_str_len = len(i)

    for j in inputArray:
        if len(j) == max_str_len:
            result.append(j)

    return result
```


**clean code**

```python
def allLongestStrings(inputArray):
    m = max(len(s) for s in inputArray)
    r = [s for s in inputArray if len(s) == m]
    return r
```

다음에는 list comprehension을 써보도록 하자!