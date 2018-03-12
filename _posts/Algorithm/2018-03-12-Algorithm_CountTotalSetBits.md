---
layout: post
title:  "[Basic] Count total set bits"
date:   2018-03-12 11:20:00 +0900
categories: Algorithm
---

Find the sum of all bits from numbers 1 to N.

**Input:**

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N.

**Output:**

Print the sum of all bits.

**Constraints:**

1 ≤ T ≤ 100  
1 ≤ N ≤ 1000

**Example:**

**Input:**  
2  
4  
17  

**Output:**  
5  
35  

```python
def count_total(N):
    cnt = 0
    for i in range(1,N+1):
        cnt += bin(i)[2:].count('1')

    return cnt

t = int(input())

for i in range(t):
    N = int(input())
    print(count_total(N))
```