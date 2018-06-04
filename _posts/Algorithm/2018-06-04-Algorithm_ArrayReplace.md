---
layout: post
title:  "[Stage 26] Array Replace"
date:   2018-06-04 12:15:00 +0900
categories: Algorithm
---

Given an array of integers, replace all the occurrences of `elemToReplace` with `substitutionElem`.

**Example**

For `inputArray = [1, 2, 1]`, `elemToReplace = 1` and `substitutionElem = 3`, the output should be
`arrayReplace(inputArray, elemToReplace, substitutionElem) = [3, 2, 3]`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] array.integer inputArray**

_Guaranteed constraints_:
`2 ≤ inputArray.length ≤ 10`,
`0 ≤ inputArray[i] ≤ 10`.

**[input] integer elemToReplace**

_Guaranteed constraints_:
`0 ≤ elemToReplace ≤ 10`.

**[input] integer substitutionElem**

_Guaranteed constraints_:
`0 ≤ substitutionElem ≤ 10`.

**[output] array.integer**


```python
def arrayReplace(inputArray, elemToReplace, substitutionElem):
    for i in range(len(inputArray)):
        if inputArray[i] == elemToReplace:
            inputArray[i] = substitutionElem
    return inputArray
```