---
layout: post
title:  "[Basic] Closest Number"
date:   2018-02-02 10:15:00 +0900
categories: Algorithm
---


Given two integers n and m. The problem is to find the number closest to n and divisible by m. If there are more than one such number, then output the one having maximum absolute value. If n is completely divisible by m(not equal to 0), then output n only. Time complexity of O(1) is required.

**Input:**  
The first line consists of an integer T i.e number of test cases. The first and only line of each test case contains two space separated integers n and m.

**Output:**  
Print the closest number to n which is also divisible by m.

**Constraints:**   
1<=T<=100  
-1000<=n<=1000  

**Example:**  
**Input:**  
2  
13 4  
-15 6  

**Output:**  
12  
-18  

```python
def closet_number(n,m):
    answer = 0
    if n*m == 0:
        return 0
    elif n*m > 0:
        if (n/m - n//m) >= 0.5:
            answer = (m * (n//m+1))
        else:
            answer = (m * (n//m))
    else:
        if abs((n/m - (n//m+1))) >= 0.5:
            answer = (m * (n//m))
        else:
            answer = (m * (n//m+1))
    return answer

t = int(input())
for i in range(t):
    num = list(map(int, input().split()))
    n = num[0]
    m = num[1]
    print(closet_number(n,m))
```