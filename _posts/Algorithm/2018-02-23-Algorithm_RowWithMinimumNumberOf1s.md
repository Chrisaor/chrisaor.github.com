---
layout: post
title:  "[Basic] Row with minimum number of 1's"
date:   2018-02-23 10:15:00 +0900
categories: Algorithm
---

Determine the row index with minimum number of ones. The given 2D matrix has only zeroes and ones and also the matrix is sorted row wise . If two or more rows have same number of 1's than print the row with smallest index.

**Note:** If there is no '1' in any of the row than print '-1'.

**Input:**  
The first line of input contains an integer T denoting the number of test cases. The first line of each test case consists of two integer n and m. The next line consists of n*m spaced integers. 

**Output:**  
Print the index of the row with minimum number of 1's.

**Constraints:**   
1<=T<=200  
1<=n,m<=100  

**Example:**  
**Input:**  
2  
3 3  
0 0 0 0 0 0 0 0 0  
4 4  
0 0 0 1 0 1 1 1 0 0 1 1 0 0 1 1  

**Output:**  
-1  
0  

```python
def find_min1_low(m, n, arr):
    numbers_of_1 = list()
    if sum(arr) == 0:
        return -1
    for i in range(m):
        numbers_of_1.append(sum(arr[n*i:n*(i+1)]))
    print(numbers_of_1)
    min_sum = sum(numbers_of_1)
    for i in numbers_of_1:
        print(min_sum)
        if i == 0:
            continue
        elif i < min_sum:
            min_sum = i
    print('len',len(numbers_of_1))
    return numbers_of_1.index(min_sum)

t = int(input())
for i in range(t):
    NM = list(map(int, input().split()))
    m = NM[0]
    n = NM[1]
    arr = list(map(int, input().split()))
    print(find_min1_low(m, n, arr))
```