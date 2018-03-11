---
layout: post
title:  "[Basic] Rotate by 90 degree"
date:   2018-03-11 11:20:00 +0900
categories: Algorithm
---

Given an square matrix, turn it by 90 degrees in anti-clockwise direction without using any extra space.

**Input:**

The first line of input contains a single integer T denoting the number of test cases. Then T test cases follow. Each test case consist of two lines. The first line of each test case consists of an integer N, where N is the size of the square matrix.The second line of each test case contains NxN space separated values of the matrix M.

**Output:**

Corresponding to each test case, in a new line, print the rotated array.

**Constraints:**

1 ≤ T ≤ 50  
1 ≤ N ≤ 50  

**Example:**

**Input:**  
1  
3  
1 2 3 4 5 6 7 8 9  

**Output:**  
3 6 9 2 5 8 1 4 7


```python
def rotate_by_90(N, arr):
    matrix = list()
    for i in range(N):
        matrix.append(arr[i*N:(i+1)*N])

    new_matrix = list()
    temp = list()

    for i in range(N):
        temp = []
        for j in range(N):
            temp.append(matrix[j][-i-1])
        new_matrix.append(temp)

    liner = list()
    for i in new_matrix:
        liner += i
    answer = ' '.join(map(str, liner))
    return answer

t = int(input())
for i in range(t):
    N = int(input())
    arr = list(map(int, input().split()))
    print(rotate_by_90(N,arr))
```