---
layout: post
title:  "[Easy] Two numbers with sum closest to zero"
date:   2018-04-07 11:55:00 +0900
categories: Algorithm
---

Given an integer array, you need to find the two elements such that their sum is closest to zero.
 
**Input:**  
The first line of input contains an integer T denoting the number of test cases.    
The first line of each test case is N,the size of array  
Each test case consist of a N integers which are the array elements.
 
**Output:**  
Print the two numbers in ascending order such that their sum is closest to zero.
 
**Constraints:**  
1 ≤ T ≤ 100  
1 ≤ N ≤ 1000  
-100007 ≤ a[i] ≤ 100007  
 
**Example:**  
**Input**   
3  
3  
-8 -66 -60    
6  
-21 -67 -37 -18 4 -65    
2  
-24 -73    
 
**Output**  
-60 -8  
-18 4  
-73 -24  

```python
def closest_zero(arr):
    result = list()
    answer = list()
    sum = 0
    if len(arr) == 2:
        return ' '.join(list(map(str, arr)))
    for i in range(len(arr)):
        for j in range(i+1,len(arr)):
            sum = arr[i] + arr[j]
            result.append(abs(sum))
            if len(result) >= 2 and min(result) == abs(sum):
                answer = list()
                answer.append(str(arr[i]))
                answer.append(str(arr[j]))
    return ' '.join(sorted(answer))

t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(closest_zero(arr))
```


Time limit excess :(