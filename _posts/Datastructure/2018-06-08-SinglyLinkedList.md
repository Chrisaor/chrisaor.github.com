---
layout: post
title:  "[Linked List] Singly Linked List"
date:   2018-06-08 12:15:00 +0900
categories: datastructure
---

`Array`처럼 `linked list`는 선형 자료구조를 갖는다. `Array`와 다른 점은 `linked list`의 원소들은 인접한 공간에 저장되지 않고 pointer를 사용해 각 각의 원소를 연결한다.

`Linked list`의 장점을 설명하기 전에 `array`의 한계를 먼저 말하면

1) `Array`의 크기는 고정이다. 애초에 `array`의 크기를 결정해야하기 때문에 원소의 수를 미리 알고 있어야한다. 할당된 메모리는 원소가 비어있는 것과 상관없이 사용된다.

2) `Array`에 새로운 원소를 추가하는 비용이 많이 든다. 새로운 원소에 대해 공간을 생성해야하는데 만약 그 공간에 기존 원소가 이미 존재한다면 `array`의 원소를 전부 이동시켜야하기 때문이다.

이러한 `array`의 한계는 `linked list`의 장점을 부각시킨다.

1) 쉽게 추가/삭제를 가능하게 한다.  
2) 동적으로 크기를 바꿀 수 있다.

다음은 파이썬으로 구현한 `single linked list`다.

원소를 추가하는 3가지 방법 `push`(앞쪽에 추가), `insertAfter`(원소 뒤에 추가), `append`(맨 뒤에 추가)을 파이썬으로 구현하면 다음과 같다.

```
# 노드 클래스
class Node:
    def __init__(self, data):
        # data를 입력
        self.data = data
        # 다음 노드를 가리키는 속성
        self.next = None

# 링크드 리스트 클래스
class LinkedList:
    def __init__(self):
        self.head = None

    def push(self, new_data):
        # 1. 새로운 노드 생성
        # 2. 데이터 삽입
        new_node = Node(new_data)
        # 3. 새로운 노드의 next는 현재 헤드로 설정 한다.
        new_node.next = self.head
        # 4. 새로운 노드는 linked list의 헤드가 된다.
        self.head = new_node

    def insertAfter(self, prev_node, new_data):

        # 1. prev_node가 존재하는지 체크한다.
        if prev_node is None:
            print("주어진 노드는 linked list 내부에 존재해야합니다.")
            return
        # 2. 새로운 노드를 생성
        # 3. 데이터 삽입
        new_node = Node(new_data)

        # 4. 새로운 노드의 next는 이전 노드의 next로 바꾼다.
        new_node.next = prev_node.next

        # 5. 이전 노드의 next는 새로운 노드를 가리킨다.
        prev_node.next = new_node

    def append(self, new_data):
        # 1. 새로운 노드 생성
        # 2. 데이터 삽입
        # 3. Next는 None
        new_node = Node(new_data)
        # 4. 헤더가 비어있다면 새로운 노드가 헤더
        if self.head is None:
            self.head = new_node
            return
        # 5. last까지 도달한다.
        last = self.head
        while (last.next):
            last = last.next

        # 6. last의 next는 새로운 노드로
        last.next = new_node

        # 새로운 노드는 linked list의 last가 된다.

    def printList(self):
        temp = self.head
        while temp:
            print(temp.data)
            temp = temp.next

llist = LinkedList()
A1 = Node(1)
llist.head = A1
llist.append(2)
llist.append(3)
llist.append(4)
llist.insertAfter(A1, 5)
llist.push(6)
llist.printList()
```

**결과**

HEAD  
[6] -> [1] -> [5] -> [2] -> [3] -> [4]

