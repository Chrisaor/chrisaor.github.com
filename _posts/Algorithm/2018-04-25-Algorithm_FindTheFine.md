---
layout: post
title:  "[Basic] Find the fine"
date:   2018-04-25 12:10:00 +0900
categories: Algorithm
---

Given an array of penalties, an array of car numbers and also the date. The task is to find the total fine which will be collected on the given date. Fine is collected from odd-numbered cars on even dates and vice versa.

**Input:**
The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case consists of three lines. First line of each test case contains two integers N & Date, second line contains N space separated car numbers and third line contains N space separated penalties.

**Output:**
For each test case,In new line print the total fine.

**Constraints:**  
1<=T<=100  
1<=N<=105  
1000<= Car num<=9999  
1<=Date<=31  
1<=penalty[i]<=10<sup>3</sup>  
Example:  
Input:  
2  
4 12  
2375 7682 2325 2352  
250 500 350 200  
3 8  
2222 2223 2224  
200 300 400  
**Output:**  
600  
300  

```python
def find_the_fine(date, num, fine):
    sum_fine = 0
    for i in range(len(num)):
        if date % 2 == 0:
            if num[i] % 2 == 1:
                sum_fine += fine[i]
        else:
            if num[i] % 2 == 0:
                sum_fine += fine[i]

    return sum_fine

t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    date = N[1]
    num = list(map(int, input().split()))
    fine = list(map(int, input().split()))
    print(find_the_fine(date,num,fine))
```
