---
layout: post
title:  "[Stage 11] Is Lucky?"
date:   2018-02-05 11:15:00 +0900
categories: Algorithm
---

Ticket numbers usually consist of an even number of digits. A ticket number is considered _lucky_ if the sum of the first half of the digits is equal to the sum of the second half.

Given a ticket number `n`, determine if it's _lucky_ or not.

**Example**

For `n = 1230`, the output should be  
`isLucky(n) = true`;  
For `n = 239017`, the output   should be  
`isLucky(n) = false`.

**Input/Output**

**[execution time limit] 4 seconds (py3)**

**[input] integer n**

A ticket number represented as a positive integer with an even number of digits.

Guaranteed constraints:
`10 ≤ n < 106.`

**[output] boolean**

`true` if `n` is a lucky ticket number, `false` otherwise.



```python
def isLucky(n):
    m = str(n)
    count_front = 0
    count_rear = 0
    for i in range(len(m)//2):
        count_front += int(m[i])

    for i in range(len(m)//2, len(m)):
        count_rear += int(m[i])

    return True if count_front==count_rear else False
```