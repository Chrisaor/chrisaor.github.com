---
layout: post
title:  "[Basic] Keypad typing"
date:   2018-04-14 12:05:00 +0900
categories: Algorithm
---

You are given N strings of alphabet characters and the task is to find their matching decimal representation as on the shown keypad. Output the decimal representation corresponding to the string. For ex: if you are given “amazon” then its corresponding decimal representation will be 262966.

![img](https://contribute.geeksforgeeks.org/wp-content/uploads/Phone.png)


**Input:**

The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case consists of a single line containing a string.

**Output:**

For each test case, print in a new line, the corresponding decimal representation of the given string.

**Constraints:**

1 ≤ T ≤ 100  
1 ≤ length of String ≤ 100

**Example:**

**Input**  
2  
geeksforgeeks  
geeksquiz  

**Output**  
4335736743357  
433577849

```python
def keypad_typing(str1):
    dic1 = {
    'A' : '2','B' : '2','C' : '2','D' : '3','E' : '3','F' : '3',
    'G' : '4','H' : '4','I' : '4','J' : '5','K' : '5','L' : '5',
    'M' : '6','N' : '6','O' : '6','P' : '7','Q' : '7','R' : '7',
    'S' : '7','T' : '8','U' : '8','V' : '8','W' : '9','X' : '9',
    'Y' : '9','Z' : '9',
    'a' : '2','b' : '2','c' : '2','d' : '3','e' : '3','f' : '3',
    'g' : '4','h' : '4','i' : '4','j' : '5','k' : '5','l' : '5',
    'm' : '6','n' : '6','o' : '6','p' : '7','q' : '7','r' : '7',
    's' : '7','t' : '8','u' : '8','v' : '8','w' : '9','x' : '9',
    'y' : '9','z' : '9',
    }
    answer = ''
    for i in str1:
        answer += dic1[i]
    return answer

t = int(input())
for i in range(t):
    str1 = input()
    print(keypad_typing(str1))
```