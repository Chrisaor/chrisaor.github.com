---
layout: post
title:  "[Basic] Sort the array"
date:   2018-01-21 13:30:00 +0900
categories: Algorithm
---

Given a random set of numbers, Print them in sorted order.

Input:  
The first line of input contains an integer T denoting the number of test cases. The description of T test cases follows. The first line of each test case contains a single integer N denoting the size of array. The second line contains N space-separated integers A1, A2, ..., AN denoting the elements of the array.

Output:  
Print each sorted array in a seperate line. For each array its numbers should be seperated by space.

Constraints:  
1 ≤ T ≤ 10  
1 ≤ N ≤ 1000  
1 ≤A[i]<100  

Example:  
Input:  
1  
2  
3 1  

Output:  
1 3

```python
t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    sortedarr = sorted(arr)
    answer = ' '.join(map(str, sortedarr))
    print(answer)
```

배운점 : gfg에서는 모든 답을 문자열로 출력해야 하는데 int원소 리스트를 str로 바꾸는 쉬운방법을 stack overflow에서 찾았다.
  
다음과 같이 사용하면 된다.

`answer = ' '.join(map(str, sortedarr))`