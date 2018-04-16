---
layout: post
title:  "[Basic] Uncommon characters"
date:   2018-04-16 12:05:00 +0900
categories: Algorithm
---

  
Find and print the uncommon characters of the two given strings. Here uncommon character means that either the character is present in one string or it is present in other string but not in both. The strings contain only lowercase characters and can contain duplicates.

**Input:**  
The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case contains two strings.

**Output:**  
Print the uncommon characters of the two given strings in sorted order.

**Constraints:**  
1<=T<=10^5  
1<=length of two   strings<=10^5  

**Example:**  
**Input:**  
1  
characters  
alphabets  

**Output:**  
bclpr  

```python
def uncommon_chr(str1, str2):
    temp = list()
    for i in list(str1):
        if i not in list(str2):
            temp.append(i)
    for j in list(str2):
        if j not in list(str1):
            temp.append(j)
    return ''.join(sorted(set(temp)))

t = int(input())
for i in range(t):
    str1 = input()
    str2 = input()
    print(uncommon_chr(str1,str2))
```
