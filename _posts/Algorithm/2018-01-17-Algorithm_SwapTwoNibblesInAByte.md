---
layout: post
title:  "[Basic] Swap two nibbles in a byte"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given a byte, swap the two nibbles in it. For example 100 is be represented as 01100100 in a byte (or 8 bits). The two nibbles are (0110) and (0100). If we swap the two nibbles, we get 01000110 which is 70 in decimal.

Input:

The first line contains 'T' denoting the number of testcases. Each testcase contains a single positive integer X.


Output:

In each separate line print the result after swapping the nibbles.


Constraints:

1 ≤ T ≤ 70<br>
1 ≤ X ≤ 255


Example:

Input:

2<br>
100<br>
129<br>

Output:

70<br>
24<br>


```python
def swapNibbles(n):
    lowerNib = n&15
    upperNib = n >> 4
    answer = lowerNib*16 + upperNib
    return answer



t = int(input())
for i in range(t):
    n = int(input())
    print(swapNibbles(n))
```
