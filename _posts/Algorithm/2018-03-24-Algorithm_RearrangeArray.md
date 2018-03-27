---
layout: post
title:  "[Basic] Rearrange Array Alternately"
date:   2018-03-23 11:45:00 +0900
categories: Algorithm
---

Given a sorted array, rearrange  the array alternately i.e first element should be max value, second min value, third second max, fourth second min and so on. Eg: arr[] = {1, 2, 3, 4, 5, 6, 7} O/P: {7, 1, 6, 2, 5, 3, 4} 

**Input:**  
First line of input ia the number of test cases T. First line of test case contain the array size 'N' and second line of test case contain the array.


**Output:**  
Numbers in the required form are displayed to the user.


**Constraints:**  
1 <=T<= 30  
1 <=N<= 100  
1 <=arr[i]<= 1000  


**Example:**

**Input:**  
2  
6  
1 2 3 4 5 6  
11   
10 20 30 40 50 60 70 80 90 100 110

**Output:**  
6 1 5 2 4 3  
110 10 100 20 90 30 80 40 70 50 60  


```python
def find_all_pairs(A,B,x):
    temp = list()
    answer = list()
    for i in range(len(A)):
        for j in range(len(B)):
            if (A[i]+B[j]) == x:
                temp.append(A[i])
                temp.append(B[j])
                answer.append(temp)
                temp = list()
    if len(answer) == 0:
        return -1
    else:
        answer = sorted(answer)
        for i in range(len(answer)):
            answer[i] = list(map(str, answer[i]))
        return ', '.join([' '.join(i) for i in answer])


t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    x = N[2]
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    print(find_all_pairs(A,B,x))
```