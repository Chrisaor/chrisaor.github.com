---
layout: post
title:  "[Basic] Count the numbers "
date:   2018-03-25 11:45:00 +0900
categories: Algorithm
---

  
Given a number N, count the numbers from 1 to N which comprise of digits, **only** in set 1, 2, 3, 4 and 5. 

**Input:**

The first line of input contains an integer T denoting the number of test cases.Then T test cases follow. Each test case consist of one line. Each line of each test case is N, where N is the range from 1 to N.

**Output:**

Print the count of numbers in the given range from 1 to N.

**Constraints:**

1 ≤ T ≤ 100  
1 ≤ N ≤ 1000

**Example:**

**Input:**  
2  
100  
10  
**Output:**  
30  
5
 

**Explanation:**  

When `n` is `20` then answer is `10` because `1 2 3 4 5 11 12 13 14 15` are only in given set. `16` is not beause `6` is not in given set, only `1 2 3 4 5` in set.


```python
def count_numbers(N):
    cnt = 0
    num_list = list('12345')
    for i in range(1,N+1):
        if set(str(i)) < set(num_list):
            cnt += 1
    return cnt

t = int(input())
for i in range(t):
    N = int(input())
    print(count_numbers(N))
```




