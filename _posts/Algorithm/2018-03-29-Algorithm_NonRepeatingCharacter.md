---
layout: post
title:  "[Easy] Non Repeating Character"
date:   2018-03-29 11:45:00 +0900
categories: Algorithm
---

Given a string s consisting of lowercase Latin Letters, find the first non repeating character in s.

**Input:**

The first line contains T denoting the number of testcases. Then follows description of testcases.  
Each case begins with a single integer N denoting the length of string. The next line contains the string s.
 
**Output:**


 For each testcase, print the first non repeating character present in string.
 
 Print -1 if there is no non repeating character.
 
**Constraints:**


1<=T<=50  
1<=N<=100


**Example:**

**Input:**

3  
5    
hello  
12  
zxvczbtxyzvy  
6  
xxyyzz  

**Output:**
h  
c  
-1  


```python
def non_repeating_chr(str):
    list1 = list(str)
    for i in range(len(list1)):
        if list1.count(list1[i]) == 1:
            return list1[i]
    return -1

t = int(input())
for i in range(t):
    n = int(input())
    str = input()
    print(non_repeating_chr(str))
```