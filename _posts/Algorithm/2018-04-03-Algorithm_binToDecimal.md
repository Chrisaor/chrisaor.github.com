---
layout: post
title:  "[Basic] Binary number to decimal number"
date:   2018-04-03 11:55:00 +0900
categories: Algorithm
---


Given a Binary Number, Print its decimal equivalent.

**Input:**

The first line of input contains an integer T denoting the number of test cases. The description of T test cases follows. Each test case contains a single Binary number. 

**Output:**

Print each Decimal number in new line.

**Constraints:**

1< T <100  
1<=Digits in Binary<=8

**Example:**  
**Input:**  
2  
10001000  
101100  

**Output:**  
136  
44

```python
def bin_to_decimal(bin_str):
    bin_str = bin_str[::-1]
    answer = 0
    for index, i in enumerate(bin_str):
        answer += int(i) * (2**index)
    return answer

t = int(input())
for i in range(t):
    bin_str = input()
    print(bin_to_decimal(bin_str))
```