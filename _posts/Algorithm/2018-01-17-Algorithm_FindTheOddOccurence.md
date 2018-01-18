---
layout: post
title:  "[Basic] Find the Odd Occurence"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given an array of positive integers. All numbers occur even number of times except one number which occurs odd number of times. Find the number.

Expected Time Complexity: O(n)<br>
Expected Auxiliary Space​: Constant

Input:

The first line of input contains a single integer T denoting the number of test cases. Then T test cases follow. Each test case consist of two lines.

The first line of each test case consists of an integer N, where N is the size of array.
The second line of each test case contains N space separated integers denoting array elements.

Output:

Corresponding to each test case, print in a new line, the number which occur odd number of times.

Constraints:

1 ≤ T ≤ 100<br>
1 ≤ N ≤ 202<br>
1 ≤ A[i] ≤ 1000

Example:

Input:<br>
1<br>
5<br>
8 4 4 8 23

Output:<br>
23

```python
t = int(input())

for i in range(t):
    n = int(input())
    numbers = [str(x) for x in input().split()]
    result = []
    for j in range(n):
        if numbers[j] not in result:
            result.append(numbers[j])
        else:
            result.remove(numbers[j])
    print(' '.join(result))
```