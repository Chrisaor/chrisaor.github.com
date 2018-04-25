---
layout: post
title:  "[Basic] Remove common characters and concatenate"
date:   2018-04-25 12:15:00 +0900
categories: Algorithm
---

Two strings are given. Modify 1st string such that all the common characters of the 2nd strings have to be removed and the uncommon characters of the 2nd string have to be concatenated with uncommon characters of the 1st string.

**Note:**  
If the modified string is empty then print '-1'.

**Input:**  
The first line consists of an integer T i.e number of test cases. The first line of each test case consists of an string s1.The next line consists of an string s2. 

**Output:**  
Print the required output.

**Constraints:**  
1<=T<=100  
1<=|Length of Strings|<=100

**Example:**  
**Input:**  
2  
aacdb  
gafd  
abcs  
cxzca  

**Output:**  
cbgf  
bsxz

```python
def remove_common(str1, str2):
    list1 = list(str1)
    list2 = list(str2)
    temp = list()
    for i in list1:
        if i not in list2:
            temp.append(i)
    for i in list2:
        if i not in list1:
            temp.append(i)
    if len(temp) != 0:
        return ''.join(temp)
    else:
        return -1

t = int(input())
for i in range(t):
    str1 = input()
    str2 = input()
    print(remove_common(str1, str2))
```