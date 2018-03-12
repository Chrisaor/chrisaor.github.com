---
layout: post
title:  "[Basic] Remaining String"
date:   2018-03-12 13:30:00 +0900
categories: Algorithm
---

Given a string without spaces, a character, and a count, the task is to print the string after the specified character has occurred count number of times.  
Print “Empty string” incase of any unsatisfying conditions.  
(Given character is not present, or present but less than given count, or given count completes on last index).  
If given count is 0, then given character doesn’t matter, just print the whole string.

 

**Input:**

First line consists of T test cases. First line of every test case consists of String S.Second line of every test case consists of a character.Third line of every test case consists of an integer.


**Output:**

Single line output, print the remaining string or "Empty string".


**Constraints:**

1<=T<=200  
1<=|String|<=10000


**Example:**

**Input:**

2   
Thisisdemostring  
i     
3​  
geeksforgeeks  
e  
2  

**Output:**  
ng  
ksforgeeks  

```python
def remaining_string(str1, key, num):
    str_list = list(str1)
    if num == 0:
        return str1

    try:
        for i in range(num-1):
            str_list.remove(key)
    except ValueError:
        return 'Empty string'

    if key in str_list:
        if str_list.index(key) == len(str_list)-1:
            return 'Empty string'
        else:
            return ''.join(str_list[str_list.index(key)+1:])
    else:
        return 'Empty string'
        
t = int(input())
for i in range(t):
    str1 = str(input())
    key = str(input())
    num = int(input())
    print(remaining_string(str1, key, num))
```
