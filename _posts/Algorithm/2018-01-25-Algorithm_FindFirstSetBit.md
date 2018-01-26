---
layout: post
title:  "[Basic] Find first set bit"
date:   2018-01-26 10:05:00 +0900
categories: Algorithm
---

Given an integer an N, write a program to print the position of first set bit found from right side in the binary representation of the number.

Input:  
The first line of the input contains an integer T, denoting the number of test cases. Then T test cases follow. The only line of the each test case contains an integer N.

Output:  
For each test case print in a single line an integer denoting the position of the first set bit found form right side of the binary representation of the number.

Constraints:  
1<=T<=200  
0<=N<=106  

Example:  
Input:  
2  
18  
12  

Output:  
2  
3  

Explanation:  
Test case 1:  
Binary representation of the 18 is 010010, the first set bit from the right side is at position 2.

```python
def find_first_bit(n):
    bin_str = bin(n)[2:]
    bin_num = int(bin_str)
    for i in range(len(bin_str)):

        if bin_num == 0:
            return 0
        elif bin_num % 10 == 1:
            return i+1
        bin_num = bin_num // 10

t = int(input())
for i in range(t):
    n = int(input())
    print(find_first_bit(n))
```

처음에 bin\_num /= 10 이렇게 썼더니 float형이 되어서 10의 지수형태로 출력이 되서 자꾸 오류가 났다. 10분동안 뻘짓하고 bin\_num = bin\_num //10 으로 바꿨더니 해결...