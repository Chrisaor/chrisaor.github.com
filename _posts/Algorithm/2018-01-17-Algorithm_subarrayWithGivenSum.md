---
layout: post
title:  "[Easy] Subarray with given sum"
date:   2018-01-18 11:30:00 +0900
categories: Algorithm
---

Given an unsorted array of non-negative integers, find a continuous sub-array which adds to a given number.

Input:

The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case consists of two lines. The first line of each test case is N and S, where N is the size of array and S is the sum. The second line of each test case contains N space separated integers denoting the array elements.

Output:

Corresponding to each test case, in a new line, print the starting and ending positions of first such occuring subarray if sum equals to subarray, else print -1.

Note: Position of 1st element of the array should be considered as 1.

Expected Time Complexity: O(n)

Constraints:
1 ≤ T ≤ 100<br>
1 ≤ N ≤ 100<br>
1 ≤ an array element ≤ 200

Example:

Input:<br>
2<br>
5 12<br>
1 2 3 7 5<br>
10 15<br>
1 2 3 4 5 6 7 8 9 10

Output:<br>
2 4<br>
1 5

Explanation : 
In first test case, sum of elements from 2nd position to 4th position is 12<br>
In second test case, sum of elements from 1st position to 5th position is 15

```python
def subarraySum(sum_result, arr):
    answer = []
    result = ""
    for i in range(len(arr)):
        temp = []
        for j in range(i,len(arr)):
            temp.append(arr[j])
            if sum(temp) == sum_result:
                answer.append(str(i+1))
                answer.append(str(j+1))
                result = " ".join(answer)
                return result
            elif sum(temp) > sum_result:
                break;
    return -1
    
t = int(input())
for i in range(t):
    n = list(map(int, input().split()))
    sum_result = n[1]
    arr = list(map(int, input().split()))
    print(subarraySum(sum_result, arr))
```

시간 복잡도 O(n)이 아니라면 그다지 어려운 문제는 아닌데...O(n)으로 맞추기 위해 어떻게 풀어야 할지 감이 안온다 :(