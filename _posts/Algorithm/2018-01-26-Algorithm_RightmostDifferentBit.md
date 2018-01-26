---
layout: post
title:  "[Basic] Rightmost different bit"
date:   2018-01-26 13:05:00 +0900
categories: Algorithm
---

Given two numbers **M** and **N**. Write a program to find the position of rightmost different bit in binary representation of numbers.

**Input:**  
First line of input contains a single integer T which denotes the number of test cases. T test cases follows. First line of each test case contains two space separated integers M and N.

**Output:**
For each test case print the position of rightmost different bit in binary representation of numbers. If both M and N are same then print -1 in this case.

**Constraints:**  
1<=T<=100  
1<=M<=1000  
1<=N<=1000  

**Example:
Input:**  
2  
11 9  
52 4  
**Output:**  
2  
5  

```python
def rightmost_different_bit(m,n):
    xor_result = bin(m^n)[2:]
    for i in range(1,len(xor_result)+1):
        if xor_result[-i] == '1':
            return i
	return -1



t = int(input())
for i in range(t):
    arr = list(map(int, input().split()))
    m = arr[0]
    n = arr[1]
    print(rightmost_different_bit(m,n))
```

