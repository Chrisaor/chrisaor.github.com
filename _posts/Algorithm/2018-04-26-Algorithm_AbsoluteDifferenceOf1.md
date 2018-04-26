---
layout: post
title:  "[Basic] Absolute Difference of 1"
date:   2018-04-26 12:15:00 +0900
categories: Algorithm
---

Given an array.Print all numbers less than k in the array, with the fact that absolute difference between any adjacent digits should be 1.

**Note:** Print '-1' if no number if valid.

**Input:**
The first line consists of an integer T i.e number of test cases. The first line of each test case consists of two integers n and k.The next line consists of n spaced integers. 

**Output:**
Print the required output.

**Constraints:**   
1<=T<=100  
1<=n<=103  
1<=k,a[i]<=104

**Example:**  
**Input:**  
2  
8 54  
7 98 56 43 45 23 12 8  
10 1000  
87 89 45 235 465 765 123 987   499 655  

**Output:**  
7 43 45 23 12 8   
87 89 45 765 123 987 

```python
def absolute_difference1(n, arr):
    temp = list()
    result = list()
    for i in arr:
        if i < n:
            temp.append(i)
    for i in temp:
        element = -1
        dif = 0
        k = 0
        for j in range(len(list(str(i)))):
            if element == -1:
                element = int(list(str(i))[j])
            else:
                dif = abs(element - int(list(str(i))[j]))
                element = int(list(str(i))[j])
                if dif != 1:
                    break;
            if dif == 1 and j == len(list(str(i)))-1:
                result.append(i)
            elif i // 10 == 0:
                result.append(i)
    if len(result) != 0:
        return ' '.join(map(str, result))
    else:
        return -1
        
t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    n = N[1]
    arr = list(map(int, input().split()))
    print(absolute_difference1(n,arr))
```
