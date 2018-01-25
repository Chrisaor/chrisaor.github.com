---
layout: post
title:  "[Stage 5] ShapeArea"
date:   2018-01-25 10:05:00 +0900
categories: Algorithm
---

Below we will define an n-interesting polygon. Your task is to find the area of a polygon for a given n.

A 1-interesting polygon is just a square with a side of length 1. An n-interesting polygon is obtained by taking the n - 1-interesting polygon and appending 1-interesting polygons to its rim, side by side. You can see the 1-, 2-, 3- and 4-interesting polygons in the picture below.

![image](https://user-images.githubusercontent.com/33015649/35366021-975985d4-01ba-11e8-8c51-b4b828599fb2.png)

Example

For n = 2, the output should be
shapeArea(n) = 5;  
For n = 3, the output should be
shapeArea(n) = 13.  

Input/Output

- [execution time limit] 4 seconds (py3)

- [input] integer n

Guaranteed constraints:
1 ≤ n < 104.

- [output] integer

The area of the n-interesting polygon.


```python
def shapeArea(n):
    answer = 1
    if n == 1:
        return 1
    else:
        for i in range(1,n):
            answer += 4*i
    return answer


print(shapeArea(4))
```