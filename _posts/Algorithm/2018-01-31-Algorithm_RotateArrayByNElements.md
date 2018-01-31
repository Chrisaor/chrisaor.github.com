---
layout: post
title:  "[Basic] Rotate Array by n elements"
date:   2018-01-31 15:05:00 +0900
categories: Algorithm
---

Given an array of size n, rotate it by d elements. 

**Input:** The first line of the input contains T denoting the number of testcases. First line of test case is the number of elements 'n' and elements 'd' to be rotated. Second line of test case will be the array elements.  
**Output:** Rotated array is displayed to the user.


**Constraints:**

1 <=T<= 50  
1 <=n<= 100  
d<=n    
1 <=arr[i]<= 100  


**Example:**

**Input:**  
2  
5 2  
1 2 3 4 5   
10 3  
2 4 6 8 10 12 14 16 18 20  

**Output:**  
3 4 5 1 2  
8 10 12 14 16 18 20 2 4 6 

```python
def rotate_array(n, arr):
    temp = arr[:n]
    arr = arr[n:]
    answer = arr+temp
    return ' '.join(map(str, answer))

t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    n = N[1]
    arr = list(map(int, input().split()))
    print(rotate_array(n, arr))
```
 