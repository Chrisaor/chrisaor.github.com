---
layout: post
title:  "[Sort] 선택 정렬(Selection Sort)"
date:   2018-05-23 12:15:00 +0900
categories: Algorithm
---


선택 정렬은 주어진 배열에서 최솟값(최댓값)을 찾아 맨 왼쪽(오른쪽) 값과 교체한다. 최대값을 찾아야 하므로 정렬 상태와 관계없이 언제나 O(n<sup>2</sup>)의 시간복잡도를 가진다.


- 내가 짠 코드

```python
def selection_sort(list1):
    for i in range(len(list1)):
        minimum = list1[i]
                for j in range(i,len(list1)):
            if j+1 == len(list1):
                continue
            elif minimum >= list1[j+1]:
                minimum = list1[j+1]
                min_index = j+1
        list1[min_index], list1[i] = list1[i], list1[min_index]
        min_index = i+1
    return list1

list1 = [9,1,6,8,4,3,2,0,5,7]
print(selection_sort(list1))
```

- [blog](http://ejklike.github.io/2017/03/04/sorting-algorithms-with-python.html)

```python
def selectionSort(x):
    for size in reversed(range(len(x))):
        max_i = 0
        for i in range(1, 1+size):
            if x[i] > x[max_i]:
                max_i = i
        x[max_i], x[size] = x[size], x[max_i]
        
x = [9,1,6,8,4,3,2,0,5,7]
print(selectionSort(x))  
```

- hello coding 그림으로 개념을 이해하는 알고리즘

```python
def findSmallest(arr):
	smallest = arr[0]
	smallest_index = 0
	for i in range(1, len(arr)):
		if arr[i] < smallest:
			smallest = arr[i]
			smallest_index = i
	return smallest_index

def selectionSort(arr):
	newArr = []
	for i in range(len(arr)):
		smallest = findSmallest(arr)
		newArr.append(arr.pop(smallest))
	return newArr
	
arr = [9,1,6,8,4,3,2,0,5,7]	
print(selectionSort(arr))
```
