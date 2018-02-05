---
layout: post
title:  "[Easy] String comparison(Failed)"
date:   2018-02-05 10:15:00 +0900
categories: Algorithm
---

[https://practice.geeksforgeeks.org/problems/string-comparison/0/?ref=self](https://practice.geeksforgeeks.org/problems/string-comparison/0/?ref=self)

In a native language the increasing order of priority of characters is a, b, c, d, e, f, g, h, i, j, k, l, m, n, ’ng’ , o, p, q, r, s, t, u, v, w, x, y, z. You are given two strings string1 and string2 and your task is to compare them on the basis of the given priority order.

Print ‘0’ if both the strings are equal, ‘1’ if string1 is greater than string2 and ‘-1’ if string1 is lesser than string2. All the strings consist of lowercase English alphabets only.


**Input:**

The first line of the input contains a single integer T denoting the number of test cases. Each of the test case consists of a single line containing space separated two strings string1 and string2. 
 

**Output:**

For each test case, print the required output in a new line. 
 

**Constraints:**

1 <= T <= 1000

1 <= |string1, string2| <= 10^8 
 

**Example:**

**Input:**

3

adding addio

abcng abcno

abngc abngc

**Output:**

-1

1

0

**Explanation:**

Assume 0-based indexing.

For the **1st test case**:

The Strings differ at index = 4. Comparing ‘ng’ and ‘o’, we have string1 < string2.

For the **2nd test case**:

The Strings differ at index = 3. Comparing ‘ng’ and ‘n’, we have string1 > string2.

For the **3rd test case**:

Both the strings are same.


```python
import string
def string_comparison(str1, str2):

    str_ascii = string.ascii_lowercase
    str_list = [' ']

    for i in str_ascii:
        if i == 'n':
            str_list.append(i)
            str_list.append('ng')
        else:
            str_list.append(i)
    print(str_list)

    str1_cnt = 0
    str2_cnt = 0
    str1_list = list()
    str2_list = list()

    for i in range(len(str1)):
        if str1[i] == 'n' and i < len(str1)-1:
            if str1[i+1] == 'g':
                str1_list.append('ng')
            else:
                str1_list.append('n')
        elif str1[i] == 'g' and str1[i-1] == 'n':
            continue
        else:
            str1_list.append(str1[i])

    for i in range(len(str2)):
        if str2[i] == 'n' and i < len(str2)-1:
            if str2[i+1] == 'g':
                str2_list.append('ng')
            else:
                str2_list.append('n')
        elif str2[i] == 'g' and str2[i-1] == 'n':
            continue
        else:
            str2_list.append(str2[i])

    if len(str1_list) > len(str2_list):
        str2_list += [' '] * (len(str1_list) - len(str2_list))
    else:
        str1_list += [' '] * (len(str2_list) - len(str2_list))
    for a, b in zip(str1_list, str2_list):
        str1_cnt += str_list.index(a)
        str2_cnt += str_list.index(b)
    if str1_cnt > str2_cnt:
        return 1
    elif str2_cnt > str1_cnt:
        return -1
    return 0


t = int(input())
for i in range(t):
    str_input = list(input().split(' '))
    str1 = str_input[0]
    str2 = str_input[1]
    print(string_comparison(str1, str2))
```

**result**

```
Your program took more time than expected.  
Time Limit Exceeded
Expected Time Limit < 0.5sec  
Hint : Please optimize your code and submit again.
```