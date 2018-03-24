---
layout: post
title:  "[Basic]  "
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
def rearrange_array(arr):
    rearray = list()
    for i in range(len(arr)):
        if i % 2 == 0:
            num = arr.pop()
            rearray.append(num)
        else:
            num = arr[0]
            del arr[0]
            rearray.append(num)
    return ' '.join(map(str, rearray))


t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(rearrange_array(arr))
```