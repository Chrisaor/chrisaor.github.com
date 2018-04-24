---
layout: post
title:  "[Basic] Twice counter"
date:   2018-04-24 12:10:00 +0900
categories: Algorithm
---

Given an array of n words. Some words are repeated twice, we need count such words.

**Input:**  
The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case contains an integer n denoting the number of words in the string. The next line contains n space separated words forming the string.

**Output:**  
Print the count of the words which are repeated twice in the string.

**Constraints:**  
1<=T<=10^5  
1<=no of words<=10^5  
1<=length of each word<=10^5  

**Example:**  
**Input:**  
2  
10  
hate love peace love peace hate love peace love peace  
8  
Tom Jerry Thomas Tom Jerry Courage Tom Courage

Output:  
1  
2

```python
def twice_counter(list1):
    temp = list()
    cnt = 0
    for i in range(len(list1)):
        if list1.count(list1[i]) == 2:
            temp.append(list1[i])
    return len(set(temp))

t = int(input())
for i in range(t):
    n = input()
    list1 = list(input().split(' '))
    print(twice_counter(list1))
```
