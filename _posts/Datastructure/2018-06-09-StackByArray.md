---
layout: post
title:  "[Stack] Array를 이용한 스택 구현"
date:   2018-06-09 12:15:00 +0900
categories: datastructure
---

Stack은 특정한 순서로 수행되는 선형 자료구조이다. 그 순서란 LIFO(Last In First Out) 또는  FILO(First In Last Out)을 말한다. 즉, 스티커 메모지를 겹쳐서 위로 덧붙였을 때 맨 위쪽부터 다시 하나씩 떼어지는 과정을 생각하면 된다.

다음 4가지 기본 작동원리가 적용된다.

- Push : 스택에 아이템을 추가한다. 스택이 꽉찼다면 overflow 상태가 된다.

- Pop : 스택으로부터 아이템을 제거한다. 추가된 순서와 반대로 맨 나중에 추가된 아이템부터 제거된다. 만약 스택이 비어있다면 underflow상태가 된다.

- Peek or Top : 스택의 꼭대기를 반환한다.

- isEmpty : 스택이 비어있다면 true, 아니라면 false를 반환한다.

**스택 동작의 시간복잡도 :**

push(), pop(), isEmpty() 그리고 peek()모두 O(1) 복잡도를 가진다.

**구현 :**

두 가지 구현 방법이 있다.

1. 배열(array)를 이용한다.
2. Linked list를 이용한다.

다음은 array를 이용한 스택을 파이썬으로 구현한 것이다.

```python
# Array를 이용한 스택 구현

from sys import maxsize
# sys module에서 maxsize import
# 스택이 비어있을 때 - 무한대를 리턴하기 위함

# 스택을 생성하는 함수. 스택의 사이즈는 0으로 초기화된다.
def createStack():
    stack = []
    return stack

# 스택의 사이즈가 0일 때 빈 스택이 된다.
def isEmpty(stack):
    return len(stack) == 0

# 스택에 아이템을 추가하는 기능. 추가되면 크기가 1씩 늘어난다.
def push(stack, item):
    stack.append(item)
    print("pushed to stack "+ item)

# 스택에서 아이템을 제거하는 기능, 크기가 1씩 감소한다.
def pop(stack):
    if (isEmpty(stack)):
        return str(-maxsize-1)

    return stack.pop()

stack = createStack()
push(stack, str(10))
push(stack, str(20))
push(stack, str(30))
print(pop(stack) + " popped from stack")
print(pop(stack) + " popped from stack")
print(pop(stack) + " popped from stack")
```

**결과**
 
```
pushed to stack 10
pushed to stack 20
pushed to stack 30
30 popped from stack
20 popped from stack
10 popped from stack
```