---
layout: post
title:  "[Easy] Parenthesis Checker"
date:   2018-06-12 12:22:00 +0900
categories: Algorithm
---

Given an expression string exp, examine whether the pairs and the orders of “{“,”}”,”(“,”)”,”[“,”]” are correct in exp.
For example, the program should print 'balanced' for exp = “`[()]{}{[()()]()}` and `not balanced` for exp = `[(])`


**Input:**

The first line of input contains an integer T denoting the number of test cases.   
Each test case consist of a string of expression, in a separate line.

**Output:**

Print `balanced` without quotes if pair of parenthesis are balanced else print `not balanced` in a separate line.


**Constraints:**

1 ≤ T ≤ 100  
1 ≤ |s| ≤ 100


**Example:**

**Input:**  
3  
{([])}  
()  
()[]  

**Output:**    
balanced  
balanced  
balanced  

```python
def parenthesis_checker(exp_list):
    temp = list()
    for i in range(len(exp_list)):
        try: # 이유 모를 index error로 인해 추가
            if i == 0:
                temp.append(exp_list[i])
            elif exp_list[i] == ']' and temp[-1] == '[':
                temp.pop()
            elif exp_list[i] == '}' and temp[-1] == '{':
                temp.pop()
            elif exp_list[i] == ')' and temp[-1] == '(':
                temp.pop()
            else:
                temp.append(exp_list[i])
        except IndexError:
            pass

    if temp:
        return 'not balanced'
    return 'balanced'

t = int(input())
for i in range(t):
    exp_list = list(map(str, input().split()))
    print(parenthesis_checker(exp_list))
```