---
layout: post
title:  "[Easy] Count Substrings"
date:   2018-01-23 13:45:00 +0900
categories: Algorithm
---


Given a binary string, count number of substrings that start and end with 1. For example, if the input string is “00100101”, then there are three substrings “1001”, “100101” and “101”.

Input:  
The first line contains T denoting the number of testcases. Then follows description of testcases. 
Each case contains a string containing 0's and 1's.
 

Output:  
For each test case, output a single line denoting number of substrings possible.

Constraints:  
1<=T<=100  
1<=Lenght of String<=100  


Example:  
Input:  
1  
10101  

Output:  
3  


```python
def count_substrings(str1):
    answer = 0
    for i in range(len(str1)):
        for j in range(i+1,len(str1)):
            if str1[i] == '1':
                if str1[j] == '1':
                    answer += 1
    return str(answer)

t = int(input())
for i in range(t):
    str1 = input()
    print(count_substrings(str1))
```


