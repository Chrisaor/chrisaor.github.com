---
layout: post
title:  "[Basic] Set Bits"
date:   2018-01-31 15:15:00 +0900
categories: Algorithm
---

Given a positive integer **N**, print count of set bits in it. For example, if the given number is 6, output should be 2 as there are two set bits in it.

**Input:**

The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. The next T lines will contain an integer **N**.

**Output:**  
Corresponding to each test case, in a new line, print count of set bits in it.

**Constraints:**

1 ≤ T ≤ 100

1 ≤ N ≤ 1000000


**Example:**

**Input:**

2  
6  
11  
 

**Output:**  
2  
3


```python
def set_bits(n):
    i = 0
    while n != 0:
        if n & 1 == 1:
            i += 1
        n = (n >> 1)
    return i


t = int(input())
for i in range(t):
    n = int(input())
    print(set_bits(n))
```

