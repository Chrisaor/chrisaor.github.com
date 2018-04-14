---
layout: post
title:  "[Basic] Generate Binary Numbers"
date:   2018-04-14 12:00:00 +0900
categories: Algorithm
---

Given a number n, Write a program that generates and prints all binary numbers with decimal values from 1 to n.

**Input:**

The first line of input contains an integer T denoting the number of test cases.  
The first line of each test case is N.

**Output:**

Print all binary numbers with decimal values from 1 to n in a single line.

**Constraints:**

1 ≤ T ≤ 100  
1 ≤ N ≤ 500

**Example:**

**Input**  
2  
2  
5

**Output**  
1 10  
1 10 11 100 101

```python
def bin_nums(n):
    temp = list()
    for i in range(1, n+1):
        temp.append(bin(i)[2:])
    return ' '.join(temp)

t = int(input())
for i in range(t):
    n = int(input())
    print(bin_nums(n))
```