---
layout: post
title:  "[Stack] Array를 이용한 스택 구현"
date:   2018-06-09 12:15:00 +0900
categories: datastructure
---


```python
# array를 이용한 스택 구현

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