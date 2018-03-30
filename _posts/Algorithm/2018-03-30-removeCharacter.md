---
layout: post
title:  "[Basic] Remove character"
date:   2018-03-30 11:55:00 +0900
categories: Algorithm
---

Given two strings s1 & s2, remove those characters from first string which are present in second string. Both the strings are different and contain only lowercase characters.

**Input:**

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is s1,s1 is first string.
The second line of each test case contains s2,s2 is second string.

**Output:**

Print the modified string(s1). For each test case, print the output in a new line.

**Constraints:**

1 ≤ T ≤ 15
1 ≤ s2 < s1 ≤ 50

**Example:**

**Input:**  
2  
geeksforgeeks  
mask  
removeccharaterfrom  
string  

**Output:**  
geeforgee  
emovecchaaefom


```python
def remove_chr(str1, str2):
    list1 = list(str1)
    list2 = list(str2)
    result = list()
    for i in range(len(list1)):
        for j in range(len(list2)):
            if list1[i] not in list2:
                result.append(list1[i])
                break;
   return ''.join(result)

t = int(input())
for i in range(t):
    str1 = input()
    str2 = input()
    print(remove_chr(str1, str2))
```
