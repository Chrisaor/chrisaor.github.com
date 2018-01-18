---
layout: post
title:  "[Basic] Minimum difference pair"
date:   2018-01-17 11:30:00 +0900
categories: Algorithm
---

Given an unsorted array, find the minimum difference between any pair in given array.

Input:

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N, the size of array. Second line of the test case is the Array.

Output:

Print the minimum difference between any two pairs.

Constraints:

1 <= T <= 30<br>
1 < N <= 100<br>
1 <= arr[i] <= 100000

Example:<br>
Input:<br>
2<br>
5<br>
2 4 5 7 9<br>
10<br>
87 32 99 75 56 43 21 10 68 49

Output:<br>
1<br>
6

```python
def minimumDiff(arr):
    sortedArr = sorted(arr)
    answer = abs(sortedArr[0] - sortedArr[1])
    for i in range(len(sortedArr)-1):
        diff = abs(sortedArr[i] - sortedArr[i+1])
        if answer > diff:
            answer = diff
    return answer


#
t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(minimumDiff(arr))
```

