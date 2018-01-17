---
layout: post
title:  "[Level2] 자연수를 뒤집어 리스트로 만들기"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

digit_reverse함수는 양의 정수 n을 매개변수로 입력받습니다.
n을 뒤집어 숫자 하나하나를 list로 표현해주세요
예를들어 n이 12345이면 [5,4,3,2,1]을 리턴하면 됩니다.

```python
def digit_reverse(n):
    x = str(n)
    result = []
    for i in range(len(x)):
        result.append(int(x[i]))
    result.reverse()
    return result

# 아래는 테스트로 출력해 보기 위한 코드입니다.
print("결과 : {}".format(digit_reverse(12345)));
```
