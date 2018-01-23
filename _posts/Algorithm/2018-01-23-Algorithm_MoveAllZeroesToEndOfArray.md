---
layout: post
title:  "[Easy] Move all zeroes to end of array"
date:   2018-01-23 13:50:00 +0900
categories: Algorithm
---

Given an array of random numbers, Push all the zero’s of a given array to the end of the array. For example, if the given arrays is {1, 9, 8, 4, 0, 0, 2, 7, 0, 6, 0}, it should be changed to {1, 9, 8, 4, 2, 7, 6, 0, 0, 0, 0}. 

Input:  
The first line contains an integer 'T' denoting the total number of test cases. In each test cases, First line is number of elements in array 'N' and second its values.

Output:  
Print the array after shifting all 0's at the end.​

Note: An extra space is expected at the end after output of every test case

Constraints:  
1 <= T <=100  
1 <= N <=1000  
0 <= a[i] <=100  

Example:  
Input:  
1  
5  
3 5 0 0 4  

Output:  
3 5 4 0 0

```python
def moveAllZeroes(arr1):
    temp = []
    for i in range(len(arr1)):
        if arr1[i] == 0:
            temp = arr1[i]
            arr1.remove(arr1[i])
            arr1.append(0)

    return ' '.join(map(str,arr1))

t = int(input())
for i in range(t):
    n = int(input())
    arr1 = list(map(int, input().split()))
    print(moveAllZeroes(arr1))
```


