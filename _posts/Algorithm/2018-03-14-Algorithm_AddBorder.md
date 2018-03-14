---
layout: post
title:  "[Stage 15] Add Border"
date:   2018-03-14 11:40:00 +0900
categories: Algorithm
---

Given a rectangular matrix of characters, add a border of asterisks(*) to it.

**Example**

For

```
picture = ["abc",
           "ded"]
```

the output should be

```
addBorder(picture) = ["*****",
                      "*abc*",
                      "*ded*",
                      "*****"]
```

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] array.string picture**

A non-empty array of non-empty equal-length strings.

**Guaranteed constraints:**  
1 ≤ picture.length ≤ 100,  
1 ≤ picture[i].length ≤ 100.  

**[output] array.string**

The same matrix of characters, framed with a border of asterisks of width `1`.

```python
def addBorder(picture):
    top_bottom_frame = ""
    framed_picture = list()

    for i in range(len(picture)):
        if i == 0:
            top_bottom_frame = '*' * (len(picture[0])+2)
            framed_picture.append(top_bottom_frame)

        framed_str = '*' + picture[i] + '*'
        framed_picture.append(framed_str)

        if i == len(picture)-1:
            top_bottom_frame = '*' * (len(picture[0])+2)
            framed_picture.append(top_bottom_frame)

    return framed_picture
```
