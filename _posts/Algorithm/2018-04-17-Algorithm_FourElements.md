---
layout: post
title:  "[Basic] Four Elements"
date:   2018-04-17 12:05:00 +0900
categories: Algorithm
---

Given an array of integers, find a combination of four elements in the array whose sum is equal to a given value X.

**Input:**  
First line consists of T test cases. First line of every test case consists of an integer N, denoting the number of elements in an array. Second line consists of N spaced array elements. Third line of every test case X.

**Output:**  
Single line output, print 1 if combination is found else 0.

**Constraints:**  
1<=T<=100  
1<=N<=100  

**Example:**  
**Input:**  
1  
6  
1 5 1 0 6 0  
7  
**Output:**  
1

```python
from itertools import permutations

def four_elements(arr, n):
    perm = permutations(arr,4)
    for i in perm:
        if sum(i) == n:
            return 1
    return 0

t = int(input())
for i in range(t):
    N = input()
    arr = list(map(int, input().split()))
    n = int(input())
    print(four_elements(arr,n))
```

결과값은 나오나 제출 실패


```
# 제출시 에러 메세지
Runtime Error:
Runtime ErrorTraceback (most recent call last):
  File "/home/a5333f5a51affe9299848086f95b815c.py", line 13, in <module>
    arr = list(map(int, input().split()))
ValueError: invalid literal for int() with base 10: 'Z;C\\h9NXH\\Ns,Rz{2fL4&^kz5KG^0TQ4nsn$.g[TE+qw\'nt7"jq&s_"*To2bnat*V0,/ue-<:}6?9T}aApe6y1fO#v[;%QCZ`M3#]>>uez_|P*_;DnO@"_9KWsd[n)_P|pR[0:{t6(>00HJR`C<,%TU&qc+a64\\]&X:]<_{xec*?Uz<8hVBkTB;GOmSd#YC(3\'
```
