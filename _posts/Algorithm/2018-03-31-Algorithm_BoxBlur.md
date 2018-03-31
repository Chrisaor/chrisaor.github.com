---
layout: post
title:  "[Stage 23] Box Blur"
date:   2018-03-31 11:55:00 +0900
categories: Algorithm
---
추후 코드 수정

```python
import math

def boxBlur(image):
    temp1 = list()
    temp2 = list()
    temp3 = list()
    temp4 = list()
    result = list()
    answer = list()
    image_sum = 0

    for i in range(len(image)):
        for j in range(len(image)):
            for k in range(j, 3+j):
                image_sum += image[i][k]
            temp1.append(image_sum)
            image_sum = 0
            if 3+j >= len(image):
                temp2.append(temp1)
                temp1 = list()
                break;
    for i in range(len(temp2)):
        for j in range(len(temp2)):
            for k in range(i,i+3):
                if k >= len(temp2) or j >= 3:
                    break;
                else:
                    temp3.append(temp2[k][j])
                    if len(temp3) == 3:
                        result.append(sum(temp3)//9)
                        temp3 = list()
        if i >= len(temp2[0])-1:
            break;
    n_matrix = math.sqrt(len(result))

    for i in range(len(result)):
        temp4.append(result[i])
        if i % n_matrix == n_matrix-1:
            answer.append(temp4)
            temp4 = list()

    return answer
```