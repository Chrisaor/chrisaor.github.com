---
layout: post
title:  "[Level2] 두 정수 사이의 합"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

adder함수는 정수 a, b를 매개변수로 입력받습니다.
두 수와 두 수 사이에 있는 모든 정수를 더해서 리턴하도록 함수를 완성하세요. a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
예를들어 a가 3, b가 5이면 12를 리턴하면 됩니다.

a, b는 음수나 0, 양수일 수 있으며 둘의 대소 관계도 정해져 있지 않습니다.

```python
def adder(a, b):
    result = 0
    if a == b:
        return a
    else:
        for i in range(min(a,b), max(a,b)+1):
            result += i
        return result
print(adder(3, 5))
```