---
layout: post
title:  "[Medium] Maximum length Bitonic Subarray"
date:   2018-03-26 11:45:00 +0900
categories: Algorithm
---

Given an array A[0 … n-1] containing n positive integers, a subarray A[i … j] is bitonic if there is a k with i < k < j such that A[i] < A[i + 1] ... < A[k] > A[k + 1] > .. A[j – 1] >  A[j]. Write a program that returns the length of the maximum length bitonic subarray.

**For Example:** In array {20, 4, 1, 2, 3, 4, 2, 10} the maximum length bitonic subarray is {1, 2, 3, 4, 2} which is of length 5.

**Note:** A[k] can be first or last element. Ex:-  10 20 30 , 30 20 10 and 1 2 3 4 3 2 1 these all are bitonic array.

**Input:**

The first line of input contains an integer T denoting the number of test cases.
The first line of each test case is N,N is the size of array.
The second line of each test case contains N input A[i].

**Output:**

Print the maximum length of bitonic subarray.

**Constraints:**

1 ≤ T ≤ 200  
1 ≤ N ≤ 200  
1 ≤ A[i] ≤ 1000  

**Example:**  

**Input:**  
2  
6  
12 4 78 90 45 23  
4  
10 20 30 40

**Output:**  
5  
4  

```python
def bitnoic_subarr(arr):
    asc = 0
    des = 0
    temp = list()
    result = list()

    if len(arr) == 1:
        return 1
    elif len(arr) == 2:
        return 2
        
    for i in range(len(arr)):
        if len(temp) == 0:
            temp.append(arr[i])

        elif asc == 0 and des == 0:
            temp.append(arr[i])
            if temp[-1] < temp[-2]:
                des = 1
            elif temp[-1] > temp[-2]:
                asc = 1
            else:
                asc = 0
                des = 0

        elif asc == 1 and des == 0: # asc모드
            if temp[-1] > arr[i]:  # 낮은 원소 발견
                asc = 0 # asc모드 종료
                des = 1 # des모드 시작
                temp.append(arr[i]) # temp에 넣고
                if i == len(arr)-1:  # 마지막 원소이면
                    result.append(len(temp)) # temp길이를 result에 삽입
                    if len(result) == 0:
                        return 0
                    elif len(result) == 1:
                        return result[0]
                    else:
                        return max(result)
            elif temp[-1] < arr[i]:
                temp.append(arr[i])
                if i == len(arr)-1:  # 마지막 원소이면
                    result.append(len(temp)) # temp길이를 result에 삽입
                    if len(result) == 0:
                        return 0
                    elif len(result) == 1:
                        return result[0]
                    else:
                        return max(result)
            else:
                temp.append(arr[i])
                temp = temp[-1:]
                asc = 0
                des = 0

        elif asc == 0 and des == 1:
            if temp[-1] < arr[i]:
                result.append(len(temp))
                asc = 0
                des = 0
                temp = temp[-1:]

            if temp[-1] > arr[i]:
                temp.append(arr[i])
                if i == len(arr)-1:
                    result.append(len(temp))
                    if len(result) == 0:
                        return 0
                    elif len(result) == 1:
                        return result[0]
                    else:
                        return max(result)
            elif temp[-1] < arr[i]:
                temp.append(arr[i])
                if i == len(arr)-1:
                    result.append(len(temp))
                    if len(result) == 0:
                        return 0
                    elif len(result) == 1:
                        return result[0]
                    else:
                        return max(result)
            else:
                temp.append(arr[i])
                temp = temp[-1:]
                asc = 0
                des = 0

    if len(result) == 0:
        return 0
    else:
        return max(result)

t = int(input())
for i in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(bitnoic_subarr(arr))
```

첫 Medium레벨 성공!  
비효율적인 코드지만 성공을 축하!