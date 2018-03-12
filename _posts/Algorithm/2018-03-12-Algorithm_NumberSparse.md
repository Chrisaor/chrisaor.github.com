---
layout: post
title:  "[Basic] Number is sparse or not"
date:   2018-03-12 11:35:00 +0900
categories: Algorithm
---

Given a number, check whether it is sparse or not. A number is said to be a sparse number if in binary representation of the number no two or more consecutive bits are set.

**Input:**  
The first line of input contains an integer T denoting the number of test cases. The first line of each test case is number 'N'.

**Output:**  
Print '1' if the number is sparse and '0' if the number is not sparse.

**Constraints:**  
1 <=T<= 100  
1 <=n<= 100

**Example:**  
**Input:**  
4  
72  
12  
2  
3  

**Output:**  
1  
0  
1  
0  

```python
def number_sparse(N):
    cnt1 = 0
    while N > 0:
        if N % 2 == 1:
            if cnt1 == 1:
                return 0
            else:
                cnt1 += 1
        else:
            cnt1 = 0
        N = N >> 1
    return 1
    
t = int(input())
for i in range(t):
    N = int(input())
    print(number_sparse(N))
```