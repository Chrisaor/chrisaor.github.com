---
layout: post
title:  "[Stage 22] Avoid Obstacles"
date:   2018-03-23 11:45:00 +0900
categories: Algorithm
---

You are given an array of integers representing coordinates of obstacles situated on a straight line.

Assume that you are jumping from the point with coordinate `0` to the right. You are allowed only to make jumps of the same length represented by some integer.

Find the minimal length of the jump enough to avoid all the obstacles.

**Example**

For `inputArray = [5, 3, 6, 7, 9]`, the output should be
`avoidObstacles(inputArray) = 4`.

Check out the image below for better understanding:

![example](https://user-images.githubusercontent.com/33015649/37811451-b03001ea-2e9d-11e8-8407-09bff5afa53a.png)


**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] array.integer inputArray**

Non-empty array of positive integers.

**Guaranteed constraints:**  
`2 ≤ inputArray.length ≤ 10`,  
`1 ≤ inputArray[i] ≤ 40`.  

**[output] integer**

The desired length.

```python
def avoidObstacles(inputArray):
    inputArray = sorted(inputArray)
    for i in range(1, 41):
        jump = 0
        for j in range(inputArray[-1]+2):
            jump += i
            if jump in inputArray:
                break
            elif jump > inputArray[-1] and j == inputArray[-1]+1:
                return i
```