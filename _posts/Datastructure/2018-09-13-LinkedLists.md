---
layout: post
title:  "[Linked Lists] Linked Lists (1)"
date:   2018-09-13 12:20:00 +0900
categories: datastructure
---


## 추상적 자료구조 (Abstract Data Structures)

- Data
 - 예) 정수, 문자열, 레코드, ...
 
- A set of operations
 - 삽입, 삭제, 순회, ...
 - 정렬 탐색, ...


## 연결 리스트(Linked Lists)

데이터 원소들을 순서를 지어 늘어놓는다는 점에서 연결 리스트 (linked list) 는 선형 배열 (linear array) 과 비슷한 면이 있지만, 데이터 원소들을 늘어놓는 방식에서 이 두 가지는 큰 차이가 있다. 구체적으로는, 선형 배열이 번호가 붙여진 칸에 원소들을 채워넣는 방식이라고 한다면, 연결 리스트는 각 원소들을 줄줄이 엮어서 관리하는 방식.

### 선형 배열에 비해서 연결리스트가 가지는 이점

연결 리스트에서는 원소들이 링크 (link) 라고 부르는 고리로 연결되어 있으므로, **가운데에서 끊어 하나를 삭제하거나, 아니면 가운데를 끊고 그 자리에 다른 원소를 (원소들을) 삽입하는 것이 선형 배열의 경우보다 쉽다.** 여기에서 쉽다 라고 표현한 것은, 빠른 시간 내에 처리할 수 있다는 뜻. 이러한 이점 때문에, **원소의 삽입/삭제가 빈번히 일어나는 응용에서는 연결 리스트가 많이 이용된다.** 컴퓨터 시스템을 구성하는 중요한 요소인 운영체제 (operating system) 의 내부에서도 이러한 연결 리스트가 여러 곳에서 이용되고 있다.

### 연결 리스트가 선형 배열에 비해서 가지는 단점

생각하기 쉬운 하나의 단점은, **선형 배열에 비해서 데이터 구조 표현에 소요되는 저장 공간 (메모리) 소요가 크다는 점이다.** 링크 또한 메모리에 저장되어 있어야 하므로, 연결 리스트를 표현하기 위해서는 동일한 데이터 원소들을 담기 위하여 사용하는 메모리 요구량이 더 크다. 그보다 더 우리의 관심을 끄는 단점은, **k 번째의 원소 를 찾아가는 데에는 선형 배열의 경우보다 시간이 오래 걸린다는 점이다.** 선형 배열에서는 데이터 원소들이 번호가 붙여진 칸들에 들어 있으므로 그 번호를 이용하여 대번에 특정 번째의 원소를 찾아갈 수 있는 데 비하여, 연결 리스트에서는 단지 원소들이 고리로 연결된 모습을 하고 있으므로 특정 번째의 원소를 접근하려면 앞에서부터 하나씩 링크를 따라가면서 찾아가야 한다.

### 자료 구조 정의

1. Node
 - Data
 - Link(next)


```python
class Node:
	def __init__(self, item):
		self.data = item
		self.next = None
```


2. Linked List


```python
class LinkedList:
	def __init__(self):
		self.nodeCount = 0
		self.head = None
		self.tail = None
```

### 연산 정의

1. 특정 원소 참조 (k 번째)
2. 리스트 순회
3. 길이 얻어내기
4. 원소 삽입
5. 원소 삭제
6. 두 리스트 합치기

#### 1. 특정 원소 참조

```python
def getAt(self, pos):
	if pos <= 0 or pos > self.nodeCount:
		return None
	i = 1
	curr = self.head
	while i < pos:
		curr = curr.next
		i += 1
	return curr
```

	
#### 실습. traverse LinkedList


```python
class Node:
    def __init__(self, item):
        self.data = item
        self.next = None

class LinkedList:
    def __init__(self):
        self.nodeCount = 0
        self.head = None
        self.tail = None

    def getAt(self, pos):
        if pos < 1 or pos > self.nodeCount:
            return None
        i = 1
        curr = self.head
        while i < pos:
            curr = curr.next
            i += 1
        return curr

    def traverse(self):
        result = list()
        curr = self.head
        while curr != None:
            result.append(curr.data)
            curr = curr.next
        return result


# 이 solution 함수는 그대로 두어야 합니다.
def solution(x):
    return 0


linkedlist = LinkedList()
a = Node(35)
b = Node(65)
c = Node(13)
d = Node(95)
a.next = b
b.next = c
c.next = d

linkedlist.head = a
print(linkedlist.traverse())
```

