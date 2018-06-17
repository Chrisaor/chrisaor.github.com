---
layout: post
title:  "[Tree] Binary Tree"
date:   2018-06-17 12:16:00 +0900
categories: datastructure
---

선형 데이터 구조인 배열, 링크드 리스트, 스택 그리고 큐와 달리 트리는 계층적 데이터 구조이다.

맨 위의 노드는 트리의 루트라고 한다. 원소의 바로 아래에 있는 원소를 자식이라고 부른다. 반대로 원소의 바로 위에 있는 원소는 부모라고 한다. 예를 들어 a는 f의 자식이고 f는 a의 부모다. 마지막으로 자식이 없는 원소를 리프라고 한다.

```
      tree
      ----
       j    <-- root
     /   \
    f      k  
  /   \      \
 a     h      z    <-- leaves  
```

 
 
#### 왜 트리인가?

1. 트리를 사용하는 한 가지 이유는 자연스럽게 계층을 형성하는 정보를 저장하기 위함이다. 예를 들어 컴퓨터의 파일 시스템이 그렇다.

2. BST와 같은 트리는 적절한 접근/검색을 제공한다. (배열보다는 느리고 링크드 리스트보다 빠르다)

3. 트리는 적절한 삽입/삭제 기능을 제공한다. (배열보다 빠르고 정렬되지 않은 링크드 리스트보다 느리다)

4. 링크드 리스트처럼, 배열과 다르게, 트리는 노드가 포인터를 사용하여 연결되므로 노드 수에 제한이 없다.

#### 트리의 주요 응용은 다음을 포함한다.

1. 계층 데이터 조작
2. 쉽게 정보를 검색하기
3. 정렬된 데이터를 조작하기
4. 시각 효과를 위한 디지털 이미지 합성을 위한 워크 플로우
5. 라우터 알고리즘
6. 다중 단계 의사 결정의 형태

#### 이진 트리(바이너리 트리)

자식이 2개 이하인 트리를 이진 트리라고 한다. 이진 트리의 각 원소는 2개의 자식만 가질 수 있으므로 일반적으로 왼쪽 자식, 오른족 자식이라고 부른다.

**트리의 노드들은 다음과 같은 구성을 갖는다.**

1. 데이터
2. 왼쪽 자식에 대한 포인터
3. 오른쪽 자식에 대한 포인터

```python
# 이진트리에서 각 노드를 나타내는 파이썬 클래스
class Node:
	def __init__(self, key):
		self.left = None
		self.right = None
		self.val = key
```

**간단한 첫 트리**

```python
class Node:
	def __init__(self, key):
		self.left = None
		self.right = None
		self.val = key

root = Node(1)

''' following is the tree after above statement
        1
      /   \
     None  None'''
     
root.left = Node(2)
root.right = Node(3)

''' 2 and 3 become left and right children of 1
           1
         /   \
        2      3
     /    \    /  \
   None None None None'''

root.left.left = Node(4)

'''4 becomes left child of 2
           1
       /       \
      2          3
    /   \       /  \
   4    None  None  None
  /  \
None None'''
```

**Summary** : 트리는 계층적 자료구조다. 트리의 주요 용도는 계층적 자료를 유지, 관리하고 적절한 접근 및 삽입/삭제 수행을 제공한다. 이진 트리는 모든 노드의 최대 두 명의 자식이 있는 특별한 케이스다.


