---
layout: post
title:  "[Easy] Minimize string value"
date:   2018-03-02 11:20:00 +0900
categories: Algorithm
---

Given a string of lowercase alphabets and a number k, the task is to print the minimum **value** of the string after removal of ‘k’ characters.  The **value** of a string is defined as sum of squares of count of each distinct character. For example consider the string “saideep”, here frequencies of characters are s-1, a-1, i-1,e-2, d-1, p-1 and value of the string is 1^2 + 1^2 + 1^2 + 1^2 + 1^1 + 2^2 = 9.

**Input:**  
The first line of input contains the number T denoting the no of test cases . Then T test cases follow. Each test case contains two lines.The first line of each test case contains a string str. The second line of each test case consist of an integer k .

**Output:**  
The output for each test case will be an integer denoting the min possible value of the string.

**Constraints:**  
1<=T<=100   
1<=str<=50  
1<=k<=50  

**Example:**  
**Input:**  
2  
abccc  
1  
aaab  
2  
**Output:**  
6  
2  

```python
import string

def minimize_string(str1, k):
    str_list = [i for i in str1]
    cnt_list = list()
    ascii_str = [i for i in string.ascii_lowercase]
    for i in ascii_str:
        if i in str1:
            cnt_list.append(str1.count(i))
    for i in range(k):
        if max(cnt_list) == 0:
            return 0
        else:
            cnt_list[cnt_list.index(max(cnt_list))] -= 1
    return sum([i**2 for i in cnt_list])

t = int(input())
for i in range(t):
    str1 = input()
    k = int(input())
    print(minimize_string(str1,k))
```