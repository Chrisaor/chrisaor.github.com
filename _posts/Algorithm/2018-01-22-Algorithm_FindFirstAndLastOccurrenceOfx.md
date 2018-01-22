---
layout: post
title:  "[Basic] Find first and last occurrence of x"
date:   2018-01-22 13:30:00 +0900
categories: Algorithm
---

Given a sorted array with possibly duplicate elements, the task is to find indexes of first and last occurrences of an element x in the given array.


Input:
The first line of input contains an integer T denoting the no of test cases. Then T test cases follow. Each test case contains an integer N denoting the size of the array. Then in the next line are N space separated values of the array. The last line of each test case contains an integer x.

Output:
For each test case in a new line print two integers separated by space denoting the first and last occurrence of the element x. If the element is not present in the array print -1.

Constraints:  
1<=T<=101  
1<=N<=100  
1<=A[],k<=100

Example:

```
Input : A[] = {1, 3, 5, 5, 5, 5 ,67, 123, 125}    
        x = 5
Output : First Occurrence = 2
         Last Occurrence = 5

Input : A[] = {1, 3, 5, 5, 5, 5 ,7, 123 ,125 }    
        x = 7
Output : First Occurrence = 6
         Last Occurrence = 6
```         
         
Input:  
2  
9  
1 3 5 5 5 5 67 123 125  
5  
9  
1 3 5 5 5 5 7 123 125  
7
  
Output:  
2 5  
6 6  

```python
def find_idx(arr, num):
    first_occ = -1
    last_occ = -1
    for i in range(len(arr)):
        if first_occ < 0 and arr[i] == num:
            first_occ = i
            last_occ = i
        elif arr[i] == num:
            last_occ = i
    result = [first_occ, last_occ]
    if first_occ == -1:
        return -1
    else:
        return ' '.join(map(str,result))
        
t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    num = int(input())
    print(find_idx(arr, num))
```

