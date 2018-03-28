---
layout: post
title:  "[Basic] Count the elements"
date:   2018-03-28 11:45:00 +0900
categories: Algorithm
---

Given two unsorted arrays A, B. They can contain duplicates. For each element in A , count elements less than or equal to it in array B .

**Time Complexity: O(n)**

**Input:**  
The first line contains a single integer T i.e. the number of test cases. The first line of each test case consists of a integer N. The second and third line of each test case consists of N spaced integers representing array A and array B respectively. 

**Output:**
In one line for each element in array A print the elements less than or equal to it in array B with a comma ',' in between.

**Constraints:**  
1<=T<=100  
1<=N<=100  

**Example:**  
**Input:**  
2  
6  
1 2 3 4 7 9  
0 1 2 1 1 4  
7  
95 39 49 20 67 26 63   
77 96 81 65 60 36 55  

**Output:**  
4,5,5,6,6,6  
6,1,1,0,4,0,3  

```python
def count_elements(arr1, arr2):
    result = list()
    for i in range(len(arr1)):
        cnt = 0
        for j in range(len(arr2)):
            if arr1[i]>=arr2[j]:
                cnt += 1
        result.append(str(cnt))

    return ','.join(result)

t = int(input())
for i in range(t):
    N = int(input())
    arr1 = list(map(int, input().split()))
    arr2 = list(map(int, input().split()))
    print(count_elements(arr1,arr2))
```


Time Complexity: O(n)을 지키지 않아서 나중에 다시 도전