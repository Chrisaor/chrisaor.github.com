---
layout: post
title:  "[Easy] Three Great Candidates"
date:   2018-01-23 14:00:00 +0900
categories: Algorithm
---

The hiring team of Google aims to find 3 candidates who are great collectively. Each candidate has his or her ability expressed as an integer. 3 candidate are great collectively if product of their abilities is maximum. Find the maximum collective ability from the given pool of candidates.

Input:  
The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. First line of each test case conatins an interger N  denoting the number of candidates.  
The second line of each test case contains N space separated elements denoting the ablities of candidates.


Output:  
Corresponding to each test case, print the desired output(maximum collective ability of three candidates) in a newline.


Constraints:  
1 ≤ T ≤ 100  
3 ≤ N ≤ 1000  
-1000 ≤ ability ≤ 1000


Example:  
Input:  
1  
6  
0 -1 3 100 70 50  

Output:  
350000

Explanation:  
70\*50\*100 = 350000 which is the maximum possible.

```python
def three_great_candidates(arr):
    arr1 = sorted(arr)
    case1 = arr1[0]*arr1[1]*arr1[-1]
    case2 = arr1[-1]*arr1[-2]*arr1[-3]
    if case1 >= case2:
        return str(case1)
    else:
        return str(case2)


t = int(input())
for i in range(t):
    n = int(input());
    arr = list(map(int,input().split()))
    print(three_great_candidates(arr))
```

3개의 원소를 뽑아 곱했을 때 최대값이 나오게 만드는 문제, 처음에 젤 큰 값 3개만 뽑으면 되네 하고 30초만에 풀었다가 다시 보고 아~ 했던 문제..

음수가 2개와 양수가 곱해지는 경우를 생각해야했다.

그래서 sort를 하고 가장 큰 3개의 값을 곱하거나 앞 2개(두 수가 음수일 경우 곱할 때 양수로 바뀜), 끝 1개의 값을 곱하여 그중 큰 수를 뽑으면 된다.

직관적으로 풀었는데 딱히 더 좋은 방법을 못찾겠다.