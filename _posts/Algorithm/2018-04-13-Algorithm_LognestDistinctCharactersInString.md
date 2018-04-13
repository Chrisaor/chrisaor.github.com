---
layout: post
title:  "[Easy] Longest Distinc characters in string"
date:   2018-04-13 11:55:00 +0900
categories: Algorithm
---
      
Given a string, find length of the longest substring with all distinct characters.  For example, for input "abca", the output is 3 as "abc" is the longest substring with all distinct characters.

**Input:**

The first line of input contains an integer T denoting the number of test cases.  
The first line of each test case is String str.

**Output:**

Print length of smallest substring with maximum number of distinct characters.  
Note: The output substring should have all distinct characters.

**Constraints:**  

1 ≤ T ≤ 100  
1 ≤ size of str ≤ 10000

**Example:**

**Input**  
2  
abababcdefababcdab  
geeksforgeeks  
 
**Output:**  
6  
7

```python
def longest_dchr(str1):
    temp = list()
    result = list()
    str1_list = list(str1)
    if len(set(str1_list)) == len(str1_list):
        return len(str1_list)
    for i in range(len(str1_list)):
        if str1[i] in temp:
            result.append(len(temp))
            temp = temp[temp.index(str1[i])+1:]
            temp.append(str1[i])
        else:
            temp.append(str1_list[i])
    result.append(len(temp))
    return max(result) if not len(result)==0 else 0

t = int(input())
for i in range(t):
    str1 = input()
    print(longest_dchr(str1))
```