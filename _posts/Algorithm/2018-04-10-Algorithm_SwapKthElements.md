---
layout: post
title:  "[Basic] Swap kth elements"
date:   2018-04-10 11:55:00 +0900
categories: Algorithm
---

Given an array, swap kth element from beginning with kth element from end.

**Input:**

The first line of input contains an integer T denoting the number of test cases.  
The first line of each test case is N and k,N is the size of array and kth number.  
The second line of each test case contains N input C[i].

**Output:**  

Print the modified array.

**Constraints:**

1 ≤ T ≤ 100  
1 ≤ K ≤ N ≤ 500  
1 ≤ C[i] ≤ 1000

**Example:**

**Input**  
1  
8 3  
1 2 3 4 5 6 7 8

**Output**  
1 2 6 4 5 3 7 8

```python
def swap_kth(a, arr):
    temp = list()
    temp.append(arr[a-1])
    arr[a-1] = arr[-a]
    arr[-a] = temp[0]
    return ' '.join(map(str, arr))


t = int(input())
for i in range(t):
    N = list(map(int, input().split()))
    a = N[1]
    arr = list(map(int, input().split()))
    print(swap_kth(a,arr))
```