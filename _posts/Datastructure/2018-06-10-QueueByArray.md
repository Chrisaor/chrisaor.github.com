---
layout: post
title:  "[Queue] Array를 이용한 큐 구현"
date:   2018-06-10 12:16:00 +0900
categories: datastructure
---

스택처럼 큐는 특정한 순서로 동적하는 선형 자료구조이다. 순서는 FIFO(First In First Out). 가장 좋은 예는 마트에서 고객들이 줄을 선 순서대로 계산을 하고 나가는 것이다. 스택과 다른 점은 제거될 때이다. 스택에서는 가장 최근에 추가된 아이템이 제거되지만 큐에서는 가장 오래된 아이템이 먼저 제거된다. 

**큐에서의 동작**

**Enqueue :** 큐에 아이템을 추가한다. 큐에 꽉 차있다면 overflow 상태.

**Dequeue :** 큐에서 아이템을 제거한다. 아이템이 추가된 순서로 pop된다. 만약에 큐가 비어있으면 underflow 상태이다.

**Front :** 큐에서 제일 맨 앞의 아이템을 얻는다.

**Rear :** 큐에서 제일 맨 마지막의 아이템을 얻는다.



```python
class Queue:
	
    def __init__(self, capacity):
        self.front = self.size = 0
        self.rear = capacity - 1
        self.Q = [None] * capacity
        self.capacity = capacity

    def isFull(self):
        return self.size == self.capacity

    def isEmpty(self):
        return self.size == 0

    def EnQueue(self, item):
        if self.isFull():
            print("Full")
            return
        self.rear = (self.rear + 1) % (self.capacity)
        self.Q[self.rear] = item
        self.size = self.size + 1
        print("%s euqueued to queue" %str(item))

    def DeQueue(self):
        if self.isEmpty():
            print("Empty")
            return

        print("%s dequeued from queue" %str(self.Q[self.front]))
        self.front = (self.front + 1) % (self.capacity)
        self.size = self.size - 1

    def que_front(self):
        if self.isEmpty():
            print("Queue is empty")
        print("Front item is", self.Q[self.front])

    def que_front(self):
        if self.isEmpty():
            print("Queue is empty")
        print("Front item is", self.Q[self.front])

    def que_rear(self):
        if self.isEmpty():
            print("Queue is empty")
        print("Rear item is", self.Q[self.rear])


if __name__ == '__main__':
    queue = Queue(30)
    queue.EnQueue(10)
    queue.EnQueue(20)
    queue.EnQueue(30)
    queue.EnQueue(40)
    queue.DeQueue()
    queue.que_front()
    queue.que_rear()
```