---
layout: post
title:  "[Array] Python Sort, Search"
date:   2018-06-30 12:20:00 +0900
categories: datastructure
---

배열을 정렬하는 것은 배열 안에 있는 원소들을 정해진 규칙에 따라서 새로 늘어놓는 작업을 말한다.

파이썬에서는 배열(리스트)을 정렬하는 방법이 이미 제공하고 있다.

(1) sorted()

- 내장 함수(built-in function)
- 정렬된 새로운 리스트를 얻어냄


```python
>>> L = [3, 8, 2, 7, 6, 10, 9]
>>> L2 = sorted(L)
>>> L2
[2, 3, 6, 7, 8, 9, 10]
```

(2) sort()

- 리스트의 메서드
- 해당 리스트를 정렬함

```python
>>> L = [3, 8, 2, 7, 6, 10, 9]
>>> L.sort()
>>> L
[2, 3, 6, 7, 8, 9, 10]
```


**정렬(Sort)**

문자열로 이루어진 리스트의 경우 정렬 순서는 사전 순서(알파벳 순서)를 따름. 

만약 문자열 길이 순서로 정렬하려면?
--> 정렬에 이용하는 `키(key)`를 지정

```python
>>> L = ['abcd', 'xyz', 'span']
>>> sorted(L, key=lambda x: len(x))
['xyz', 'abcd', 'span']
```

```python
>>> L = [{'name':'John', 'score':83},
		{'name':'Paul','score':92}]

# 점수가 높은 순으로 정렬
>>> L.sort(lambda x: x['score'], reverse=True)
```


**탐색(Search)**

탐색 알고리즘(1) - **선형 탐색(Linear Search)**

- 리스트의 길이에 비례하는 시간 소요 -> O(n)

- 최악의 경우 : 모든 원소를 다 비교해보아야함.

탐색 알고리즘(2) - **이진 탐색(Binary Search)**

- 탐색하려는 리스트가 이미 정렬되어 있는 경우에만 적용 가능

- 크기 순으로 정렬되어 있다는 성질 이용

예) 아래 리스트에서 30을 찾으시오

[1, 3, 7, 8, 12, 15, 17, 21, 24, 30, 32, 34, 39, 45, 51, 62]

```python
def solution(L, x):
    low = 0
    high = len(L)-1
    while low <= high:
        mid = (low+high)//2
        if L[mid] == x:
            return mid
        if L[mid] > x:
            high = mid -1
        else:
            low = mid +1
    return -1
```


한 번 비교가 일어날 때마다 리스트를 반씩 줄임 => **divide & conquer**
-> O(log *n*)




