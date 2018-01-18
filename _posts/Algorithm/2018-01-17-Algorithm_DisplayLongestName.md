---
layout: post
title:  "[School] Display longest name"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given a list of names, display the longest name.

Input: First line of input contains an integer T, the number of test cases. T test cases follows. First line of each test case contains an integer N, i.e total number of names. Next N lines contains names of different length.


Output: Longest name in the list of names.


Constraints:

1<=T<=10<br>
1<=N<=10<br>


Example:

Input:<br>
1<br>
5<br>
Geek<br>
Geeks<br>
Geeksfor<br>
GeeksforGeek<br>
GeeksforGeeks

Output:<br>
GeeksforGeeks

```python
def LongestName(arr):
    answer = ""
    for i in range(len(arr)):
        if len(answer) < len(arr[i]):
            answer = arr[i]
    return answer

t = int(input())
for i in range(0, t):
    n = int(input())
    arr = []
    for j in range(n):
        arr.append(input())
    print(LongestName(arr))
```