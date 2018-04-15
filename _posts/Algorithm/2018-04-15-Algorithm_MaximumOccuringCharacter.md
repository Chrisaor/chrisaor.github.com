---
layout: post
title:  "[Basic] Maximum Occuring Character"
date:   2018-04-15 12:05:00 +0900
categories: Algorithm
---

Given a string, find the maximum occurring character in the string. If more than one character occurs maximum number of time then print the lexicographically smaller character.

**Input:**

The first line of input contains an integer T denoting the number of test cases. Each test case consist of a string in 'lowercase' only in a separate line.

**Output:**

Print the lexicographically smaller character which occurred the maximum time.

**Constraints:**

1 ≤ T ≤ 30  
1 ≤ |s| ≤ 100

**Example:**

**Input:**  
2  
testsample  
geeksforgeeks

**Output:**  
e  
e

```python
def maximum_char(char1):
    count_list = list()
    temp = list()
    for i in char1:
        count_list.append(char1.count(i))

    max_count = max(count_list)
    for i in char1:
        if char1.count(i)== max_count:
            temp.append(i)
    return sorted(temp)[0]

t = int(input())
for i in range(t):
    char1 = input()
    char1 = list(char1)
    print(maximum_char(char1))
```

