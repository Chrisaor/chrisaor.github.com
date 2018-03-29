---
layout: post
title:  "[Easy] Anagram"
date:   2018-03-29 11:56:00 +0900
categories: Algorithm
---

Given two strings, check whether two given strings are anagram of each other or not. An anagram of a string is another string that contains same characters, only the order of characters can be different. For example, “act” and “tac” are anagram of each other.

**Input:**

The first line of input contains an integer T denoting the number of test cases. Each test case consist of two strings in 'lowercase' only, in a separate line.

**Output:**

Print "YES" without quotes if the two strings are anagram else print "NO".

**Constraints:**

1 ≤ T ≤ 30

1 ≤ |s| ≤ 100

**Example:**

**Input:**
2  
geeksforgeeks  
forgeeksgeeks  
allergy  
allergic  

**Output:**  
YES  
NO  

```python
def anagram(str1, str2):
    str1 = sorted(list(str1))
    str2 = sorted(list(str2))
    if str1 == str2:
        return "YES"
    else:
        return "NO"

t = int(input())
for i in range(t):
    str1 = input()
    str2 = input()
    print(anagram(str1,str2))
```


