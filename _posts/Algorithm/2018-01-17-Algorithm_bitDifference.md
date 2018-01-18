---
layout: post
title:  "[Basic] Bit Difference"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

You are given two numbers A and B. Write a program to count number of bits needed to be flipped to convert A to B.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is a and b.

Output:

Print the number of bits needed to be flipped.

Constraints:

1 ≤ T ≤ 100<br>
1 ≤ a,b ≤ 1000

Example:

Input:<br>
1<br>
10 20

Output:<br>
4
 

Explanation:

A  = 1001001<br>
B  = 0010101<br>
No of bits need to flipped = set bit count i.e. 4


```python
def bitDiff(A,B):
    bitA = bin(A)[2:]
    bitB = bin(B)[2:]
    bitLen = abs(len(bitA)-len(bitB))
    answer = 0
    if len(bitB) >= len(bitA):
        bitA = '0'*bitLen + bitA
    else:
        bitB = '0'*bitLen + bitB
    for i in range(len(bitA)):
        if bitA[i] != bitB[i]:
            answer += 1
    return answer

t = int(input())
for i in range(t):
    n = list(map(int, input().split()))
    print(bitDiff(n[0],n[1]))
```    