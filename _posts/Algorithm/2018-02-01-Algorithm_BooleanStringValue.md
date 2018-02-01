---
layout: post
title:  "[Basic] Sum of two large Numbers"
date:   2018-02-01 15:15:00 +0900
categories: Algorithm
---

Given a string consisting of only 0, 1, A, B, C where  
A = AND
B = OR
C = XOR
Calculate the value of the string assuming no order of precedence and evaluation is done from left to right.

**Input:**

The first line of input contains an integer T denoting the number of test cases.
The next T lines contains T strings.

**Output:**

Print the value of the string assuming no order of precedence and evaluation is done from left to right

**Constraints:**  
1 ≤ T ≤ 100
1 ≤ length(string)<=1000


**Examples:**  
**Input :**  
1  
1A0B1

**Output : 1**   
1 AND 0 OR 1 = 1

**Input :**  
2  
1C1B1B0A0  
1A0B1  

**Output :**   
0
1

```python
def boolean_string(bool_string):
    calc = list()
    for i in bool_string:
        calc.append(i)

    while len(calc) != 1:
        if calc[0] == '0':
            X = 0
        else:
            X = 1

        if calc[2] == '0':
            Y = 0
        else:
            Y = 1

        if calc[1] == 'A':
            calc[:3] = [str(X&Y)]
        elif calc[1] == 'B':
            calc[:3] = [str(X|Y)]
        else:
            calc[:3] = [str(X^Y)]

    return str(calc[0])

t = int(input())
for i in range(t):
    bool_string = input()
    print(boolean_string(bool_string))
```

뭔가 더 깔끔한 방법이 있지 않을까...?

그리고 파이썬에서는 X and Y, X or Y를 써도 된다는 걸 깜빡했다.