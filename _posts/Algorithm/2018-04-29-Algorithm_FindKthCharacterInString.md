---
layout: post
title:  "[Basic] Find k-th character in string"
date:   2018-04-29 12:15:00 +0900
categories: Algorithm
---

Given a decimal number m. Convert it in binary string and apply n iterations, in each iteration 0 becomes 01 and 1 becomes 10. Find kth character in the string after nth iteration.

**Input:**  
The first line consists of an integer T i.e number of test cases. The first and only line of each test case consists of three integers m,k,n. 

**Output:**  
Print the required output.

**Constraints:**   
1<=T<=100  
1<=m<=50  
1<=n<=10  
0<=k<=|Length of final string|

**Example:**  
**Input:**  
2  
5 5 3  
11 6 4  

**Output:**  
1  
1

```python
def find_kth_char(m,k,n):
    bin_m = bin(m)[2:]
    temp = list()
    for i in range(n):
        for j in list(bin_m):
            if j == '0':
                temp.append('01')
            else:
                temp.append('10')
        bin_m = ''.join(temp)
        temp = list()
    return list(bin_m)[k]

t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    m = N[0]
    k = N[1]
    n = N[2]
    print(find_kth_char(m,k,n))

```